# Data Register

| Data | Why we store it | Who can read it | Retention | How we delete it |
| --- | --- | --- | --- | --- |
| Account email and password hash | Sign-in | Account owner, app role | Life of the account | Delete user, sessions, and owned rows |
| [Module record] | [purpose] | That module's facade only | [duration] | [path] |

## Rules
- The module that owns the table records any personal data it adds.
- Store instants in UTC. Store money as minor units plus a currency.
- Admin writes and permission changes are audited: who, what, target, when.
- Logs carry ids and `traceId`. They omit passwords, tokens, and raw payment data.
