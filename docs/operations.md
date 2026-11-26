# Provider orchestration, state and recovery

## Entry points and ownership

| Entry point | Actor and boundary | Durable result |
| --- | --- | --- |
| Save connection / test | Church administrator, ACL + Joomla form/request token | Validated account namespace, capability evidence, credential reference; test performs no remote mutation |
| Save programme or occurrence | Schedule editor with authority over selected church, fields and destinations | Versioned local state, any exceptions, mapping changes and outbox entries in a single save boundary |
| Provider callback | Provider or authenticated relay; raw bytes and transport-specific validation | Verified event receipt, unique deduplication key, quick acknowledgement following durable storage |
| State poll / reconciliation | Scoped task identity; rate-limited per connection | Current observation with provider resource/session ID, original timestamp, observed time and expiry |
| Public status descriptor | Anonymous/member according to occurrence-level access | Sanitized state, checked time, timing, legal viewing options. No remote write, credential, private path or synchronous provider API call per viewer. |
| Recording worker | Background identity scoped to owner, authorized source and destination | Durable transfer/checkpoints, final verified file mapping or actionable retry state |
| Archive / publish | Existing sermon model under appropriate create/edit/state authority | One association and complete metadata, with manual intervention available for exceptions |
| Rotate/revoke/delete | Separate elevated action; require selected resource and explicit confirmation | Audit result, dependent resources invalidated or queued for review, local sermons preserved |

## Keep four independent states

An occurrence can be scheduled yet its encoder already connected for a rehearsal. A broadcast can be ended while its local archive is still downloading. A sermon can be unpublished while the church operator can preview the finished file.

| State dimension | Values to represent | Source of truth |
| --- | --- | --- |
| Editorial | draft, published, cancelled, archived | Joomla editor and scope |
| Schedule sync, per destination | local only, pending, synchronized, conflict, authorization required, remote absent, failed | Current desired revision and last confirmed remote projection |
| Broadcast, per session | unknown, idle, connecting, live, reconnecting, ended, failed | Fresh provider/owner signal and media readiness |
| Archive, per rendition | disabled, awaiting completion, requested, preparing, transferring, verifying, verified, failed, expired, removed | Export operation, worker and file store |
| Sermon publication | none, held for assignment/review, linked draft, published, intentionally excluded | Existing component's editorial workflow and association |

### Reconciliation rules

1. Normalize provider event ID, tenant/resource kind, active session ID, event time and receipt time. An active event from one session outranks an old disconnected event from a different session. Unknown event types are stored minimally for diagnosis and do not trigger a guessed side effect.
2. Compare with last confirmed observation for **that session**. On version/timestamp ambiguity call the authorized provider read operation. A later delivery timestamp alone does not make an older event authoritative. An authoritative end is terminal for that session, except explicit operator correction with evidence.
3. Give observations an expiry. Suggested starting policy: refresh active or soon-due resources every 30–60 seconds within quota, idle channels less often; mark provider status stale after two missed successful refreshes plus transport jitter. A callback-only integration still needs a way to retire stale state. Cache viewer requests against the last local observation.
4. An operator with live-control permission may explicitly mark a specific occurrence live/ended when provider evidence is absent. Record actor, reason, start/end, expiry and scope; display manual status as such in administration. After expiry fall back to valid provider evidence or unknown. Never implicitly mark an occurrence live from a nonempty stream URL, reusable playlist, viewer count or scheduled start.
5. Decide public summary from eligible destinations only. A primary outage with a verified live alternate offers that alternate according to configured priority. An unauthorized/private alternate cannot make a hidden preacher publicly live. A genuinely live but link-only Telegram service can show a live indicator if confirmed by an authorized operator or provider.
6. Reconciliation after downtime works back from the last checkpoint with overlap, deduplicates provider events, pages to completion and records the high-water mark only after durable processing. Backfill completed recordings into the correct historic occurrence before updating public badges.

## Task runner and backpressure

A Joomla scheduler task, an authorized CLI worker, or the target installation's existing queue may drive the same durable job interface. Standard shared hosting must support the provider player/link and local calendar without a resident media server. Large files may require a scheduled CLI worker or chunked short-lived tasks. Show last scheduler heartbeat, oldest due job, stuck transfer age, available disk and stale providers in administration. “No jobs have run” is an actionable state even when the site receives no visitors.

Order jobs by time sensitivity: confirm imminent/live events, persist webhook receipts and schedule updates, protect expiring recording export, then routine media transfer and background reconciliation. Limit concurrency per account and storage target. Reserve a lease atomically before external IO; renew it for long downloads. Releases and retries carry a fencing version so an expired worker cannot overwrite the work of a replacement. Temporary failures retry with increasing bounded delay and jitter. Honor Retry-After and provider-specific wait intervals. Limit an individual task's wall time and bytes; do not hold a DB transaction across HTTP/media IO.

| External outcome | Treatment | Operator message/action |
| --- | --- | --- |
| 2xx create, response lost | Uncertain write; query by ID/correlation, compare owner/fields, adopt unique match | “Checking whether the provider created this service.” If ambiguous, choose existing remote event. |
| 400 / validation | Save structured reason, stop retries for same revision | Identify field/rule; corrected revision enqueues a new attempt. |
| 401 / expired credential | Refresh only once under lock where supported; otherwise suspend authenticated actions | Reconnect same account; independent public viewing remains available if permitted. |
| 403 / plan / privacy | Mark operation unavailable for this connection; never broaden privacy on retry | Show capability, grant or entitlement needed; preserve other operations. |
| 404 / remote unknown | Distinguish wrong region/credentials and deletion from provider readiness delay | Reconcile using mapped identity. Ask before replacement of externally owned event. |
| 409 / revision mismatch | Read current state and compare ancestry | Show per-field differences; publish no hidden conflict. |
| 429 / throttle | Respect provider delay and fair scheduling across churches | Show delayed sync and upcoming risk; no flood of repeated admin actions. |
| 5xx, timeout, outage | Bounded retry and circuit pause per connection; do not stop other churches | Show last success and backup action, recheck selectively. |
| Local disk full / file invalid | Keep original provider source, clean only known temporary files, suspend transfer | Show bytes needed and options to change destination, retry, or use provider playback. |
| Worker lost after save | Lease expiry/recovery, adopt finalized data by durable marker | Duplicate delivery produces one event/sermon/file. |

## Callback acceptance

Use a named endpoint per provider and a locally registered connection binding. Opaque installation identifiers are routing hints; signature/credential and remote account/resource establish authority. Bound raw body size, read raw bytes once, check the provider-prescribed replay window and compare signatures in constant time. Use the callback transport's exact challenge/verification response. A provider webhook does not carry a Joomla browser CSRF token. For a provider without verifiable push, treat the notification as a rate-limited hint and confirm via its authenticated resource API before changes. See each provider profile for the signing method; the same signature header must not be assumed across vendors.

After authentication, persist and deduplicate before acknowledgement. Honour verification probes separately. Use asynchronous processing with at-least-once delivery assumptions. On duplicate receipt return the correct provider acknowledgement without replaying side effects. Keep deletion tombstones and archive mappings longer than the retry window; redact or expire raw inputs early. Re-deliver from the inbox under an audited operator action, never by disabling signature checking.

## Restore and operational support

Pause writes before restoring a site snapshot. Rebind tokens to the correct provider account and region, verify local files by digest and restore ACLs, reconcile outstanding remote event IDs, clear expired worker leases and rotate any credentials included in an exposed backup. Do not allow staging and production clones to control the same provider resources by default. A read-only rehearsal install may import events and use recorded fixtures without issuing writes.

Support receipt contains Joomla/JCB version, church-safe correlation ID, sanitized provider/resource kind, desired/observed revision, last successful observation, operation result, local file size/checksum and current queue state. Exclude credentials, signed links, provider raw headers, private invites and real sermon media. Deactivate a malfunctioning connector independently of other churches, schedules, media and sermons.

Sources: [Joomla scheduled tasks](https://guide.joomla.org/user-guide/scheduled-tasks), [Mux webhook delivery](https://www.mux.com/docs/core/listen-for-webhooks), [Twitch EventSub handling](https://dev.twitch.tv/docs/eventsub/handling-webhook-events), [Amazon IVS EventBridge delivery](https://docs.aws.amazon.com/ivs/latest/LowLatencyUserGuide/eventbridge.html).
