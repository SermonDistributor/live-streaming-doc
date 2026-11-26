# Local recording and local-origin playback

## Select an appropriate host capability

| Host capability | Available mode | What the editor must see |
| --- | --- | --- |
| Ordinary Joomla/PHP hosting, writable private directory, periodic tasks | Completed provider export or editor upload, local progressive replay | Transfer size limit, retention, last task heartbeat and available disk |
| Same hosting with insufficient single-request duration | Chunked final-file download with persisted checkpoints where origin supports it. Resumable upload by editor | Progress survives request timeout, or clear incompatibility and an alternate upload/export |
| Supervised church worker with approved capture source | Authorized direct-source recording, segmentation and optional offline remux/audio extraction | Exact source, start/stop policy, clock, lease, capture limits, permissions and recovery |
| Administrator's camera/microphone in a supported secure browser | Operator-initiated local recording or encoder-file upload | Permission, audio input selection, visible recording status, stop, save and upload confirmation |
| Only a protected social iframe or app invite | Existing provider replay, independently recorded original or live only | This input is for viewing. No capture button for viewer playback. |

Allow video-only, audio-only, both tracks and imported captions separately. If local playback is selected without a viable local source, let the editor choose “wait for local file” or explicitly permit provider replay while pending. Keep provider-only archives and local-only archives usable independently.

## Optional browser recording

Offer capture only in a protected operator interface with consent and secure origin. Test browser APIs and supported media types at runtime. Let the operator record audio alone, camera with microphone, or an explicitly granted device/screen where policy permits. Preview local devices before starting and let the operator choose tracks. Never try to capture a YouTube/Vimeo iframe or pretend a participant's browser is an always-on server.

Show actual track status, recorder state, elapsed time, estimated local size and progress of completed chunks. Negotiate a supported container/codec. File extension follows the captured bytes. The browser may emit WebM rather than MP4. Assign one capture/upload ID and consecutive chunk numbers. Persist server acknowledgments and chunk sizes/digests, deduplicate repeated chunks, and reject conflicting data for a previously accepted position. Authorize every chunk for the same church and occurrence. Finalize only when the end marker, sequence, length and media integrity agree. Provide “save on this device” where supported so closing the page after capture need not discard the original.

When the operator closes the tab, loses the device, denies camera, switches input or exhausts device storage, mark partial/missing tracks explicitly. Recovered partial media can be reviewed manually, never silently published as a full sermon. Screen locks/backgrounding on phones can suspend capture. Recommend the encoder's own recording as an independent fallback for a service relying on local capture. Test Chrome/Firefox desktop and Safari/iOS with supported modes and refusal paths. Lack of an API removes only that option.

## Optional server recording of a permitted source

Use a church-controlled or expressly exportable audio/HLS source. Start under explicit operator action or a configured programme recording policy with bounded lead-in and postroll. Clock-triggered start does not imply the service is live. Capture may contain waiting/slate and is labelled accordingly. Format and source identity must match the authorized configuration. A durable lease prevents multiple simultaneous recorders for the same desired session. Limit maximum duration independent of a missing stop signal, rotate temporary segments and check headroom. Respect inactivity timeouts and storage policy.

For HLS handle media sequence progression, relative URIs, new variants, discontinuities and reconnects. For protected content verify that the church has capture rights and supported access. Do not bypass DRM. Detect live-window gaps rather than filling them with a later playlist. Preserve per-part timestamps, dubs/channels and caption references where available. Deduplicate segments by stream identity, sequence and discontinuity context. Duplicate filenames across restarted sessions may identify different bytes. Stop safely on source/ownership change. Keep late-arriving segments separate from a subsequent service.

Finalization can retain a complete HLS package or remux an authorized source to a suitable local format. A mux/remux repackages existing tracks. Re-encoding requires CPU, can change quality, and must be deliberate. If an audio-only replay is requested and the authorized video has no audio track, report unavailable audio rather than delivering a silent substitute. Validate tracks, duration, seek and file ACL before registering it with the [sermon workflow](recording-and-sermon.md).

## Local delivery

Generate the church playback route/descriptor from the verified File identity. Public media can use a church-configured direct URL. Restricted files require a private storage origin and a checked route or short-lived internal redirect/signed URL. Avoid forcing a large video through a long-running PHP request if the web server can serve its bytes after authorization. Preserve HTTP content type, byte ranges, HEAD, partial content and seeking. An unsatisfiable range returns the appropriate response without disclosing protected data. For alternate audio, publish the audio URI and label it clearly. Do not initialize video when the listener selects audio.

Retain an untouched original if church policy requires it. Generating a web-friendly derivative never overwrites that original. Identify derivatives by source digest, transformation specification and rendition. If transformation or disk verification fails, keep the last known good playable version. A deliberate change from local back to provider is a playback preference change, independent of removal/retention.

Sources include [MediaRecorder](https://developer.mozilla.org/en-US/docs/Web/API/MediaRecorder), [captureStream](https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement/captureStream), [HLS](https://www.rfc-editor.org/rfc/rfc8216), [HTTP semantics and range requests](https://www.rfc-editor.org/rfc/rfc9110).
