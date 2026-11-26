# 5centsCDN, Castr, Dacast, Wowza and CDN-backed providers

## 5centsCDN

Use the church's service and its API key. The v2 API documents X-API-Key with base https://api.5centscdn.com/v2. GET /streams/push supports pagination. Inspect pull resources separately for churches relaying an existing origin. Adopt a stream by service/stream ID, not its display name. Obtain enabled playback protocols and the published HLS URL or official embed from the provider. Respect domain locks, token policies and geographic rules. Do not enable or change protocols during discovery. Some update methods replace the protocol array and can silently disable omitted outputs. Render a church player only for a permitted, working playback protocol.

For status use the stream's supported status/monitoring capability and evidence scoped to the stream. A configured push resource or HLS endpoint alone is not evidence of a current live service. Ordinary push/pull streams use the Joomla calendar. A scheduled playlist is a playout resource: if the church already uses scheduled programming, read that resource via List Scheduled Playlists and map times only when it represents the published service. Enabling prerecorded playout or restarting a publisher is outside an ordinary calendar edit.

Where DVR is active use List/Get DVR streams then the corresponding export action for the recorded range and destination folder. Store service ID, recording ID and UTC bounds. Verify the completed exported file and authorized access before ingesting it locally. Never assume a DVR is enabled because an HLS URL plays. Treat missing geographic coverage, protocol approval, insecure-only ingest or DVR add-on as capabilities of the particular church account. Its retail and reseller plans are separate from this church-owned model.

Test a locked protocol array, a successful read-only import, an inactive push input, an export straddling a reconnect, and a stream available from Europe but slow for a Namibian mobile viewer. Maintain a visible alternate and low-data choice.

References include [push resources](https://developer.5centscdn.net/api-reference/get-streams-push), [push update semantics](https://developer.5centscdn.net/api-reference/post-streams-push-streamid), [website/HLS playback](https://www.5centscdn.net/help/how-to-push-or-ingest-a-live-stream-to-5centscdn/), [scheduled playlists](https://developer.5centscdn.net/api-reference/get-streams-scheduledplaylist), [DVR details](https://developer.5centscdn.net/api-reference/get-streams-record-streamid), [DVR export](https://developer.5centscdn.net/api-reference/post-streams-record-streamid-export).

## Castr

Adopt the Castr livestream object under church API credentials. Use the returned approved embed or a church-provided HLS playback URL for native playback. Castr documents HLS availability on qualifying plans and provision on request. Inability to obtain HLS must not prevent an allowed iframe. Preserve existing push destinations. For monitor and export use the API's livestream, pull URLs and recording operations. For a new or existing stream cloud_recording controls live recording. Changing it requires the editor's explicit recording choice. Discover recordings by stream/time and their returned identifiers, download only the authorized export and handle expiry.

Castr's scheduled prerecorded broadcast feature is distinct from a recurring camera feed. If an account has a compatible scheduled playout already, follow its timings and treat it as a prerecorded service. Otherwise use Joomla weekly schedule over the stable live resource. Test HLS unavailable on an otherwise valid account, provider embed under a domain restriction, a primary recording that arrives later than a downstream copy, and a no-recording policy.

References include [API](https://docs.castr.com/en/articles/3342306-castr-livestreaming-api), [direct HLS entitlement](https://docs.castr.com/en/articles/2636039-how-to-get-hls-url-for-a-live-stream), [live pull URLs](https://developers.castr.com/docs/getting-pull-urls-for-an-online-stream), [cloud recording](https://developers.castr.com/docs/record-your-livestream), [prerecorded schedule modes](https://docs.castr.com/en/articles/2995235-scheduling-modes-in-castr-pre-recorded-live).

## Dacast

Use church-owned Dacast API credentials and channel ID. Distinguish live channel from VOD asset. Begin by reading the channel and official embed descriptor. Use Dacast's iframe when entitled. Offer native playback only when the church supplies an authorized external-player URL for the channel. Retain the provider player when required by security, paywall or player licensing. Record the published viewing URL as the independent fallback.

The API exposes live channel creation/update and recording/DVR settings. A reusable live channel is not a dated public event. Keep the calendar in Joomla unless the specific Dacast offering exposes a compatible event with documented read/write dates. Changing DVR/record flags is an explicit resource action that may affect subsequent services on the channel. Map completed VODs by their identifiers, import permitted exports or accept an original. Test private/paid player, token expiry, disabled recording and two Sundays using the same channel.

References include [API introduction](https://docs.dacast.com/docs/getting-started), [live channel creation](https://docs.dacast.com/docs/create-live-streams-channels), [update live settings](https://docs.dacast.com/reference/update-stream), [lookup embed](https://docs.dacast.com/reference/lookup-embed-code).

## Wowza

Distinguish Wowza Video's managed cloud API from a church's existing Wowza Streaming Engine endpoint. For either, adopt verified HLS playback/official embed and a fallback. For Wowza Video, use the existing cloud stream/transcoder identifier and the account's documented authorization. Its schedules explicitly start/stop a transcoder and support one-time/repeating actions. Display this side effect and require a production-control permission. Import an already scheduled transcoder action if it actually corresponds to a public service. Do not silently start/stop production for every countdown. A dedicated Streaming Engine host has a different API and configuration. Support generic playback and a church-approved recording export even when the managed API is absent.

References include [Wowza Video schedules](https://developer.wowza.com/docs/wowza-video/guides/more-tasks/schedule-a-transcoder), [Wowza developer documentation](https://developer.wowza.com/).

## Bunny, Livepush and other hosting setups

* **Bunny CDN / Bunny Stream.** Accept an existing authorized HLS origin delivered over Bunny CDN or an official Bunny player for a compatible asset. Bunny video-library and CDN credentials have different scopes. The reviewed library documentation establishes uploaded video/encoding/delivery. A stored video or CDN zone alone does not establish a live input, status feed or scheduled event. When a plan exposes a live product, require its actual returned descriptor and tested capability before enabling remote writes. [Bunny Stream docs](https://docs.bunny.net/stream), [CDN and delivery](https://bunny.net/pricing/).
* **Livepush.** Its event-hosting offering supplies website player and direct HLS. Multistream-only plans can have different entitlements. Select website hosting separately from downstream public links, and import an authorized export when available. Represent any provider scheduled pre-recorded event accurately. [Live event hosting](https://livepush.io/pricing/live-stream-hosting/index.html).
* **Existing CDN/object storage.** A storage service, VPS, CDN or domain name describes delivery infrastructure. Ask for playback descriptor, production ownership, any status source, and permitted archive/export separately. Generic HLS/MP4 with a Joomla schedule is a complete working mode. CloudFront, Cloudflare R2 and existing S3-compatible deliveries fit here without changing the church's pipeline. [CloudFront delivery](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/live-streaming.html).
