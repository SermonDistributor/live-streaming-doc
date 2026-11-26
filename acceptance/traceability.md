# Requirements to verification

| Church outcome | Specification | Minimum demonstration |
| --- | --- | --- |
| Install without operating a streaming server; default to church-owned Mux | [Product contract](../docs/product-contract.md), [Mux](../providers/mux.md), [settings](../docs/authoring-and-settings.md) | EX01–EX04 and Mux provider cases |
| Continue existing workflows or use a different provider | [Provider matrix](../providers/README.md), [provider operations](../providers/operation-reference.md) | CA09, PL03–PL06 and one working link-only, embed, native-media and API-owned account |
| A church may keep audio, video or both locally and play them from its server | [Recording and sermon](../docs/recording-and-sermon.md), [capture modes](../docs/capture-on-church-host.md) | AR01–AR06, AR12–AR15 and protected Range/seek test |
| One sermon linked to correct preacher and host church | [Field dictionary](../docs/field-dictionary.md), [JCB record map](../docs/blueprint-record-map.md), [archive](../docs/recording-and-sermon.md) | AR01, AR07–AR11 and AR16–AR19 |
| Preacher page shows live status and a link; Joomla menus/module support | [Playback](../docs/playback-and-joomla.md), [native views](../docs/blueprint-record-map.md) | PL01–PL13 and existing menu/SEF regression |
| Several countdowns, recurring days, next/all/pinned/rotation | [Calendar](../docs/calendar.md), [worked examples](../docs/synchronization-examples.md) | CA01–CA08, CA12, CA16 and PL12 |
| Schedule only once wherever supported; import existing event or handle provider exceptions | [Sync examples](../docs/synchronization-examples.md), [provider-specific operations](../providers/operation-reference.md) | CA09–CA15 and one read/create/update/readback provider |
| No duplicate events or sermons during failures, retries and simultaneous services | [Persistence](../docs/data-contract.md), [operations](../docs/operations.md) | CA08–CA11, OP01, OP04, AR07–AR08 and AR16 |
| Keep access, privacy, portability and church ownership | [Security](../docs/security-and-privacy.md), [data contract](../docs/data-contract.md) | EX05, EX10, PL06, PL10–PL11, OP02–OP10 |
| Extend supplied blueprint and verify reproducible Joomla delivery | [Blueprint handoff](../docs/blueprint-handoff.md), [sequence](../docs/implementation-sequence.md) | Existing installation upgrade, fresh import/build, smoke tests and redacted real-account evidence |

The detailed case IDs appear in the [acceptance catalogue](use-cases.md). The [provider certification plan](provider-tests.md) defines separate account- and plan-dependent evidence. Keep unsupported or untested capabilities visible in release notes and the connection health screen.
