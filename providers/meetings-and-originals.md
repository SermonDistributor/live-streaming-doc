# Church meetings, town halls and original-file sources

A meeting provider has separate organizer, presenter, attendee, registration and recording URLs. The church page should expose only a link or player the intended audience can use. A downloadable original supplied by the church can belong to a service broadcast on a completely different provider.

## Zoom

Choose account-authorized OAuth for the church's permitted users/resources, or a church-managed eligible Server-to-Server OAuth app for its own account. Enumerate the granted resources and scopes in the connection test. Keep confidential client secrets, meeting host start URLs and any registrant-specific credentials on the authorized side. Meaningfully label a public registration page versus a private member's join link. Never put the organizer's start_url into the calendar.

| Action | Procedure |
| --- | --- |
| Adopt | Select the church's existing meeting or webinar, or add an attendee URL manually. Record the meeting/webinar ID and host account. Recurring series have occurrences. A completed meeting instance has its own UUID. |
| Create | Use the authorized user's meetings/webinars creation operation for the selected product. Supply topic, service start, timezone, duration, approved audience/registration settings, eligible recurrence and intended host. Store returned identity and attendee-facing URL. A recurring fixed-time meeting uses Zoom's fixed-time recurrence semantics. |
| Edit | Read current state, distinguish a whole series from a particular occurrence, and send an update at that scope only when supported. If a whole-series mutation would surprise the editor, keep the local exception and show the difference. |
| State | Register Zoom endpoint validation and signed webhook delivery using the app's own secret, or poll authorized status. Match meeting/webinar started and ended with owning account and instance. “Meeting started” proves an interactive room is running. A separate downstream broadcast still needs its own playable signal. |
| Recording | On recording.completed load cloud recordings for the concluded instance. Identify the right recording_files entries, track type, recording interval and download_url, using an authorized download token or bearer as Zoom documents. Encode recurring meeting instance UUIDs in request paths according to Zoom's rules, including possible leading slash or double slash. |
| Preview | An attendee link in this profile opens the approved join/registration flow. A church relaying to Mux/YouTube/another service can select that destination as its public player. Embedding an interactive Meeting SDK needs explicit app credentials, authorization, registration and accessibility handling. Do not present it as a generic HLS URL. |

Store separate screen, audio and gallery-track outputs as related files only when selected and appropriate for publication. A meeting may contain confidential participants, chat or transcript artifacts. Restrict imports by track allowlist and review consent before publication. Never import every record in an account simply because recordings.read is granted. A meeting recorded solely on the organizer's computer requires the owner-file path.

References include [Zoom meeting and recording operations](https://developers.zoom.us/docs/api/meetings/), [API usage and identifiers](https://developers.zoom.us/docs/api/using-zoom-apis/), [Zoom webhooks](https://developers.zoom.us/docs/api/webhooks/), [custom livestream output](https://support.zoom.com/hc/en/article?id=zm_kb&sysparm_article=KB0064210).

## Microsoft Teams town halls

Map tenant, organizer, virtualEventTownhall ID, virtual event session and attendee-facing access. Confirm supported cloud, tenant policy, license and delegated/application permission for **each** operation in the versioned Graph reference. A church with an existing attendee URL can always use the restricted link profile without provisioning a Microsoft application.

1. With the necessary tenant-consented organizer identity, create via POST /solutions/virtualEvents/townhalls. Supply the approved display name, description, start and end dateTimeTimeZone, audience and mapped optional fields. Store event identity. A newly created draft is not yet a public service.
2. Read back and apply allowed updates via PATCH /solutions/virtualEvents/townhalls/{id}. Translate local time using timezones accepted by Graph, preserving original IANA zone and UTC here. Before publishing confirm the event audience and domain. POST /solutions/virtualEvents/townhalls/{id}/publish only when the editor explicitly issues publication or enables that mapped automated action.
3. List sessions for the town hall and validate the attendee-facing join/registration result for the intended audience. Never substitute an organizer or presenter URL. A restricted tenant may disallow anonymous access even though the public Joomla shell is visible.
4. Use the documented cancel operation for that event after confirmation. If one weekly church programme maps to several independent town halls, move or cancel only the mapped future event. A whole-series operation needs separate UI and scope proof.
5. Replay permissions may belong to a tenant-protected recording in Microsoft storage. Use an authorized playback link, lawful exported original or an entitled file connector. A public presentation does not imply the church can anonymously download every organizer's recording. Distinguish an event's scheduled time from verified live broadcast/playback.

References include [create town hall](https://learn.microsoft.com/en-us/graph/api/virtualeventsroot-post-townhalls?view=graph-rest-1.0), [edit](https://learn.microsoft.com/en-us/graph/api/virtualeventtownhall-update?view=graph-rest-1.0), [publish](https://learn.microsoft.com/en-us/graph/api/virtualeventtownhall-publish?view=graph-rest-1.0), [list sessions](https://learn.microsoft.com/en-us/graph/api/virtualevent-list-sessions?view=graph-rest-1.0), [cancel](https://learn.microsoft.com/en-us/graph/api/virtualeventtownhall-cancel?view=graph-rest-1.0).

## Adopt the church's original, wherever it is stored

| Source of original | What to persist | How to acquire it safely |
| --- | --- | --- |
| Existing component-managed file | File GUID, owning entity, church, type, verification state | Check rights, existence, format and permission to associate or copy. Do not move a file away from an existing sermon without explicit approval. |
| Browser/encoder upload | Upload session and original digest | Use private staging, resumable chunks if supported, then managed-file workflow. Preserve original until verified. |
| S3 or compatible bucket | Owner account, bucket, object key, version when available | Church-scoped read permission or narrowly time-limited export ticket. If object or ETag changes during resume, revalidate and restart. |
| Box | Church account, file ID, desired version when supported | Use the authorized file-content download operation. Follow the provider-supplied temporary download securely. Persist file identity and access policy. Box file storage is distinct from the default **Mux** live provider. |
| Dropbox | Church-owned credential/reference, file ID/revision if available | Use the authorized file download operation and capture metadata. A path move or rename must not create a second archive for the same source revision. |
| Google Drive | File ID and revision where available, permissions | Check download capabilities and use the media download operation for a video/audio binary. A public share/preview URL or Workspace document export is not a media master. |
| Approved HTTPS export | Canonical authorized origin, expected asset ID/revision, optional expiry | Refresh from the owner. Stream into private staging. Treat content as untrusted until verified. |

The church may use any of these sources with Telegram, YouTube, a church-specific host, or a generic watch link. Preserve the selection of service occurrence, preacher, host church and recording kind through the import. Detect identical bytes across related variants for review, but never auto-link two churches solely because their byte hashes match.

References include [Box file content](https://developer.box.com/reference/get-files-id-content/), [Dropbox file download](https://www.dropbox.com/developers/documentation/http/documentation#files-download), [Google Drive downloads](https://developers.google.com/workspace/drive/api/guides/manage-downloads).
