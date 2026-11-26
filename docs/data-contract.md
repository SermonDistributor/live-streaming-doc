# Persistent records, identities and invariants

These are logical names for authoring, not imposed table identifiers. Reuse an existing blueprint entity where it implements the same ownership and lifecycle. Keep numeric Joomla primary keys and the package's standard GUID, access, publication, audit, ordering, language and version conventions where relevant. A signed URL, alias, display name or date alone is never a durable join.

## Cardinalities

A church owns many connections and programmes. A programme supplies defaults for many occurrences. An occurrence can exist without a programme (ad hoc) and can have many destinations and sessions. A connection can be reused across many occurrences. One occurrence has at most one automatically created primary sermon. One sermon can own many media files. Occurrences can deliberately link an existing sermon. Two independent services delivered on one channel remain two occurrences.

| Logical record | Required and optional values | Keys, purpose and deletion |
| --- | --- | --- |
| Connection | GUID, church GUID, adapter type, label, validated base/account identifier, credential reference or null, enabled operations, environment, enabled state; optional token metadata, last validation result/time | Index church + enabled. Public link mode needs no credentials. Archive a connection with dependencies. Reauthorization must identify the same account/environment before reusing remote IDs. |
| Programme | GUID, church, default preacher, title, recurrence rule, local start clock, IANA zone, nominal duration, validity bounds, language/access, destination defaults, recording and publication defaults | At most one active revision for a generation period. End date limits new materialization. Preserve history when a programme ends. |
| Occurrence | GUID, programme or null, original local recurrence identity, effective scheduled UTC and end, event timezone, title, church, preacher, series/category, overrides, publication/access, cancellation, version, status evidence | Unique programme + original recurrence identity for generated rows; generate an independent GUID for ad hoc services. Index church/time, preacher/time and publish/access. Moving an occurrence retains its GUID and original identity. |
| Destination | GUID, occurrence, connection, role (primary/alternate/audio), order, external page or validated embed/media descriptor, room/stream/event identifiers, source of schedule, capabilities and last observation | Unique local destination identity. Scope remote identity by connection, resource kind and remote object. A reusable channel may occur many times; unique occurrence mapping applies to a remote **event/session** where appropriate. |
| Broadcast session | GUID, destination, provider session/asset references; observed start/end, last successful evidence, evidence kind and revision; reconnect relationship | Unique provider session per connection when available. One occurrence may include a restart or simultaneous destinations. Preserve original signal times and receipt times separately. |
| Archive asset | GUID, occurrence, session if known, kind (video/audio/captions), source resource ID and format, local File GUID or remote playback ID, size, MIME, duration, checksum, verification status, retention and selection | Source key covers connection + asset + rendition. Files of different kinds stay separate. Keep recording origin and local-copy verification independently. |
| Sermon association | Occurrence GUID, sermon GUID, creation mode, metadata provenance, manual edit marker | Unique occurrence association, atomically claimed. Sermon GUID joins existing Sermon. An editor can mark a service intentionally excluded or deliberately relink it with audit. |
| Remote mapping | Connection, local entity/occurrence, scope, remote ID, remote revision/ETag when offered, normalized fingerprints, last successful sync and direction | Enforce uniqueness for event claims within connection. References to per-channel settings shared by events use separate resource scope. Archive tombstones to prevent recreation after cancellation. |
| Incoming event | Provider/connection namespace, remote event ID or documented stable digest, kind, resource ID, received/provider time, verification result, bounded payload, processing state | Unique namespace + remote event key. Retain keys past the provider retry window. Store secrets, media and full HTTP headers outside this record. |
| Background operation | Type, occurrence/connection, idempotency key, desired revision, state, attempts, next attempt, lease owner/expiry, checkpoint, last failure code | Unique operation + target + desired revision. Index state + due time. Leases expire after worker loss. Durable intent must exist before a remote write. |
| Audit/change history | Actor or automation identity, action, church, local subject GUID, result, redacted diff, UTC time, correlation ID | Append-only business trail, including manual live override, relink, credential rotation and media deletion. Apply configured retention and restricted read access. |

## Field semantics that protect church ownership

* Save the occurrence's church explicitly. The connection must belong to that church or be explicitly authorized by a church-scoped sharing policy. Default to separate connections per church, including where two churches happen to use the same vendor.
* The preacher reference is the preacher row GUID. A platform's presenter name is a suggestion for editorial review. If the host changes the speaker, store an occurrence exception and record when it took effect. Do not rewrite older sermons by updating a recurring programme.
* Store zero, empty, absent and unknown according to the imported field's actual contract. Missing monitoring credentials means unknown automation capability. Missing reachability is a monitoring failure. Neither implies the stream ended.
* Resolve recording storage, public playback, and file download as three different permissions. A locally held private master can coexist with a public provider replay. Automatic imports inherit the narrower event and sermon ACLs; independently check file view/download rules.
* Store UTC instants and source timezone identifiers. Keep the original wall time for weekly generation. Keep provider account IDs, event IDs, stream IDs, playback IDs, session IDs, asset IDs and file IDs in separately typed fields or typed mappings. A single generic URL cannot restore these distinctions.
* Store secret references and key versions. Private credentials, ingest stream keys, invite hashes, signed replay URLs and OAuth refresh tokens do not belong in menu parameters, public exports, browser data, notes or exception logs. Public calendar feeds omit even a private invite's URL.
* A recording policy snapshot takes effect for that occurrence. Mid-service policy changes require an explicit choice about already recorded material. Never retroactively enable public replay or remote deletion.

## Indexes and concurrent edits

Enforce database uniqueness on the occurrence series identity, event receipt, operation intent, source rendition and automatic sermon association. A PHP pre-check alone loses a race. Use optimistic version checks for administrator saves; return which fields changed when an older editor/session attempts to overwrite them. Atomically reserve occurrence ownership and enqueue side effects within the same transaction where the database supports it. When a generated model manages its own save boundary, use a durable reservation and recovery lookup tied to the occurrence GUID.

A polling refresh and callback can observe the same recording. Both request the same source-rendition operation and converge on one File. A sermon save succeeding before the link write must be discoverable by a durable creation marker or preassigned GUID. After a timeout with no conclusive remote response, read back or reconcile before retrying a creation. Keep ambiguous remote duplicates in a review queue.

## Delete and portability rules

| Action | Result |
| --- | --- |
| Unpublish occurrence | Remove public badges, countdown and replay links according to ACL. Keep audit, recording and remote programme unchanged unless separately requested. |
| Cancel occurrence | Tombstone recurrence identity. Queue cancellation only on mapped remote events whose owner is Joomla. Preserve the original public route with a cancellation notice subject to access. |
| Disconnect account | Revoke/forget local secrets and stop automated jobs. Preserve verified local files and sermons. Show reconnect state for remote replays. Never delete all provider streams. |
| Delete sermon or attachment | Follow the existing model lifecycle. Flag the association orphaned or deliberately removed so retries do not silently recreate an editor's deletion. |
| Remove provider asset | Confirm account, asset, retention policy and verification of the required backup. Identify other occurrences/sermons referencing the same asset. Never cascade from a church-wide shared channel ID. |
| Export site settings | Export stable public configuration and relation identities. On restore, rebind environment-specific credentials and verify paths, file manifests and provider identity before enabling writes. |
