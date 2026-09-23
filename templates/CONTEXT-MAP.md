# Context Map

Use this file only when the system has more than one bounded context. A small or single-team product does not need it.

| Context | Owns | May call | Must not touch |
| --- | --- | --- | --- |
| [Name] | [Tables or aggregates] | [Other context's facade or contract] | [Another context's tables] |

A change to this map is an ADR in `DECISIONS.md` before the code lands.
