# Public playback, Joomla menus and the volunteer experience

## Onboard once

1. The church administrator selects its existing Church record. Show Mux first, followed by “Connect another provider” and “Use an existing link”. Explain which organization owns the account, where provider invoices go, and whether a new cloud resource would be created. Start with no external writes.
2. Choose an existing broadcast/channel or request a new one on that provider. Display the validated account/channel label and last successful check. For imported setups, preserve encoder configuration and the original provider link. Keep rehearsal and production separate.
3. Display a read-only capability summary: on-site church player, provider embed, external link, live detection, schedule read/write, recording export and audio availability. Give the reason for each disabled operation (plan, scope, privacy, unsupported API, hosting constraint). Re-test capabilities on reconnect.
4. Pick replay and publication settings, confirm that the media destination is writable if local archiving is selected, and preview the anonymous and members-only experiences. Permit saving in manual/link mode even when automation is unavailable.
5. Assign a menu item or module to the homepage. Preview countdown, waiting, live and replay states without broadcasting or publishing a real event. The test screen identifies simulated evidence and cannot create sermons.

## Menu item and module contract

| Joomla surface | Required controls | Resolution |
| --- | --- | --- |
| Live service | Fixed occurrence or current service; optional church or programme, language and authorized view levels | Stable route for sharing; pinned item keeps working after end and can offer the sermon link. Current-service route intentionally changes content over time. |
| Live calendar | Church, preacher, programme, date window, next/all, display zone, page size | Accessible chronological list grouped by church date; show empty state, optionally past services with replay links. |
| Live now | Church/site scope, include audio, order | List only fresh confirmed or explicitly operator-confirmed eligible live destinations. One church may host multiple simultaneously. |
| Preacher page integration | Existing preacher record and language; configurable badge/upcoming section | Query by occurrence.preacher GUID and visibility. “Preacher is live” links to the occurrence, not an ambiguous homepage or encoder address. No badge for another preacher sharing a channel. |
| Countdown module | Menu assignment and position, current/next/all/pinned/programme, church/preacher selection, live priority, maximum rows, local/church timezone | Multiple instances coexist without duplicate DOM IDs, independent timers or duplicate player libraries. Reuse shared layout and poll/cache results. |
| Sermon detail | Existing sermon route and metadata; selected archive media and captions | Local audio/video or provider replay according to editor's preference. Public share links point to the church's page. |

Allow independent simultaneous programmes, overflow rooms, translated feeds, audio-only and an alternate destination. Pick one playable default based on the church's priority and audience rights. Label alternatives with language, platform and media type. Never load every player automatically: avoid third-party requests and duplicate audio until selected.

Routes must round-trip with SEF on/off, URL rewriting on/off, menu aliases, multilingual associations, subdirectory installations and no active menu. Generate links through the existing router, preserve Itemid and language where appropriate, use an occurrence identifier for canonical URLs, and redirect an old alias only to the same authorized record. An inaccessible item stays absent from lists and APIs; its title, thumbnails, channel or countdown must not leak. Keep existing category/preacher/series/sermon links functional.

## Playback resolution

| Input | Public behaviour | Failure/permission boundary |
| --- | --- | --- |
| Licensed HLS URL or provider playback ID | Native HLS where supported, otherwise a tested local HLS-capable player using Media Source Extensions. Offer captions, poster, fullscreen and standard controls. | Fetch master, variant, segments, keys and text tracks over HTTPS with appropriate CORS/auth. A working manifest fetch alone is insufficient. |
| Authorized DASH manifest | Compatible DASH player when a tested codec/browser combination is available; serve an alternate HLS or provider player when configured | Do not assume Safari plays DASH natively. DRM/licence playback requires the provider-supported player, credentials and licence configuration. |
| Local MP4/M4A/MP3 or direct permitted audio mount | Native video/audio with accessible controls. Enable byte-range seeking and correct MIME. Stream to the browser from the authorized location. | Local view/download access is enforced for the actual bytes. An HTML page gate alone cannot privatize a public provider URL or media file. |
| Provider embed | Render a provider-specific iframe or SDK using validated IDs/URLs. Preserve mandatory attribution, controls and origin/referrer behavior. | Respect domain allowlists, age, geography, account privacy and third-party cookie conditions. Never extract a manifest from the iframe. |
| External only, player blocked or permitted fallback | Render “Watch on …”, “Listen on …” or “Join …” with the provider's documented HTTPS/deep link. State when sign-in or an app may be needed. | Display the link before attempting an embed when policy blocks the provider. Never silently navigate the visitor away. Private invite links inherit the relevant ACL. |

For an unknown source the editor declares its kind: HLS, DASH, progressive media, audio stream, approved embed, or external HTTPS URL. URL extension is a hint; verify MIME/format, authorization and allowed host. A share page or RTMP ingest URL is not a playback manifest. For embed providers, store canonical IDs and player parameters where possible. An iframe pasted by an editor is parsed into a validated descriptor; scripts, event handlers and arbitrary HTML never become article markup. Unknown embeds require a site administrator's exact host policy. Otherwise show the safe public URL.

## Signal and recovery

| Observation | Viewer message / operator action |
| --- | --- |
| Published upcoming | Service title, preacher, local start and countdown; share/calendar action as available |
| Time reached, provider silent | Waiting for broadcast. Keep the page usable and poll the local status cache. |
| Encoder connected, no playable stream yet | Starting. Enable actual playback after readiness; do not mark every preacher on the channel live. |
| Confirmed playable | Watch live / Listen live. Expose whether a link opens a third-party site. |
| Network drops inside reconnect window | Reconnecting; preserve occurrence, avoid autoplay loops; bounded retries with jitter. |
| Evidence expired, service unreachable | State unknown or temporarily unavailable, show the last checked time for operators. Never display a stale live badge indefinitely. |
| End confirmed, replay absent | Ended; recording processing if requested. Offer next service; no blank video. |
| Replay available | Switch to explicitly selected verified source; preserve session position when practical. If local unavailable, fall back only to an authorized alternative permitted by policy. |
| Cancelled, removed, private or blocked | Serve cancellation or neutral unavailable message according to ACL. Do not expose embargoed titles, alternate providers or private invitations. |

Keyboard users must be able to activate the player, choose another source and exit fullscreen. Announce changes once, support captions and transcripts when provided, respect reduced motion, avoid audible autoplay, and offer an explicit low-data audio option where a real separate track exists. A muted video is not an audio-only stream. Load player assets once through Joomla's Web Asset Manager as required by the active view. Defer third-party players until the church's consent policy permits them.

Sources: [HLS](https://www.rfc-editor.org/rfc/rfc8216), [hls.js browser matrix](https://github.com/video-dev/hls.js), [Joomla Web Asset Manager](https://manual.joomla.org/docs/next/general-concepts/web-asset-manager/), [WCAG 2.2](https://www.w3.org/TR/WCAG22/).
