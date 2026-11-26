# Live streaming for Sermon Distributor

A church connects its own provider account, keeps its existing encoder if it wishes, and publishes a stable live-service page on its Joomla website. Mux is the first integration offered. Churches can use another provider, keep their own recordings, serve a local replay, or send viewers to an external service when that is the supported viewing experience.

This repository specifies the feature for the future agent extending the supplied Sermon Distributor JCB blueprint. It covers church administration, provider APIs, local media, Joomla presentation and acceptance. It contains no implementation or hosted streaming infrastructure.

## Start here

| Reader or decision | Read |
| --- | --- |
| Understand the product and the church's choices | [Product contract](docs/product-contract.md), [ownership and lifecycle diagrams](docs/system-boundaries.md), [administrator screens](docs/authoring-and-settings.md), [service-day walkthrough](docs/volunteer-and-support.md) |
| Begin authoring in the supplied JCB package | [Blueprint handoff and existing data](docs/blueprint-handoff.md), [native record and code ownership](docs/blueprint-record-map.md), [delivery sequence](docs/implementation-sequence.md) |
| Choose or adopt a provider | [Capability matrix](providers/README.md), [Mux default integration](providers/mux.md), [operation-level API reference](providers/operation-reference.md), [meeting and file integrations](providers/meetings-and-originals.md) |
| Enter a service once and synchronize it | [Calendar, exceptions and timers](docs/calendar.md), [worked synchronization examples](docs/synchronization-examples.md) |
| Show the service and the preacher on the church website | [Player, links, menu items and countdown modules](docs/playback-and-joomla.md) |
| Create one sermon and keep local audio/video | [Recording and attribution](docs/recording-and-sermon.md), [capture and local delivery](docs/capture-on-church-host.md) |
| Implement durable state and protect the church's media | [Data relationships](docs/data-contract.md), [field dictionary](docs/field-dictionary.md), [jobs and recovery](docs/operations.md), [access and privacy](docs/security-and-privacy.md) |
| Review or certify the result | [Requirements crosswalk](acceptance/traceability.md), [acceptance cases](acceptance/use-cases.md), [provider certification](acceptance/provider-tests.md), [normalized example traces and instructions](acceptance/fixture-notes.md) |

## The service lifecycle

1. **Connect or adopt.** A church administrator chooses an existing resource or explicitly creates one. The integration discovers the account's actual capabilities. A simple viewing link can be configured without API keys.
2. **Schedule.** An editor chooses host church, preacher, local timezone and recurrence. Joomla creates remote dated events where supported or follows the provider's existing calendar. Reusable stream inputs keep their existing keys.
3. **Go live.** Provider evidence or an authorized volunteer confirms the service. The preacher's page shows a live badge linked to its church occurrence. The visitor gets a permitted church player, official embed, or labelled watch/listen button.
4. **Archive.** The church can retain a provider replay, verified local video/audio, both, or neither. A background job handles authorized exports. A church can also supply its own original recording.
5. **Publish.** One occurrence produces one automatic primary sermon with the existing church and preacher relationships. A second audio/video track, reconnect or duplicate webhook cannot create a second sermon. The church's access and editorial policy control release.

## Evidence and boundaries

Provider references were reviewed on 26 November 2026. Links in each provider chapter identify the particular API or policy behind its design. Documented capability and a successful test in a church's actual account are distinct evidence. Scope or plan restrictions, regional behaviour, host capacity, changes to provider APIs and the precise incoming JCB package revision are resolved during the documented implementation and certification workflow.

Provider-specific coverage includes Mux, Cloudflare Stream, AWS IVS, YouTube, 5centsCDN, Telegram, Vimeo, Twitch, Facebook, Restream, PeerTube, Castr, Dacast, Wowza, church hosts, existing self-hosted services and meeting platforms. The generic HLS, DASH, direct audio/video, approved embed and external-link profiles extend coverage to providers without a dedicated adapter. A working external link alone does not claim provider scheduling, live detection or recording export.
