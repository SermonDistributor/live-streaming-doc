# Worked synchronization and identity examples

Identifiers in this chapter are human-readable fixture labels for documentation. They identify distinct resources and are never token values or production GUIDs. The implementation should replace these labels with isolated test rows and the target provider's real IDs when exercising an authorized tenant.

## A church adopts existing YouTube events

Church North owns channel N. It already scheduled a Sunday broadcast under event Y1 and configured OBS to encoder resource K1. The editor selects Follow provider. A read discovers Y1, associated K1, public visibility, Sunday at 09:00 and video ID. The editor assigns preacher Ruth and selects the local archive policy “encoder file”. A row for the occurrence and (connection N, Y1) mapping is added. There is no create, bind or reset request on adoption.

Thursday the church moves Y1 to 10:00 in YouTube Studio. The next sync changes the occurrence's effective start while preserving its public GUID and Sunday's preacher. A calendar subscriber sees the same event UID updated. At 09:00 local no live badge appears. At actual live status, Ruth's page links to the occurrence. The church submits the authorized original, and the component creates one sermon with the selected host and preacher. The YouTube VOD may remain an alternate. Uploading a second rendition attaches to the same sermon.

If the editor changes ownership to Manage here, show a diff and save a last-common state before writing. A previously imported remote event must be reused. Never make a second public YouTube event for Y1 just because the local default provider is Mux.

## Three churches, simultaneous broadcasts, signed callbacks

Church North and Church East have different Mux environments. Both use an input called “Main”. West uses its own Cloudflare account. A visitor with access only to North must never see East's speaker or private thumbnail. The same human-readable input label and overlapping time have no identifying power. Use connection namespace and actual remote ID, then a confirmed occurrence.

At 08:55 North begins rehearsal. East goes live at 09:00 and North at 09:05. One callback for East arrives twice and a delayed idle signal for North's prior Sunday arrives after 09:05. The results are exactly two confirmed live services in an authorized site-wide list, one North service in North's module, one badge per assigned preacher, and no association with the prior Sunday's asset. A member losing access during playback cannot receive a renewed signed playback token. West's failed export has no effect on North or East.

## A provider accepts a create request but times out

1. Editor saves Sunday occurrence revision 3. Transaction records intent “create event for destination D/revision 3” and the expected immutable owning church.
2. Worker A obtains a lease and sends the create. The provider creates E7. The response times out before the external ID reaches Joomla.
3. Worker B sees the uncertain result. If the API supports the original idempotency key, reuse it. Otherwise use supported correlation metadata or the authorized recent event list to locate E7. Match by unique stable identity. Two Sunday broadcasts with matching titles require human selection.
4. Editor changes start time while resolution is pending. Keep revision 4 queued after the original create. Once E7 is adopted, apply a supported update for revision 4. Superseded intent must not create another event.
5. Provider delivers callback before the local mapping arrives. Store, authenticate and park it. After linking E7 replay the receipt, update its actual state and acknowledge completion in history. No duplicate post or invisible failed service remains.

## Local and remote edits disagree

| Local change since baseline | Remote change since baseline | Resolution |
| --- | --- | --- |
| Changed preacher | Changed event title | Keep both. Preacher is Joomla-owned. Reflect title according to calendar owner. |
| Changed time to 10:00 | Changed same service time to 11:00 | Mark time conflict, show baseline and both zones, require explicit choice. Avoid rewriting other dates. |
| Service changed to member-only | Provider unexpectedly made public | Stop issuing/embedding public access under a member-only promise. Show urgent privacy conflict and authorized remedy. |
| Cancelled Thursday exception | Provider only supports deletion of all recurring events | Preserve Joomla exception, flag provider divergence. Never delete the entire series. |
| Editor linked an existing sermon | New audio asset arrives | Keep chosen sermon, attach valid audio and preserve editor's title and publication status. |
| Local export complete, replay embargoed | Provider posts a public VOD link | Store evidence for editor, observe local embargo on all public views. Warn that a public provider can still be visited independently. |

## Calendar read/import/export for existing workflows

A provider API, a church's published iCalendar feed, and an editor can all describe the same service. Require one declared schedule owner and an explicit binding when importing duplicates. Optional read-only ICS subscription supports DTSTART/DTEND or DURATION, TZID/VTIMEZONE, RRULE, RDATE, EXDATE, detached RECURRENCE-ID, STATUS, SEQUENCE and LAST-MODIFIED where present. Resolve all-day entries as date announcements unless an explicit service time is supplied. A floating time requires the editor to choose its intended church zone. Never guess where a floating entry occurred.

Poll using conditional HTTP requests where available, handle feed pagination/size caps and timezone parsing, and retain a last-good projection after a truncated or unavailable response. A missing event on one failed or limited fetch is not a cancellation. Use explicit cancellations or confirmed removal within a known complete window. Preserve source UID plus recurrence ID, owner and feed namespace. Identical UIDs in two feeds require explicit reconciliation. Authenticate imported feeds where appropriate, keep secret subscription URLs private and reject unsafe redirects.

A public “Add to calendar” export uses a stable opaque UID/DTSTAMP for the authorized event, safe URL to the church page, correct timezone/UTC and updated SEQUENCE for edits or cancellations. Do not include stream keys, member-only invites or private notes. Private subscriptions need their own scoped revocable token and cache policy. A static downloaded calendar file cannot receive later changes. Label it differently from a live subscription.

## An external-only church still gets a full service

A volunteer pastes the Telegram live invite, selects Sunday programme and uses the existing church microphone recording. On the site, a countdown runs until due. Without provider status, the authorized operator marks the service live. The preacher page displays a link to its church service page and the page offers the Telegram button. When finished, the volunteer uploads the audio original. The same archive flow verifies the file, creates a draft sermon with that Sunday's speaker and church, and plays the approved audio from Joomla. Neither Telegram API access nor a video replay is a prerequisite.

See [iCalendar](https://www.rfc-editor.org/rfc/rfc5545).
