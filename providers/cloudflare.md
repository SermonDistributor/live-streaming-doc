# Cloudflare Stream and existing Cloudflare delivery

## Church-owned Stream account

Collect account ID, narrowly scoped Stream API token and the customer playback hostname or official descriptor. Use a read-capable credential for adoption. Writing an input or preparing a download requires appropriate write permission. The API base is https://api.cloudflare.com/client/v4/accounts/{account_id}/stream. List live_inputs, select the current input by UID, and import the church-supplied name. When explicitly creating an input, POST /live_inputs with meta.name and the appropriate recording options. Store input UID, returned ingest information privately, and the provider's playback hostname. Never build the customer code from the account ID.

**Cloudflare documents that recording.mode=automatic enables both HLS/DASH playback and automatic recording. Its default off mode disables this playback as well as recording.** Explain this coupling before the church activates a normal Stream live input. If the church requires zero provider recording, leave the setting untouched and let it choose a compatible alternative. Deleting recordings later is a different consent choice.

| Work item | Evidence and behaviour |
| --- | --- |
| Schedule | Joomla owns each timed occurrence and countdown for a reusable input. Stream input creation is separate from a calendar event. Scheduled deletion is retention, not service scheduling. |
| Stable live player | Use the input UID with the official Stream iframe or customer hostname manifest template. The input URL follows the current live broadcast. Use an individual video UID for immutable replay. Returned playback.hls and playback.dash are player sources. |
| Live status | Use the documented customer delivery endpoint /{INPUT_ID}/lifecycle for status and videoUID. Notifications for live_input.connected, disconnected or error are hints and require the scoped input mapping. A connected encoder can still be preparing media. |
| Notifications | Configure Stream Live Notifications via a Cloudflare Notifications destination, restricted to permitted input IDs. Uploaded/VOD-ready Stream webhooks use a **different** registration and signing contract. Validate each via its documented mechanism. Where a live notification cannot be authenticated to the required standard, use it only to trigger an authenticated status lookup. |
| Find recording | GET /live_inputs/{uid}/videos, follow pagination and match the video UID observed in the session. Require ready status. Rapid reconnects can yield multiple recordings. Group them by service evidence, never by “newest video”. |
| Local copy | POST /{video_uid}/downloads for MP4 or /{video_uid}/downloads/audio for M4A. GET downloads/read returned state until ready. Use the authorized returned URL. Private downloads may require a signed token with downloadable permission. Verify local media before changing playback preference. |
| Privacy | Allowed origins and signed URL settings for input and individual video can differ. Validate the replay policy separately. An allowlisted domain is a distribution control, not proof of individual Joomla membership. |
| Retention | Check account capacity and deleteRecordingAfterDays before promising backup availability. Keep a distinct remote-delete action for a verified asset and report a missed export window. |

An account that already distributes its own HLS via R2 or another Cloudflare-hosted origin can supply a **direct media descriptor**. A bucket or ordinary CDN hostname does not create an RTMP ingest, live-event API, viewer counter, encoder or downloadable master. Joomla should ask for a permitted manifest and original-file export if the church has one. Maintain the existing cache/signing/access policy.

Acceptance: import an existing input without changing keys. Receive an unrecognized input notification and verify no change to another church. Observe the same input on two Sundays with distinct video UIDs. Request audio and MP4 independently. Simulate retention expiry while exporting. Keep the correct report and original sermon association.

Provider documentation includes [input settings and recording mode](https://developers.cloudflare.com/stream/stream-live/start-stream-live/), [live player and lifecycle](https://developers.cloudflare.com/stream/stream-live/watch-live-stream/), [live notifications](https://developers.cloudflare.com/stream/stream-live/webhooks/), [recordings for an input](https://developers.cloudflare.com/stream/stream-live/replay-recordings/), [file downloads](https://developers.cloudflare.com/stream/viewing-videos/download-videos/), [VOD webhooks](https://developers.cloudflare.com/stream/manage-video-library/using-webhooks/), [R2 buckets](https://developers.cloudflare.com/r2/buckets/public-buckets/).
