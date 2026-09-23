# Context Map

Next.js apps compose screens. They are not a system of record. Each context below is the only writer of its tables.

| Context | Owns | May call | Must not touch |
| --- | --- | --- | --- |
| Identity | Accounts, sessions, roles | None for identity data | Other contexts' tables |
| [Context] | [Aggregates] | [Facade or versioned contract] | Any table it does not own |
| Web app | Routing, composition, session display | Owning context facades | Domain tables and vendor SDKs |

## Rules
- A new arrow on this map is an ADR before the code.
- Cross-context calls use `packages/contracts` or the exported facade. They do not import a repository.
- An extracted service gets its own database. The row in this map changes from "facade" to "versioned contract".
