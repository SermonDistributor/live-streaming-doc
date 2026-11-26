# Proposed field contracts and consistency checks

This is a logical dictionary for mapping to JCB Fields. Reuse equivalent existing Fields and the component's chosen field storage conventions. Precision/size examples describe the value to preserve. They do not override the imported schema for existing entities. Store timestamps as UTC with enough precision to order observations and an explicit timezone for wall-clock schedules. Preserve an original provider timestamp or revision where useful. Never truncate opaque provider IDs. Reject unsupported sizes with a diagnostic.

## Identity, scope and timing

| Logical field | Value and nullability | Constraints and use |
| --- | --- | --- |
| application GUID | Existing package-compatible UUID, required per standalone business row | Immutable. Use existing Joomla numeric id for native CRUD/router operations if appropriate, never to match records across restored installations. |
| church_guid | Required Church relationship for connections/programmes/occurrences | Valid existing GUID with write authority. Copy host identity into the actual sermon.church field at creation. |
| preacher_guid | Preacher GUID. Nullable on a draft or unassigned inbound session | Required before automatic attributed sermon publication. Resolve override before default. Public “live” link requires a confirmed subject. |
| programme_guid | Optional for an occurrence | Null means genuinely ad hoc. Do not make all ad hoc rows collide on a unique null programme key. |
| recurrence_key | Original generated local date+time and its zone/series identity | Unique within programme lineage. Immutable when an editor moves one occurrence. For detached imported occurrences also preserve external UID and RECURRENCE-ID. |
| zone_id | Named IANA zone. Required for recurring local schedules | Preserve church selection, including when the viewer's browser zone differs. |
| rule, dates, exceptions | Supported iCalendar recurrence rule and normalized additions/exclusions | Check frequency, positive interval/count, valid weekday and bound exclusivity. Keep exceptions outside the rolling materialization horizon. |
| nominal_start, nominal_duration | Local wall time and positive duration | Defaults for future generation. Changing duration does not automatically end a live broadcast. |
| scheduled_start_utc, scheduled_end_utc | Required start for scheduled occurrence. Optional end for unknown duration | Order/filter by effective instant. A revised start is independent of actual_start and recurrence_key. |
| actual_start_utc, actual_end_utc | Nullable until proven by session/owner | End must belong to same session and be consistent with start. Late delivery must not end a newer session. |
| visibility / publication / language | Native access levels, published flags and language IDs/tags | Apply at the occurrence, linked resources and media delivery path. Never treat language as access. |
| version / edited_by / updated_at | Version and protected actor/time | Support optimistic conflict detection and distinguish editor from worker-owned metadata. |

## Connections and destinations

| Field | Type and availability | Integrity rule |
| --- | --- | --- |
| adapter | Enumerated adapter key. Required | Persist versioned capability evidence separately. Renaming a display label never changes behaviour. |
| namespace | Stable account, tenant, instance or region + account tuple | Bind credentials and all remote IDs to it. Two separate church connections remain isolated even when both reference one platform. |
| credential_ref, credential_version | Nullable secret reference plus rotation version | Null for static embeds/links. New secret validated before activation. Public projection never includes these fields. |
| capability_evidence | Set of operation, outcome, reason, checked time and effective scope | Unknown, unavailable and denied are different. Re-check on entitlement/credential change. |
| role / ordering / language | Primary, alternate or audio, explicit order, optional language | Only one preferred default per audience and media mode, with deterministic tie-breaking. |
| resource_kind and remote IDs | Case-sensitive opaque strings. Optional when an external public URL suffices | Do not lowercase IDs, infer them from filenames, or combine event/channel/asset into one untyped field. |
| presentation | Typed native HLS/DASH, direct audio/video, provider SDK, approved iframe or external link | Renderer allows only a trusted descriptor for the kind. No arbitrary executable HTML. |
| public_action | Normalized, validated URL or resource descriptor | A private invitation is still subject to ACL. Signed or time-limited values are resolved at request time if appropriate. |
| calendar_owner and field ownership | Local, remote or local-only with static destination. Field-level ownership when supported | An occurrence has a canonical schedule owner. Secondary destinations receive approved projections, not conflicting writes back into the primary calendar. |
| remote_revision, last_common, desired_revision | Opaque provider version/etag, normalized last-common field snapshot, local monotonic revision | Detect missed writes, same-field conflicts, echoes and replacements. Not every provider offers ETags. |

## Capture, attachment and publication

| Field | Type/meaning | Validation |
| --- | --- | --- |
| capture_policy | none, provider archive, provider and local, local via authorized capture/original | Distinguish requested policy from effective provider behavior and record consent on mandatory cloud recording. |
| requested_tracks, required_for_publish | Video/audio/caption selections and minimum release condition | A local-audio-only sermon can publish after verified audio even if optional video is pending. “Both required” waits for both or explicit reviewer change. |
| replay_preference | Local first, provider first, local only, provider only, external only | Separately record allowed fallbacks. Local-only never silently contacts remote host when the file disappears. |
| remote_asset_kind/id, rendition | Provider-scoped asset/session and intended file/track | Idempotency identity is the asset and rendition, not a temporary URL or checksum shared by unrelated churches. |
| transfer_state, expected_size, bytes_staged | Enumerated state and nonnegative integers large enough for multi-gigabyte files | On full response to resumed request reset cursor safely. Do not infer completeness from transferred bytes without other verification. |
| source_version, expires_at | Optional ETag/resource version and export-ticket expiry | Refresh ticket without changing ownership. ETag is not assumed to be a cryptographic file checksum. |
| local_file_guid, digest, verified_at | Existing file GUID plus strong digest, recorded validation time | File's owner/entity_type/file_type and ACL must match the intended sermon. Assign final playable status only after storage/DB agreement. |
| content_type, tracks, duration | Measured MIME/container, audio/video/caption facts, seconds | Compare against expected format and authorized file type. Store why a requested audio track is absent. |
| sermon_guid / archive exclusion | Existing sermon row GUID, creation provenance, exclusion marker | Exactly one automatic primary association. Approved manual links and intentional deletion outrank retry. |
| retention / delete permission | Provider/local retention, source deletion opt-in and verified dependency check | Clock is based on asset policy and provider window. Do not delete a source at the end of a service merely because local playback is preferred. |

## Queries and bounded retention

Maintain indexes for time-bounded programme expansion, church/preacher visibility lookups, due jobs, unfinished transfers and remote reconciliation by connection/resource. For “what is live?” page reads, query the local state and apply freshness/ACL at query time. Derive the next service from eligible occurrences, never from the last successfully exported recording. Predicted future events may be regenerated. Published GUIDs, historical instances, cancellation tombstones and existing sermon mappings must survive horizon cleanup.

Keep summaries and hashes where sufficient. Apply an explicit, configurable retention range to diagnostics and raw event envelopes. Time expiry must not break durable idempotency keys needed for a later replay. If recording approval is withdrawn, remove protected media and derived versions through the corresponding native model lifecycle and record the minimal retained deletion/audit data required by church policy.
