# Context Map

Each context is the only writer of its tables. The API gateway authenticates and rate-limits. Contexts still authorize.

| Context | Owns | May call | Must not touch |
| --- | --- | --- | --- |
| Identity | Accounts, sessions, roles | None for identity data | Other contexts' tables |
| [Context] | [Aggregates] | [In-process facade, or a versioned contract if extracted] | Any table it does not own |

## Rules
- A new arrow on this map is an ADR before the code.
- In-process calls use the exported application facade. Cross-process calls use a versioned contract and the outbox.
- An extracted service gets its own database. Shared tables are forbidden.
- Flat admin CRUD with no invariants stays a module, not a new row on this map, until it owns a real policy.
