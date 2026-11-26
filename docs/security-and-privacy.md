# Trust, visibility, data and consent

## Church scope and least privilege

| Actor | Permitted operations when granted | Sensitive boundary |
| --- | --- | --- |
| Site administrator | Register adapters/approved domains, secret store, shared-host capability, maintenance | A configuration export omits secret values. Staging cannot use production writes accidentally. |
| Church administrator | Manage connections/schedules/retention for assigned churches, approve mapping/terms | Another church's selectors, credentials, private URLs, callback records and usage are inaccessible even by direct IDs. |
| Service editor | Create/modify an occurrence, choose preacher/church, set approved visibility, resolve metadata conflicts | No automatic authority to create cloud inputs, rotate keys or delete remote recordings. |
| Broadcast volunteer | See encoder details only for assigned channel if allowed, run rehearsal, manual start/end for assigned service | Cannot enumerate other churches, access OAuth secrets or publish unreleased sermons. |
| Archive reviewer | Review local files, correct attribution, attach existing sermon, select replay, publish when authorized | Download original or delete upstream requires separate explicit rights. |
| Visitor/member | View authorized page, stream, countdown and captions; download only if separately allowed | Never receives API or ingest credentials. Every API/media endpoint enforces scope independent of the menu. |
| Background identity | Reconcile assigned mapping, stage files, invoke verified native model actions | No universal assumption of super-user; preserve actor/service provenance, track error if model requires a missing privilege. |

Map these use cases to Joomla core actions and component-scoped asset rules in the incoming package. Add distinct actions only where generated CRUD/edit.state and existing file access are insufficient (e.g. rotating ingest keys, controlling broadcasts, copying recordings, deleting provider media). Test both UI visibility and direct controller/task authorization. A legitimate guest preacher from another church requires deliberate association; it does not grant that preacher access to the host church's credentials.

## Credential lifecycle

Store tokens, token secrets, private signing keys, Telegram user sessions and account-bound invitations in restricted configuration with encryption appropriate to the host. Maintain key material outside the exported database when possible, document rotation and recovery for the site's deployer, and store only references in ordinary records. Mask existing values on edit and never return a secret to repopulate a form. Empty edit fields retain existing secrets; replacement/removal is an explicit action. Use separate webhook and API credentials. Verify new credentials against the same namespace before atomic rotation; revoke old grants through the documented provider process. A service using public links can operate without secrets.

For OAuth-based providers bind the flow to initiating Joomla user, church, connection, intended return URL and short-lived state; use exact registered redirects and PKCE where supported. Rotate/serialize refresh tokens atomically under concurrent workers. Avoid a shared confidential application secret embedded in a distributed extension. Show the actual account identity and grants after authorization. A revoked connection can lose write rights while a public embed remains usable. Explain that distinction to the administrator.

## Viewing privacy

* Match church access, publication, Joomla view levels, languages and effective media permissions for the occurrence, preacher, linked sermon, module, calendar feeds, status API, replay and thumbnails. Restrict embed/metadata loading when the church blocks a platform. An accessible website shell is insufficient if the underlying media is globally public.
* Signed playback requires server-side authorization and a bounded-lived, scoped descriptor. Keep the token only where needed for playback; avoid logging query strings and leaking values through referers, analytics, share links or error reports. Do not store expiring URLs in sermon or calendar records. Reauthorize on refresh.
* Honor consent for third-party iframe/SDKs, analytics, embedded chat and cookies. An external action works as a simple link. Show the destination/platform and any account/app requirement. Avoid unnecessary tracking; use aggregate metrics whenever sufficient.
* Treat hidden/private programme titles, guest preacher identities, images, attendance and congregation recordings as potentially sensitive. Show the church when recording occurs, where copies go, who can download them and how long they remain. Separate recording consent from permission to syndicate, derive audio or publish a sermon. Provide unpublish, retention and deletion controls with an audit receipt. Applicable rights and host obligations belong to the church.

## Safe media and webhook boundaries

Validate URLs at save and use: allow only configured HTTPS public playback/export origins and explicit provider URL forms. Block credentials in URLs, forbidden ports, unsafe schemes, local/link-local/private and metadata destinations for an ordinary public importer. Resolve and validate the actual destinations including redirects, IPv4 and IPv6; restrict redirect count and cross-host propagation of authorization. A private self-hosted instance can be allowed through a separately administrator-approved exact origin and network policy. Do not turn arbitrary user input into a server-side HTTP proxy.

Treat URL fragments, filenames, provider HTML, rich descriptions, titles, captions and metadata as untrusted; escape for the rendering context. Parse embed fields into permitted provider IDs or fixed iframe URLs. Limit allowed iframe origins, permissions and scripts through a site policy compatible with the selected provider; preserve required origin/referrer settings. Do not insert provider-returned arbitrary script into an administrator's page. Validate webhook authenticity and account namespace before applying changes, and enforce size/rate limits before expensive processing.

Store local masters and incomplete transfers in non-executable private storage. Generate filenames; enforce file-type and size rules independent of suffixes. Verify container/track content without shell-interpolating filenames or network arguments. Bound any authorized decoder/transcoder worker's resources, process identity and IO. Avoid rendering incomplete partials, exposing directory indexes or leaking protected files through a CDN's public cache. Range and HEAD requests require the same authorization as GET. For local HLS, keys, subtitles, playlists and every segment share the protection.

Sources: [OAuth security best current practice](https://www.rfc-editor.org/rfc/rfc9700), [OWASP server-side request controls](https://cheatsheetseries.owasp.org/cheatsheets/Server_Side_Request_Forgery_Prevention_Cheat_Sheet.html), [OWASP file upload controls](https://cheatsheetseries.owasp.org/cheatsheets/File_Upload_Cheat_Sheet.html), [Mux signed playback](https://www.mux.com/docs/guides/secure-video-playback), [YouTube developer policies](https://developers.google.com/youtube/terms/developer-policies).
