# Using the normalized traces

[normalized-traces.json](normalized-traces.json) contains independent documented business scenarios. Each example is a time-ordered list of observations with expected final outcomes. They serve as reusable acceptance input for a future adapter-independent harness. They are not vendor webhook schemas or HTTP requests.

## Fixture setup

Create two independent churches labelled north and east with valid application GUIDs. Give north a weekly Sunday programme for preacher ruth, Friday and Wednesday services, and an alternate preacher paul. guest-east is a real preacher associated with east who can be explicitly assigned at north. Create a distinct private member/guest ACL fixture. Seed only the connections and resources named in each case. Use isolated database and private media roots per example. Synthetic labels stand for stable test identities.

Set programme zone to Africa/Windhoek. Timestamps in the JSON are ISO UTC. A Sunday 07:00 UTC start is 09:00 in the church. The alternate schedule example intentionally moves to 08:00 UTC. For an archive that needs verification, use tiny test-owned media with known valid tracks, duration and digest or a fake store that returns an explicit verified result. Keep file/container checks in separate real-media tests.

## What each observation means

* receipt is a fixture-local delivery label. For duplicate_of, re-deliver the earlier observation referenced by subject using its same provider receipt identity. Do not invent a second provider business event.
* observed_at is the arrival/observation time. In delayed_prior_end, the source refers to an older session whose real end preceded the current live session. Set the provider occurrence time accordingly even though the receipt arrived later.
* subject identifies the resource or chosen value. Context identifies session, revision or service. The fixture adapter resolves it through the seeded church/connection mapping rather than comparing printed names.
* pending or failed archive observations must leave full-service media unpublished under the case's configured verified-local policy. A verified_* event marks a worker's local verification milestone. It cannot be manufactured from an arbitrary public callback.
* For Mux rehearsal observations, test maps to a separate test resource/session from the real service even where the same church uses a familiar input label. Never publish that test asset as a production sermon.
* Count only primary automatic sermons created by this scenario. For the existing-sermon case in the acceptance catalogue, assert that the existing identity was retained and no new sermon was created.

## Ways to exercise the traces

Run each once in order. Replay its receipts, or interleave poll and callback for the same known session, then compare identities and counts against the same expected result. Restart the worker at a persistence boundary and repeat. For the two-source case also reverse the order in which eligible recordings arrive. A final outcome may be identical even when the set of transient status messages differs.

Keep a separate provider contract test that validates raw signatures and response parsing using officially documented shapes and authorized sandbox captures. Follow that with an installed Joomla end-to-end test through the real menu, preacher page, queue and existing file/sermon models. Record which level was actually exercised.
