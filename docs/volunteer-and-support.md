# Service-day operation and support

## The Sunday workflow

**Before Sunday.** The administrator connects the church-owned provider or adopts its existing viewing URL. An editor selects church, preacher and recurrence once, chooses where to keep audio/video, configures the menu and tests the viewer-facing URL. An entitled API creates/adopts remote dated events. For a stable input the volunteer keeps the encoder destination already in use. The dashboard should show “Ready for Sunday” only for the capabilities actually checked, with a separate archive and schedule status.

**Before the service.** The volunteer opens the service desk, confirms church, room, preacher, language and privacy, and can preview the encoder without exposing a rehearsal. On a phone outside the church's local network, scan/open the permanent church URL and check whether it is accessible to a real visitor. Avoid testing only in an administrator session. Permit a change of speaker without touching the encoder key or next week's programme.

**When the service starts.** The encoder sends to the church's provider. The component accepts authenticated status or the authorized operator confirms the matching occurrence. The congregation sees the correct preacher as live. Somebody unable to watch video can choose the separate audio source if available. A church using Telegram sees its configured outside link. An alternative HLS source can remain on the church site. No one needs to manually paste a fresh embed into Joomla every Sunday when a stable input or mapped event is available.

**After the service.** The operator ends production at the proper provider or lets the encoder disconnect per policy. The site transitions to processing. A background job obtains authorized recording(s) and the service inbox shows the correct sermon, its speaker, church and archive state. If the church keeps only local replay, the public never receives a provider fallback unless the church expressly enables one. Incomplete metadata or a failed transfer stays in review, with the original media preserved where available.

## Diagnose by symptom

| What the church sees | Inspect first | Recovery and verification |
| --- | --- | --- |
| Clock reached zero and nothing plays | Occurrence is due vs actually active. Active input ID. Provider status, signal age and viewing rights | Compare authorized live desk and anonymous church URL. Offer Waiting or approved button and refresh status. Never declare live just to clear the timer. |
| Correct player, wrong preacher appears live | Account + remote channel/session mapping, actual occurrence and substitution | Unbind incorrect association under audit and bind correct person/occurrence. Repair affected badge and draft sermon. Verify historical speaker data is untouched. |
| Preacher page works but homepage does not | Module scope, menu access, language, Itemid and cache audience | Reproduce as real visitor. Correct Joomla parameters or routing and invalidate related public cache. Keep one canonical sharing route. |
| People at church can see it but home viewers cannot | External DNS/HTTPS, domain-restricted embed, login/privacy, regional network, signed URL expiry | Test from an outside network. Select a permitted fallback or locally hosted replay. Keep restrictions intact. |
| Video platform works but church iframe is blank | Player policy, allowed parent domain/referrer, CSP, consent, Safari/Firefox embed privacy and embed permission | Use provider-approved player parameters or direct approved watch fallback. Do not weaken site-wide browser protections. |
| Live works, archive does not | Specific asset ID, provider export entitlement, final recording, correct recording token and retention window | Reconcile asset. Refresh URL or request export. If expired, ask for church's encoder original. No duplicate sermons or deleted source. |
| Audio works but local video will not seek | File container, codecs, MIME, content length and authorized HTTP Range handling | Revalidate sample start/middle/end, correct protected delivery or generate compatible derivative from approved master. |
| Multiple duplicate remote events | Inspect uncertain create, source ownership, previous resource mappings and any multistreaming intermediary | Freeze writes for that destination, select canonical event, preserve public links where possible. Delete duplicates only with separate authority/confirmation. |
| Backup stalls after several minutes | Chunk size, execution time, quota, free disk, Range/ETag and job heartbeat | Resume safely or use authorized worker/upload. Verify byte count, media and digest before marking stored. |
| A private sermon appears in public list | Occurrence versus sermon/file access, cached layout, provider public playback ID and fallback descriptors | Stop public presentation, correct caches and ACL, inspect provider-side privacy separately. Verify anonymous page, feed, HEAD/Range and manifest/segments. |
| Live indicator remains overnight | Stale observation, manual-control expiry, missing offline notification, 24/7 source | Reconcile and expire the specific session. Check future service still schedules correctly. |
| A restricted staff member lost permission | Account grants, Joomla group membership and church scope | Reauthorize only the affected features. Verify another church and historical sermons still work. |

## Capacity and affordability of local replay

For a continuous average bitrate b in megabits per second, hours h, and average concurrent viewers v, approximate decimal GB delivered as **0.45 × b × h × v**. Measure actual selected renditions. Adaptive streaming, captions, retries and cache behavior change totals. A 90-minute service at 3 Mbps with 50 viewers is about 101.25 GB of viewer delivery and a single copy is about 2.025 GB. Four such services are about 405 GB for delivery and 8.1 GB for the four originals, before derivatives, partials and backups. A switch to local playback moves delivery load to church hosting. It does not make the bandwidth bill disappear.

Estimate actual peak egress separately: 50 concurrent viewers at 3 Mbps use about 150 Mbps before overhead. If the existing church web account cannot sustain that, it can **keep a local backup and continue serving provider replay**. That choice is independent of the live broadcast provider. Let the editor see retention, free space, oldest pending export, approximate local transfer volume and any configured warning threshold. A church-owned AWS/Mux/Cloudflare bill is governed by its own account and prices. Avoid displaying estimated usage as an invoice.

## Budget for four Sunday services

This example assumes four 90-minute services, 50 viewers per service who each stay for all 90 minutes, a 720p live feed, and no replay views. That is 360 input minutes (6 hours), 18,000 viewer-minutes (300 hours), and up to 360 minutes of new recordings in the month. Rates below are published USD prices checked on 26 November 2026. Recheck entitlements, credits and previous attendance when showing a church its own estimate.

| Choice | What the church would budget in this example |
| --- | --- |
| Mux Video, pay as you go | Live is available on this plan. At the listed starting 720p Plus input rate of $0.025 per minute, 360 input minutes cost about $9. The 18,000 viewer-minutes fit inside the account's initial 100,000 monthly delivery minutes if available. The plan lists a $20 monthly usage credit. Modest usage can therefore produce a zero-dollar bill. Account for accumulated archive storage, requested static renditions, and other usage on the same account. The Free plan lists on-demand video only. [Mux pricing and estimator](https://www.mux.com/pricing) |
| Cloudflare Stream | At $1 per 1,000 delivered minutes, live viewing costs $18. If the church buys one $5 block of 1,000 storage minutes, the total is about $23 before taxes, with room for these 360 minutes if the block started empty. Existing recordings, further replay traffic, and downloadable MP4 delivery can change the bill. [Cloudflare Stream billing](https://developers.cloudflare.com/stream/pricing/) |
| Amazon IVS Low-Latency | Six hours of Basic input at $0.20 per hour cost $1.20 before any account credits. Add 300 viewer-hours at the applicable output rates. For illustration, if all playback is billed as North America HD at $0.072 per hour, output is $21.60 and the live total is $22.80. Use the actual delivery region and quality mix for Namibian congregations. S3 recording and later replay have their own costs. [IVS pricing, channel classes and regions](https://aws.amazon.com/ivs/pricing/) |
| Church-hosted replay | With the same audience watching a full replay at 3 Mbps, monthly egress increases by approximately 405 GB. Obtain the **upload** speed, unit, traffic allowance and monthly cost of the host actually serving the files. An existing $100/month circuit carrying a backup has a different incremental cost from a new $100/month circuit purchased for playback. |

A measured 60 **Mbps** upload can theoretically carry 20 simultaneous 3 Mbps replays. Reserving an illustrative 25% for overhead and other traffic leaves 15. If the service really provides 60 **MB/s** of sustained upload, that is 480 Mbps, or about 120 such replays with the same reserve. Test outside the church network at the busiest time of day. When the provider serves the live feed and replays, audience growth does not multiply outbound church traffic. The church sends the encoder feed once for each configured destination and downloads each archive it chooses to retain.

## Pre-release smoke run

Select one Wednesday/Friday/Sunday rotation, one guest preacher, a YouTube-blocked audience, one entitled hosted provider and one external-only destination. Demonstrate a changed date, a delayed start, an unexpected outage, an on-site native player, the explicit external action, verified local audio with provider disconnected, and the same sermon visible from existing preacher/series navigation. Record what required API credentials and what worked without them.
