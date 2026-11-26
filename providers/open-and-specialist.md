# Existing open platforms, audio, church hosts and meeting services

This chapter connects to infrastructure the church already uses. No church must install a media server, change its encoder or move its existing broadcast.

## PeerTube

Discover the church-selected instance and version. Confirm it is the canonical permitted host, the church can publish on its channel, live is enabled, and its privacy/registration rules permit the desired audience. A public instance's hostname and video UUID form a pair. A remote federated copy is a different origin for authentication. Never send the church's access token to another instance discovered via federation or redirect.

Use the official embed URL returned by the instance, or a documented permitted playback descriptor. Use a platform-issued embed/password/access flow for restricted records. Optional church-player playback requires the authorized streaming playlist and compatible CORS. Expose P2P distribution only with the church's consent settings. Save instance, video UUID, local remote numeric ID where required, and channel. Store no RTMP key in a public projection.

For a church-authorized live channel use the instance's versioned REST API. Create via POST /api/v1/videos/live with channel, name, privacy and live settings. Read or update the live configuration at /api/v1/videos/live/{id}. Where supported use schedules[].startAt, permanentLive and saveReplay. An update to schedules may replace the entire list: read/merge by mapped dates and preserve unowned entries. When later occurrences cannot be represented, keep them local and show sync coverage. List sessions at /api/v1/videos/live/{id}/sessions and correlate the returned replayVideo. A one-time live can be replaced by its replay. A permanent live generates distinct replay videos. Read a real session or published state before asserting live. Use only authorized downloadable replay files. An administrator may disable downloads. Test private instance, federation redirect, missing live privileges, recurring permanent video and a cancelled date.

References include [PeerTube publish a live](https://docs.joinpeertube.org/use/create-upload-video), [Live REST resources](https://docs.joinpeertube.org/api-rest-reference/api/live-videos.html), [update/response fields](https://docs.joinpeertube.org/api-rest-reference/api/video.html).

## Owncast

A church can give its HTTPS instance URL, official video embed or HLS playback output. Confirm video/links stay within registered origins or an explicitly approved object-storage origin. Use the instance's public status API for the live flag and start time. An authorized Owncast webhook can reduce delay, with a readback of current status where authenticity is limited. Prefer provider status over checking whether a persistent player page returns 200. A different church using the same software has a different instance and namespace.

Joomla owns the calendar and preacher metadata. Owncast's optional object-storage delivery keeps recent live segments and prunes them. It is not a permanent recording library. Offer a separate church-owned recording/original workflow. If the church has an actual long-lived archive URI, treat it as a separate source with its own authorization and verification. A stream and chat are distinct optional embeds. Enable chat only when explicitly selected.

References include [embedding](https://owncast.online/docs/embed/), [public API](https://owncast.online/docs/plugins/apis/), [webhooks](https://owncast.online/docs/api/webhooks/), [storage and live-window retention](https://owncast.online/docs/storage/).

## MediaMTX, SRS, other existing servers

| Existing system | Input to Sermon Distributor | What to automate |
| --- | --- | --- |
| MediaMTX | Approved HTTPS HLS URL or official browser iframe for HLS/WebRTC, named stream path, optional restricted backend monitoring credentials | Map one path to one church destination. The Control API can observe a ready source. Keep management private. A church-enabled recording/playback service can list time spans and supply recordings for authorized import. A gap between recorded spans produces separate parts. |
| SRS | Church-provided HLS output/approved player, vhost, app and stream name. Optional authenticated relay from its HTTP callbacks | Receive on_publish/on_unpublish and verify the authoritative stream identity. on_dvr identifies completed files within the provider's environment. A filesystem path on another server is not a downloadable HTTPS file. Ask for a church-controlled export endpoint or original. |
| Generic WebRTC / WHEP | Documented viewer endpoint and compatible player, or provider-issued iframe | Use only the viewer protocol explicitly supported by the adapter. WHIP/RTMP/SRT ingest is for the encoder. Offer HLS or external watch fallback when ICE, firewall, codec or browser restrictions prevent connection. |
| Icecast / existing internet radio | HTTPS listener mount, media type, optional statistics endpoint, optional programme feed | Play authorized MP3/AAC/Opus formats according to browser support. Where enabled, use the matching source mount in Icecast statistics, not a count of listeners. A 24/7 station or fallback mount does not identify the preacher. Require mapping/operator signal. Archive the owner's source recording when available. |
| Other audio service | Approved direct audio URL, authorized widget, or listen link | Preserve redirects only to allowed CDN hosts. Handle mixed-content rejection, unavailable mount and unsupported codecs. On-site audio can be the primary option for a congregation. No video provider is required. |

References include [MediaMTX web playback](https://mediamtx.org/docs/read/web-browsers), [MediaMTX Control API](https://mediamtx.org/docs/features/control-api), [MediaMTX recording playback](https://mediamtx.org/docs/features/playback), [SRS HLS](https://ossrs.io/lts/en-us/docs/v6/doc/hls), [SRS callbacks](https://ossrs.io/lts/en-us/docs/v6/doc/http-callback), [Icecast statistics](https://www.icecast.org/docs/icecast-latest/server_stats/).

## Church-focused hosts and syndicators

* **SermonAudio.** Use the official Embed Editor's broadcaster webcast view or public webcast URL. Store the broadcaster identifier and generated validated embed. Distinguish a live webcast from an individual archived sermon. Keep weekly scheduling and attribution in Joomla unless the church provides an entitled integration furnishing actual schedule/recording IDs. [Embed Editor](https://www.sermonaudio.com/features/embed).
* **Sermon.net.** Accept its Live Player or a public live channel permalink. A separate replay can be linked after the event, or the church can import its retained original. Respect account/plan access and avoid hard-coding a media CDN path taken from the widget. [Providing access to a live stream](https://help.sermon.net/kb/live-streaming-1a58a82f7ac9f7a3/providing-access-to-your-live-stream-sm).
* **Church Online Platform.** It can itself embed an upstream stream. Ask for the underlying authorized provider if the church wants native integration. Otherwise offer its existing service page as a link. Do not nest a full interactive church site inside another player. [Church Online provider model](https://support.online.church/l/en/article/gs26xs0vhm-streaming-providers).
* **Other branded church services.** Configure the provider-approved public iframe URL or worship-service page, preserving access and domain rules. A branded church platform may already use Mux privately. Do not ask the church to obtain another vendor's internal Mux credentials. A visitor's player cannot be used to discover a private download source.

## Meetings, town halls and public networks

| Service | Website + schedule handoff | Recording/replay |
| --- | --- | --- |
| Zoom Meetings/Webinars | Store attendee join/registration URL. A host start_url, registrant-specific or presenter invitation is confidential. When the church authorizes Zoom's meeting/webinar API, map recurring series and occurrence IDs, timezones, start times and registration. If Zoom sends a custom livestream to Mux/another provider, use the receiving provider for the public broadcast. Meeting SDK embedding is an explicitly different interactive experience. | With required owner credentials and cloud recording entitlement, consume recording.completed, look up the meeting instance UUID and permitted recording_files, then authorize the download just in time. Otherwise import host's original or use the permitted share URL. |
| Microsoft Teams town hall | Use Microsoft Graph virtualEvents/townhalls under tenant-consented permissions if managing it here. Create draft, publish only on editor confirmation, then read sessions and map attendee-facing URLs. Retain IDs for event and session. Ordinary Teams meetings and registration links have different lifecycles. | Use tenant-permitted recording/replay or organizer's file. Never publish organizer/presenter links or infer public VOD simply from meeting completion. |
| Kick | Use its documented channel iframe/live watch page. Joomla handles occurrence, preacher and local attachment when the church uses a custom profile with no authenticated adapter. | Link permitted replay or import original. [Official embed](https://help.kick.com/en/articles/8010826-how-to-embed-your-kick-livestream). |
| Rumble | Validate the church's official embed or watch URL. Reusable channel and individual broadcast are separate references. Use operator status or approved API evidence. | Adopt provider replay or owner file. [Rumble embed](https://rumble.support/en/help/rumble-basics-how-to-embed-your-video). |
| LinkedIn Live, Instagram Live, other networks | Safe event/watch HTTPS action is the baseline. Use an approved provider embed only for the specific resource and supported audience. Registration, login or mobile-app requirement appears next to the action. | External page/authorized original. Do not scrape playlists, collect viewer credentials or manufacture a file from an embed. |

References include [Zoom meeting API](https://developers.zoom.us/docs/api/meetings/), [Zoom custom livestream destination](https://support.zoom.com/hc/en/article?id=zm_kb&sysparm_article=KB0064210), [Teams town hall creation](https://learn.microsoft.com/en-us/graph/api/virtualeventsroot-post-townhalls?view=graph-rest-1.0), [Teams sessions](https://learn.microsoft.com/en-us/graph/api/virtualevent-list-sessions?view=graph-rest-1.0).

## Add an unfamiliar provider without a code change

The operator can supply a watch page only, authorized embed, HLS/DASH source, or direct audio/video. Require a descriptive name, church, privacy scope and canonical external fallback if available. Optionally identify an approved replay/original source independently. Unknown schedule or recording capabilities remain local/manual. A custom profile can deliver a meaningful working player/link and countdown without claiming authenticated automation. Add a dedicated adapter only when a documented remote operation can be exercised end to end with the church's permissions.
