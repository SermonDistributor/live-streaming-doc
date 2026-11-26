# Provider selection and feature matrix

**Choose Mux first** for churches starting fresh. “Use what we already have” stays equally visible. Capability depends on the specific account, plan, region, privacy and resource. This table describes what to offer after discovery. It does not certify a live production tenant. A local calendar, explicit operator live control, authorized original-file import and automatic sermon attribution apply across all rows.

| Provider or existing setup | On-site viewing or handoff | Schedule authority supported by the design | Detection | Archive route |
| --- | --- | --- | --- | --- |
| [Mux](mux.md) | Church HLS player or Mux Player | Joomla over stable input | Signed webhook + readback | Asset, authorized MP4/master/M4A |
| [Cloudflare Stream](cloudflare.md) | HLS/DASH or Stream iframe | Joomla over live input | Input lifecycle and live notifications | Recording video UID, MP4/M4A |
| [AWS IVS Low-Latency](amazon.md) | IVS Web Player, supported playback alternatives | Joomla over channel | EventBridge / GetStream | Church S3 recording then validated local file or HLS tree |
| [YouTube Live](youtube.md) | YouTube iframe or watch link | Adopt or create/update individual broadcasts | Authenticated broadcast status | YouTube replay or church original |
| [5centsCDN](cdn-services.md) | HLS or official embed | Joomla for ordinary streams. Read existing scheduled playout separately | Account stream monitoring or operator | Entitled DVR export |
| [Telegram](telegram-and-social.md) | Official live invitation button | Joomla or optional admin user-session scheduled call | Authorized call state or operator | Owner recording/Saved Messages or original |
| [Vimeo Live](vimeo-and-restream.md) | Stable event iframe. Permitted direct source only if provided | Entitled Live API / follow existing schedule / local | Live event and current clip | Completed clip and authorized export |
| [Twitch](telegram-and-social.md) | Official channel/video embed or watch link | Helix schedule segments and constrained recurrence | EventSub + Get Streams | Authorized VOD link or owner file |
| [Facebook Live](telegram-and-social.md) | Public eligible video plugin or permalink | Eligible Page Graph live event, or local | LiveVideo status | Authorized owner export or original before retention expiry |
| [Restream](vimeo-and-restream.md) | Paid website player or mapped downstream | Events API, downstream ownership selected once | Upcoming/in-progress/history | Event recording listing and authorized download |
| [PeerTube](open-and-specialist.md) | Instance embed or permitted playback URL | Live schedules on supporting instance. Permanent/one-off | Session and video state | Replay video and permitted download |
| [Owncast](open-and-specialist.md) | Instance embed or HLS | Joomla | Public status and optional webhook | Separate original or configured archive |
| [MediaMTX / SRS](open-and-specialist.md) | Authorized HLS/WebRTC player as supported | Joomla or existing external calendar | Path readiness / source callbacks or operator | Authorized playback/export or original |
| [Icecast / existing radio](open-and-specialist.md) | Church audio player or listen link | Joomla or imported programme schedule | Mount/source plus programme/operator confirmation | Church's audio original or permitted source capture |
| [Castr / Dacast](cdn-services.md) | Entitled HLS or provider player | Local live calendar. Compatible provider playout independently | API or operator according to resource | Cloud recording/VOD when available |
| [Wowza](cdn-services.md) | Managed player or approved HLS | Local calendar. Separate authorized transcoder schedules | Resource monitoring | Authorized recording/export |
| [Bunny / CloudFront / R2 / hosted CDN](cdn-services.md) | Working existing manifest, video, audio or approved embed | Local or upstream production system | Upstream status/operator | Authorized source or church storage |
| [Livepush / SermonAudio / Sermon.net](open-and-specialist.md) | Entitled player, direct URL or service link | Joomla or qualified upstream event | Provider/authorized operator | Existing replay or original. [Livepush details](cdn-services.md) |
| [Zoom / Teams](meetings-and-originals.md) | Attendee link or configured downstream stream | Authorized account event operations or follow | Owner event evidence or operator | Entitled organizer recording or import |
| [Kick / Rumble / other social or church host](open-and-specialist.md) | Official permitted embed or HTTPS button | Local for link/embed profile | Operator or verified external evidence | External replay or owned file |
| [Any other provider](open-and-specialist.md) | HLS, DASH, direct audio/video, approved iframe, or link | Joomla unless a tested calendar interface is added | Declared evidence only | Independently permitted export/file |

## A connector's capability declaration

Declare discovery, account identity, adopt, create resource, live status, calendar read/create/update/cancel/recurrence scope, embed, native playback, external action, recording detection, audio/video export and remote deletion as **separate capabilities**. For each operation return supported, unavailable, or unavailable for this account, with a reason and evidence timestamp. API read access does not imply write access. A provider may allow deletion of a VOD but not deletion of its reusable live channel. The account UI must not present a destructive API as routine housekeeping.

Connection tests should be read-only. Explicitly authorize provisioning or end-to-end billable test broadcasts in the church's selected sandbox/rehearsal environment. Record effective account, region/instance, available resources and scopes, emitting only sanitized results. Acquisition of a provider credential is independent of volunteer publishing privileges.

For every native operation store resource kind, external ID, account namespace, desired revision, result and retry classification. Return canonical playback/attendee descriptors with expiry, privacy and permitted fallback. Keep stream keys, provider admin URLs and API tokens out of all public descriptors. Apply [shared operations](../docs/operations.md) when an individual provider's docs do not guarantee callback ordering or delivery.
