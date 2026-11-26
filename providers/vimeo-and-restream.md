# Vimeo Live and Restream

## Vimeo Live

Use the ongoing **event iframe** for live viewing. Vimeo distinguishes its recurring event from the current video clip. A clip embed can become last week's replay when the recurring event starts again. Accept the official event URL/iframe, preserve privacy hash and domain restrictions, and store the canonical event URI. The public watch link is the alternative when embedding is unavailable.

Vimeo distinguishes the plan that may host live events from **Live API** access, which its support guide assigns to Enterprise with Events. Do not promise API authoring just because an editor can schedule a recurring event in Vimeo's dashboard. Show manual embed and local countdown when only the dashboard is available.

| Operation | Contract |
| --- | --- |
| Provision | For an eligible church token, authenticated POST /me/live_events. Request link, uri, streamable_clip and stream_privacy.view plus supported event fields. Store the stable event URI and returned authorized embed. |
| Import/follow calendar | Retrieve the existing event by its returned URI and current recurrence/occurrence information available to the church. Store remote identity alongside Joomla date, time and ownership mode. Use the event's time attributes supported by the approved API version. |
| Manage calendar | Use the live event create/edit operations only for fields writable by the tenant. Preview single-date vs series scope. If a particular repeat rule is available only through Vimeo's UI, use follow mode or map independently supported one-off events; report unsupported exceptions. Do not automate the dashboard via scraping. |
| Detect active clip | Before/during broadcast use streamable_clip, linked to the current session. After the event use live_clips for completed clips. Match the saved clip ID; the next streamable_clip may already refer to a new service. |
| Archive | Resolve the completed clip /videos/{id}. Use owner-authorized file links from the API with public, private and video_files scopes when entitled. Use download/play/files according to their semantics and expiry. Respect privacy and domain restrictions. Store the video URI, never an expiring CDN redirect target. |
| Fallback | If export unavailable, link the approved Vimeo replay or upload church-owned original. If viewer domain/visibility prevents embedding, show the authorized destination, or no link when church policy forbids external viewing. |

Test a recurring event twice using the same iframe, two distinct clip IDs, and a local archive that remains correct after the third event is scheduled. Test a plan that can embed but has no Live API. Confirm that an event edit cannot alter archived sermon metadata.

References: [Live API entitlement](https://help.vimeo.com/hc/en-us/articles/12427830091921-How-to-access-the-Live-API), [live API and clip mapping](https://help.vimeo.com/hc/en-us/articles/12427783601937-How-to-use-the-Vimeo-Live-API), [event creation](https://developer.vimeo.com/api/live/events), [live endpoints](https://developer.vimeo.com/api/reference/live), [recurring scheduling](https://help.vimeo.com/hc/en-us/articles/36629220672401-How-to-create-a-recurring-live-event), [website embed](https://help.vimeo.com/hc/en-us/articles/12426942285841-How-to-embed-my-live-event-on-my-website), [file download links](https://help.vimeo.com/hc/en-us/articles/12427806914577-About-video-file-download-links-from-the-API).

## Restream

This integration can preserve a church's existing multi-destination workflow. Map the Restream event once as the production owner; attach its downstream YouTube, Facebook or other destinations as viewing alternatives. Creating events directly on those providers as well as via Restream can duplicate services. Let the church choose who owns each remote event.

1. Adopt the church's Restream account via OAuth, or paste the church's existing Website Video Player. Website playback is a paid feature. A documented website iframe and a verified destination page are appropriate player/link choices. Do not infer a playable HLS URL from a Restream ingest address.
2. For an account authorizing the Events API, POST https://api.restream.io/v2/user/events/new with streamType=encoder, title, description and scheduledFor in UTC. Save returned event ID; add existing authorized destinations using the Events API. Never replace a church's existing multistream lineup as a side effect of import.
3. Reconcile using Upcoming Events, In Progress Events, Event and Events History operations. Page through history when adopting a previous recording. Daily/weekly recurrence is a Joomla programme; materialize remote events only inside the supported window. At the reviewed Events index, creation, event retrieval and destination changes are documented. Gate reschedule/cancel separately rather than treating removal of a destination as deletion of an event.
4. Use Event Stream Key only for an assigned broadcaster; viewer pages receive only the website player or public destination URL. If no website player entitlement is available, show an approved downstream embed or external link chosen by the church.
5. After the event, use Event Recordings, select the video/audio track available for this account, and use Recording Download URL. Download before the account-specific retention period expires; persist ID and re-request temporary URLs when necessary. Use one sermon despite simultaneous destinations, multiple studio tracks or repeat signals.

Acceptance: existing multi-destination event imported without creating new posts; a changed downstream destination is reflected after sync; website player unavailable but the authorized alternate still works; broadcast recording arrives before the church selects a preacher and stays pending until assigned.

References: [Events API and operations](https://developers.restream.io/events), [create an event](https://developers.restream.io/events/event-create), [website player](https://support.restream.io/en/articles/4996608-stream-to-your-website), [recording exports](https://support.restream.io/en/articles/4379939-download-your-recordings).
