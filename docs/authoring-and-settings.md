# Administrator screens, fields and ownership

This is the proposed authoring contract for the feature. Labels are examples for the English language file. Reference existing Joomla/JCB conventions for controls, access checks and language associations. Every form opens through generated Admin Views or a deliberately attached Custom Admin View. Default Mux selection never provisions an account or starts billing.

## Connection form

| Control | Stored meaning | Validation and interaction |
| --- | --- | --- |
| Church, provider, label | Existing church GUID, stable adapter key, human name | Required. Church selector limited to authorized records. Duplicate names across churches are safe. |
| Setup mode | Adopt existing, create new, embed/link only | Default adopt existing. Create becomes available only for capable providers with consent and write permission. |
| Existing URL/ID | Provider account/resource kind, canonical ID and viewing URL | Preview derived provider and host. Reject encoder URL in viewer field. Changing a URL to another account/resource requires remapping, rather than silently rewriting prior events. |
| Credentials | Server-side secret reference and scopes | Show controls only if needed by operation. Validate through read-only discovery. Hidden conditional fields never block saving link-only mode. |
| Audience | Public, members, private invitation or restricted provider | Match Joomla view levels and provider capabilities; give explicit warning/actions when a public external link cannot enforce members-only access. |
| Website playback | Auto within approved choices, church player, provider player, external action | Test as anonymous/member, desktop/mobile. An admin-only preview is insufficient for anonymous playback. |
| Availability | Owner API/status source, signed callback, or operator | Store method and freshness policy separately from the event's scheduled time. |
| Automation | Capabilities requested and successfully verified | Separate buttons for connect, re-check, adopt, rotate, revoke and remove mapping. Display last success and outstanding limitations. |
| Authorized hosts/callback | Provider-specified/approved exact origins and generated callback route | Show the callback URL and verification state to admin only. Do not expose signed query strings when copying a public page URL. |

A change of provider creates or selects a different connection. Already published occurrences keep their recorded provider/resource mappings. Changing a church default affects future unsnapshotted settings; an operator chooses whether to apply it to existing future occurrences with a preview of affected resources.

## Programme and occurrence editor

| Group | Fields | Behaviour |
| --- | --- | --- |
| Identity | Title, church, main preacher, optional series/category, language, access | Use native GUID selectors and access. A guest preacher warning allows explicit approval; absence of required final attribution holds archiving. |
| Programme timing | Named zone, local date/time, duration, weekdays, repeat interval, starts on, end type/count/date | Show readable rule and date preview. Support Wednesday/Friday/Sunday with independent times and different rooms through separate programmes. |
| Occurrence exceptions | New date/time, substitute preacher/church when authorized, cancellation reason, revised title | Offer this date, this and future, or series scope. Show which remote events/keys are affected and whether the provider can represent that scope. |
| Destinations | Church connection/resource, primary or alternate, language/audio availability, published public action, owner of calendar | Include already connected account resources. Preview public destination after sanitization; do not show ingest details to website visitors. |
| Live policy | Rehearsal, automatic detection, permitted manual control, grace, expected duration | Operator-only start/end actions identify the occurrence. A rehearsal defaults to hidden from public lists and auto-sermon creation. |
| Replay policy | Live only, provider only, local backup or locally canonical; audio/video/both; preferred source and permitted fallback | On incompatible providers offer original-file ingestion. Show provider-imposed recording or public exposure before saving. |
| Publication | Hold for review, auto-publish with complete required fields, deliberately exclude | Keep an audit trail. Editor decision overrides automation retries. |
| Integration results | Local UID, connection name, sync state/last readback, chronology, file verification and sermon link | Identifiers shown to operators only as needed. Offers specific repair actions without logging secrets. |

An editor who owns only the Friday programme may not control the Wednesday input even if both use the same provider. A hidden/unpublished preacher can be selected by an authorized editor; before public release resolve whether the name, page and link may actually be shown. If only the name is embargoed, use the configured generic wording and do not reveal a hidden alias.

## Dashboard and operations

* **Upcoming:** date/time in church zone, room/programme, destination, preacher, sync status and next action. Filter by ownership, language, unresolved conflicts and timespan.
* **Live desk:** current services, actual signal, playback readiness, private rehearsal view, external fallback, distinct end-local/end-upstream actions. Show which action affects a remote broadcast and require separate permission.
* **Archive inbox:** session, source, recording status, remaining provider retention, transfer progress, local file validation, linked sermon and unresolved metadata. Allow attach original, retry, replace selected source, link existing sermon and approve.
* **Connections:** account identity, capabilities, last test, scheduler heartbeat, credential expiry where known, pending jobs and storage. A church sees only its own accounts. A successful playback check must not mark a failed export permission healthy.
* **History:** show schedule changes, switch of live source, operator confirmations, callback/poll outcomes and archive lineage. Provide a redacted receipt for support.
* **Empty state:** first-run install without providers continues serving all historic sermons. Add a menu/module before scheduling if desired; it displays a localized “No upcoming services” message without requesting credentials or contacting any cloud service.

## Configuration precedence

Apply site policy (permitted providers, data/privacy rules and host limits), then church defaults, programme defaults, occurrence exceptions and per-destination capability limits. A lower level can narrow a permission. It cannot broaden a site prohibition or add a provider-side entitlement. Snapshot archive/ownership settings for each emitted occurrence; an explicit future-series edit updates selected rows with revision history. A module/menu may select a different presentation filter or timezone label, but cannot grant access to data or override retention.

## Source migrations

To move a church from an existing YouTube broadcast to Mux, add Mux as a new connection and rehearse on a test occurrence. Keep the old destination's public link for published historic services. Select Mux as primary only for the chosen future scope. Preserve the church's menu alias and all sermon URLs. A church using a single audio mount may add a video alternative years later without rebuilding its archive or changing the audio listen action.
