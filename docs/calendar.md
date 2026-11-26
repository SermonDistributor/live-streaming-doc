# Scheduling, recurrence, countdowns and remote calendars

## What the editor does

Choose church, preacher, programme title, local timezone, start time, expected duration and destinations. Offer one service, a weekly pattern with selectable weekdays and interval, and independent schedules. Permit a start and optional last date or occurrence count. A church can maintain Wednesday evening, Friday evening and Sunday morning as three programmes with different times, preachers, encoders and visibility. Use one programme with several weekdays only when its settings genuinely are shared. Preview the next dates in the church zone before saving. Show the viewer's local time as an optional secondary label.

Offer three ownership modes per destination. **Manage here** writes eligible changes from Joomla and adopts provider-issued IDs. **Follow provider** imports the remote event and refreshes selected fields. Editor-only sermon metadata stays local. **Local calendar with existing destination** keeps scheduling in Joomla and uses a stable channel/link. Default to the last mode for providers with no event calendar. Connecting a provider must not reschedule a broadcast already entered elsewhere.

## Occurrences and exceptions

Use a named IANA timezone and local wall time as the recurrence source. Store effective UTC instants for ordering, background jobs and public clocks. Represent recurrence by standard calendar properties such as DTSTART, RRULE, RDATE, EXDATE and RECURRENCE-ID. Use a tested recurrence library for the supported rule subset. Normalize and display imported unsupported rules conservatively: ingest concrete provider occurrences in follow mode, label the original recurrence as externally managed, and disable rule edits that would lose its meaning.

| Editor action | Local change | Remote consequence |
| --- | --- | --- |
| Change this occurrence | Persist an exception keyed by the original local identity. Keep occurrence GUID and route | Update that event if individually editable. Otherwise show local override and provider limitation before applying. |
| This and future | Split into old and new rule revisions at the selected occurrence. Preserve historical IDs. Retain already published URLs and mapped instances where possible. | Map future differences individually, or use provider series operation only when its effects match exactly. |
| Entire programme | Preview affected items. Leave completed, locked or explicitly excepted occurrences alone unless individually reviewed | Queue changes for eligible open items. Show provider restrictions and resulting conflicts. |
| Skip/cancel one | Write tombstone/exception even if the worker has not created the future row yet | Cancel only a church-owned scheduled event. Never delete a reusable input or the whole recurring event as a substitute. |
| Add an extra service | New independent occurrence, optionally borrowing programme defaults | Create an independent remote event only when supported. |
| Change timezone | Offer preserve clock time or preserve instant for materialized future items. Preview both in church and viewer zone | Update by remote event identity. Warn if a provider's timezone or recurring edit would change other services. |

Generate a rolling horizon, e.g. 8 weeks as a proposed default. Extend daily and on save. Do not create infinite remote events. For a provider with short creation windows, generate local occurrences ahead and enqueue remote creation when eligible. Keep horizon/notification timing separate. Rule edits, overlapping workers and repeated scheduler ticks must produce identical occurrence keys. A cancelled date will never regenerate under the same programme revision lineage.

For weekly rules, skip invalid generated local times under RFC 5545. Show skipped dates around daylight-saving gaps in preview and allow a deliberate replacement at a valid time. In an ambiguous fold use the first occurrence under RFC rules unless the editor creates an explicit instant-specific exception. Preserve a provider's supplied UTC instant for imported one-offs. A zone rule update affects future local projections. Audit calendar changes to already-synchronized events. Reject end before start, both COUNT and UNTIL together, absent zone for recurring times, and conflicting exception IDs.

## A worked rotation

Example zone Africa/Windhoek, UTC+2 on these dates. Three recurring programmes start Wednesday 18:30, Friday 19:00 and Sunday 09:00. Store dates in the service zone. A viewer in another timezone sees the same remaining duration.

| Observation in Windhoek | Expected next display |
| --- | --- |
| Wed 25 Nov 2026 18:29 | Wednesday service, 1 minute remaining |
| Wed 18:30, no provider confirmation | Wednesday is due, labelled “Waiting for broadcast”. Friday is the next future service. Zero alone never produces a preacher's Live badge. |
| Wed 18:34, confirmed live | “Watch live” for Wednesday. In all-upcoming mode, Friday and Sunday remain visible. |
| Wed ends and returns idle | Show replay-processing or ended for Wednesday. The rotating future countdown targets Friday at 19:00. |
| Fri 27 Nov cancelled | Skip Friday in next mode. Its own accessible route explains cancellation. Next is Sun 29 Nov 09:00. |
| Sun service ends | Next is Wed 2 Dec 18:30. Editing that Wednesday to Thursday keeps its occurrence URL and does not move all Wednesdays. |

## Countdown and status display rules

* **Next only** selects the earliest eligible future occurrence within filters. **All upcoming** lists the ordered window. **Pinned** follows one occurrence, including cancellation and replay. **Programme** restricts to that series. **Rotate** advances when the selected service ends, is skipped or is cancelled. A “live now” module can list multiple simultaneous services separately.
* Separate “next future” from “currently live”. A homepage can show both. Sort ties by configured programme ordering then stable GUID. In a next-only hero that prioritizes live, prefer an eligible live occurrence and offer other simultaneous services. An overrun never hides the next service from the calendar.
* Render an absolute date, zone and watch link on the server. Add a client countdown using server UTC and the event instant. Re-sync after tab sleep, network recovery and timezone change. Never subtract one second per browser timer callback as the authoritative clock. A large clock discrepancy triggers re-sync. Screen readers hear material transitions, not every second.
* Proposed display defaults: at due time show “Waiting for broadcast” for a configurable grace period (initially 15 minutes). After grace move the next-only countdown onward while the delayed service remains reachable. Later confirmed live restores the live badge. Expected end time alone cannot end a confirmed live event.
* Prevent personalized or private state from entering anonymous full-page/module caches. Cache static calendar content separately from short-lived status. Restrict status endpoints by the same view levels and church as the rendered page.

## Synchronization protocol

1. Read provider capabilities and normalized remote state under the selected account. On adoption show matching candidates with channel/event ID, local and remote time, title, church and visibility. A shared title or time is insufficient to auto-bind.
2. Store a mapping and a revisioned outbox item in the same local save. Compare per-field ownership. Title, timezone/start/end and cancellation can be managed. Preacher GUID, sermon approval and local archive policy remain local. Show a pending badge until provider confirmation.
3. For each operation, enforce permission, current occurrence revision, provider horizon, quota and per-resource constraints. Use native idempotency if available. After timeout, query by returned ID or durable correlation. If neither identifies exactly one creation, pause that item for review rather than creating a second event.
4. On provider success read back normalized state (providers may round timestamps or change URLs). Save the actual IDs and public descriptor. Compare mapped fields with the last common state. Suppress a callback echo that merely repeats a just-applied revision.
5. When the provider alone changes an owned field in follow mode, update that local projection. When both sides changed the same field, show old/local/remote values and a per-field resolution. Auto-merge disjoint field edits. Never resolve a public/private conflict by silently choosing the broader setting.
6. Handle permission expiry, deletion, remote time collision and quota exhaustion independently per destination. Keep the approved local event and other destinations. A locked remote series that cannot cancel Friday individually must visibly retain the difference or be detached, following the editor's choice.
7. Unscheduled incoming streams appear in the operator inbox. Automatically bind only a unique enabled resource in the correct church with an unambiguous currently eligible occurrence. A recurring channel reused for rehearsal or two overlapping services requires confirmation before sermon creation.

Sources include [iCalendar recurrence and exclusions](https://www.rfc-editor.org/rfc/rfc5545), [IANA timezone database](https://www.iana.org/time-zones), [Joomla Scheduled Tasks](https://guide.joomla.org/user-guide/scheduled-tasks).
