# Cross-provider acceptance cases

These are release criteria for the future implementation and are not recorded as passing runtime tests in this PR. Build synthetic churches North and East with distinct accounts, a guest preacher, unrelated existing sermon/file, registered/public access levels and two simultaneous Sunday rooms. Use disposable provider resources for mutation tests. For each case retain initial identities, actual action, redacted event timestamps, resulting row/file counts and visible result. Exercise through Joomla's normal entry points.

## Install and entitlement

| ID | Scenario | Arrange and act | Required result |
| --- | --- | --- | --- |
| EX01 | Upgrade a populated installation | Install the previous blueprint, create a sermon/file/menu, compile and upgrade the feature build. | The existing sermon URL, preacher link, file ownership, download ACL and menu continue to work. No account is created remotely. |
| EX02 | Start on shared PHP hosting | Provide no resident worker, FFmpeg or cloud token. Add an external-link occurrence. | The church can publish and view a countdown plus external action. Unsupported recording controls identify what the host lacks. |
| EX03 | Connect Mux with read access only | Import an existing live stream and open the setup screen as the assigned church admin. | Discovery succeeds, create/reset actions require additional permission, and OBS configuration is unchanged. |
| EX04 | Tenant mismatch | Replace credentials with a valid token for another provider environment while an occurrence is mapped. | Stop authenticated writes pending rebinding. Publicly eligible existing playback and verified local files keep their independent policy. |
| EX05 | Rehearsal clone | Restore production DB and files into a staging install, then run its scheduler. | The restored copy does not mutate production resources until explicitly rebound. Local historic playback can be validated. |
| EX06 | Missing callback endpoint | Block public callback reachability while enabling read-only provider polling. | Last authenticated poll appears in diagnostics. The site never registers a nonfunctional callback as healthy. |
| EX07 | Two admins edit the same programme | Save a changed church/room then submit an older form that changes only title. | Detect version conflict. Preserve original routing or permit deliberate field merge; never switch another church silently. |
| EX08 | Account without Live API entitlement | Adopt a permitted Vimeo event iframe for an account without eligible API access. | Website player and local calendar remain usable. Remote scheduling/export controls state actual entitlement. |
| EX09 | Rotate a stream key | Reopen a connection and run read-only Test, then explicitly rotate using an assigned operator. | Test never rotates; explicit action warns about active encoders and yields a redacted audit entry. |
| EX10 | Independent church accounts | Downgrade or disconnect North's account while East is broadcasting on the same vendor. | East's jobs, playback, clock and credentials are unaffected. North's local recordings remain usable. |

## Calendar and synchronization

| ID | Scenario | Arrange and act | Required result |
| --- | --- | --- | --- |
| CA01 | Weekly chain | Create Wednesday 18:30, Friday 19:00 and Sunday 09:00 Windhoek programmes. Run the scheduler twice. | Each intended date exists once. Next rotates in chronological order, all-upcoming lists all, and pinned keeps its identity. |
| CA02 | Due time with no camera | Advance time to scheduled start with the encoder idle. | Waiting appears. No live badge, infrastructure start, automatic sermon or remote go-live call is produced. |
| CA03 | Preacher substitution | Change one Sunday from Ruth to Paul, then edit next month's programme default. | Only the selected occurrence and its future eligible scope change. That Sunday's live badge and eventual sermon use Paul. |
| CA04 | Cancelled exception before generation | Cancel Friday beyond the current horizon; run generation until that date enters the horizon. | Tombstone survives. No new visible Friday/remote duplicate and next countdown selects Sunday. |
| CA05 | Move a published instance | Change one Wednesday to Thursday after a menu/share link exists. | Original occurrence GUID/route persists; time and weekday update; later Wednesdays retain original rule. |
| CA06 | DST gap and fold | Generate Sunday 02:30 Europe/Berlin through 2026-03-29 and 2026-10-25. | Spring invalid instance is skipped with preview explanation; autumn fold has one defined first occurrence unless a deliberate exception specifies another instant. |
| CA07 | Viewer timezone changes | Open the same event from Windhoek and a device in another timezone; suspend one browser then resume. | Both show the same remaining duration and correct absolute local display. Resumed countdown corrects itself without negative drift. |
| CA08 | Two simultaneous programmes | Schedule two rooms at 09:00 with distinct inputs; later attempt to reuse the same single-source input in both. | Independent inputs may overlap. Flag the ambiguous shared input before automatic live or sermon attribution. |
| CA09 | Provider calendar adoption | Import an already scheduled broadcast twice through two paginated refreshes. | Both imports resolve to one mapped occurrence. The create and key-reset counters stay zero. |
| CA10 | Lost remote creation reply | Provider creates the event but times out. Run two sync workers and edit local title during recovery. | Resolve or park the uncertain create before any second write; one verified event remains and latest desired title is applied once. |
| CA11 | Edit conflict | Last sync stored 09:00. Joomla changes to 10:00 while provider changes to 11:00. | Show all three instants and affected field; preserve independently edited preacher metadata. No hidden overwrite. |
| CA12 | One-off unsupported upstream | Try a single Friday exception on a provider operation that changes a whole recurring series. | Show scope divergence. No mass overwrite/delete. Local exception remains visible with sync status. |
| CA13 | Provider write quota | Throttle the second of three future-event writes. | First event remains mapped; third waits/runs within quota; refused work is retried with provider delay. No duplicate Friday. |
| CA14 | Cancelled upstream event | External owner cancels a mapped service while the local connection is offline. | Follow mode imports cancellation when reconnected, preserves route and history, and stops countdown. It does not recreate upstream. |
| CA15 | ICS partial fetch | Return a truncated feed omitting previously known Sunday. Then return an explicit cancellation. | First refresh retains last-good data. Explicit cancellation for correct UID/recurrence ID changes only that event. |
| CA16 | No future services | End all programmes and hide inaccessible upcoming services. | Calendar and module have localized empty state. No other church's private time/title appears. |

## Signal, playback and navigation

| ID | Scenario | Arrange and act | Required result |
| --- | --- | --- | --- |
| PL01 | Encoder connects without readiness | Send connected signal before a playable state. | Starting is shown. Live status appears only on provider/authorized operator confirmation of the correct occurrence. |
| PL02 | Old disconnect after new live | Deliver a disconnect from the previous week after the next session becomes active. | Ignore or record historic signal and reconcile. Current preacher/room remains live. |
| PL03 | Concurrent destination outage | Primary provider fails while a verified alternate is live. | Show the permitted alternate and identify the destination. No silent redirect, duplicate audio or hidden-provider exposure. |
| PL04 | Blocked YouTube network | Enable No YouTube site policy and visit home, preacher and service pages. | No YouTube frames, scripts, thumbnails, DNS preconnect or idle player loads. Authorized local audio remains usable. |
| PL05 | Unembeddable social event | Serve an official embed restriction or unsupported viewer condition. | Present configured public action with the platform name. Avoid a blank player and preserve the local service URL. |
| PL06 | Link-only private church | Open its Telegram service as guest then member. | Guest cannot extract invite from HTML, JSON, feeds, links, cache or audit. Authorized member can follow the exact listening invite. |
| PL07 | Multiple modules on same page | Place next-only, all-upcoming and pinned modules plus preacher live badge on one page. | Independent filters and hooks, no ID collisions, one set of necessary assets. Countdowns don't each poll the vendor. |
| PL08 | SEF and language aliases | Visit through a church menu, another language, non-SEF URL and Joomla installed in a subdirectory. | Links resolve to intended occurrence/sermon subject to ACL. Active menu/language is retained where appropriate. |
| PL09 | Phone on low data | Select Listen live before initializing video on a narrow mobile viewport. | Only a real audio source is requested. Source switch stops prior audio; controls and labels remain operable. |
| PL10 | Status cache leakage | View a restricted live badge while logged in then load same URL anonymously from a cached page. | No leaked title, image, speaker alias, link or signed token. Personalized status is not reused across audiences. |
| PL11 | Member revoked during replay | Expire a signed playback token and revoke membership before refresh. | Refresh is denied. Publisher token/account secret never reached the client. Public alternative is offered only when policy permits it. |
| PL12 | Grace/overrun | Service is 20 minutes late then starts while next event is visible; another runs past expected end. | Delayed occurrence can become live; overrun remains confirmed live until actual end. Upcoming list still shows later service. |
| PL13 | Uncertain source | A 24/7 audio mount stays up between services and an encoder transmits a blank slate. | Neither signal alone marks the scheduled preacher live. Explicit attribution/operator action is available. |
| PL14 | Keyboard/caption workflow | Navigate without a mouse, switch to captions or alternate audio, dismiss embedded player. | Controls have names/focus, keyboard exits fullscreen, captions survive source selection when provided and countdown announcements remain quiet. |

## Recordings and sermons

| ID | Scenario | Arrange and act | Required result |
| --- | --- | --- | --- |
| AR01 | Audio-first replay | Two copies of the same ready signal arrive; audio becomes ready ahead of video. | One primary sermon, one verified audio attachment, one verified video attachment when complete, correct church and preacher. |
| AR02 | Provider playback before end | Worker receives asset-ready while source is still broadcasting. | Live player remains active. No completed local archive or premature sermon publication until finalization conditions hold. |
| AR03 | Provider recording impossible | Use a link-only provider and choose attach church original. | File is verified and attached to the same automatically created or existing mapped sermon; no stream scraping takes place. |
| AR04 | Transfer exceeds quota | Stop halfway through MP4 with disk full and retry after increasing capacity. | No partial public file. Original retained. Reconcile and resume only when byte ranges and source validators permit. |
| AR05 | Expired download URL | Expire the temporary source URL during retry. | Refresh under the same asset/rendition ID. Never save the temporary URL as permanent replay or download from another asset. |
| AR06 | Corrupt response | A file endpoint returns a successful HTML response, zero bytes or truncated video. | Quarantine/reject, set actionable failure, never attach or publish it. |
| AR07 | Crash after promotion | File was moved into final storage before worker lost its database connection. | Recover known path by operation/asset identity and verify bytes. One managed File row and one sermon mapping. |
| AR08 | Crash after sermon saved | Kill the job after normal sermon model save but before recording the final job status. | Find existing reserved identity/creation marker and complete association. One Joomla asset, no duplicate sermon. |
| AR09 | Guest preacher | Choose Ruth, normally at East, to preach at North. | Sermon's preacher is Ruth and church is North. Her home church is untouched. Public status respects the chosen service's ACL. |
| AR10 | Missing preacher or deleted church | Delete or unpublish required selected identity before export finishes. | Hold the verified file and draft/inbox item; no guessed assignment or public sermon. Reviewer can remap within scope. |
| AR11 | Editor protects title | Reviewer renames sermon and explicitly sets unpublished. A late video rendition arrives. | Attach media without reverting name, approval or publication. No duplicate notification of a new sermon. |
| AR12 | Insufficient file quantity | Legacy sermon already has a recording and File Type quantity could evict it. | Warn/hold copy or use explicitly configured separate allowed type. Never silently delete the old recording. |
| AR13 | Audio track missing | Receive silent video-only master when both tracks requested. | Video can verify; audio shows unavailable. Do not manufacture a silent audio attachment or misreport complete backup. |
| AR14 | Remote deleted after verification | Verify local audio/video, choose local playback, then upstream becomes inaccessible. | Local replay, seek and download behave according to Joomla ACL without provider account access. |
| AR15 | Incomplete HLS tree | S3 recording manifest references missing segments or metadata shows failure. | Do not register fully verified. Retain failure details and original; let editor recover from separately supplied original. |
| AR16 | Simulcast and fragments | Same service has Mux and YouTube replay plus short reconnect recordings. | Associate alternatives/ordered parts to one occurrence. No two published sermons and no silent removal of missing time. |
| AR17 | Recorded rehearsal | Broadcast test input with rehearsal flagged and receive working MP4. | Never auto-publish or associate to Sunday's live sermon. Retention follows rehearsal policy. |
| AR18 | Rollback a replay switch | Local file verified but inaccessible on the web host after deployment. | Diagnose origin/ACL; retain last known good authorized replay or processing message per the church's fallback policy. |
| AR19 | Restore protected media | Restore database and files to another web root, with provider disconnected. | File GUIDs and ACLs survive; media root is rebound, sample seeks correctly, downstream jobs remain paused until checked. |

## Security, failure and operations

| ID | Scenario | Arrange and act | Required result |
| --- | --- | --- | --- |
| OP01 | Callback authenticity | Send legitimate provider challenge, a valid event twice, stale event from wrong tenant and unauthorized event. | Challenge handled correctly; valid event stored once; stale/unowned messages cause no business changes. |
| OP02 | Secret redaction | Inspect public JSON, logs, support receipt, calendar subscription, export and HTML after a failed provider call. | No API secrets, refresh tokens, RTMP keys, signing keys, private invites or presigned source URLs escape. |
| OP03 | Unsafe import origin | Provide an unapproved media host or redirect outside the import policy. | Transfer stops without contacting forbidden destinations or forwarding authorization. Approved same-origin transfer still succeeds. |
| OP04 | Concurrent refresh | Force token expiry and run two workers for the same OAuth account. | One refresh is committed atomically; neither worker overwrites a newer rotated token. |
| OP05 | Read-only admin | Attempt to rotate keys, delete remote VOD and publish across churches by direct action. | Each unauthorized action fails independently of UI visibility; remote state unchanged. |
| OP06 | Lazy task runner idle | Stop the scheduler over a quiet weekend then restore it. | Admin shows stale heartbeat and oldest job. Reconciliation backfills once without rewriting the next service. |
| OP07 | Source deletion shared by two sermons | Request remote deletion where another mapped sermon still uses this recording. | Action identifies references, requests resolution and does not silently break another sermon. |
| OP08 | GDPR/privacy-style takedown workflow | Unpublish local event while permitted provider replay remains public. | All Joomla presentations honor local change; operator is told public provider access is separately managed. |
| OP09 | End browser capture unexpectedly | Interrupt capture after several acknowledged chunks; submit a repeated chunk. | Incomplete recording remains private, repeated chunk deduplicated, original/recovery action presented. |
| OP10 | Correlated provider outage | Fail every request to one vendor while another church uses Icecast or approved HLS. | Per-account throttling contains retries, existing local replays still play, and no cross-provider cascade occurs. |
