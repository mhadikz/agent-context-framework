# Data Register

The owning context fills a row before the column ships.

| Data | Context | Why we store it | Who can read it | Retention | How we delete it |
| --- | --- | --- | --- | --- | --- |
| Account email, password hash | Identity | Sign-in | Identity context, the account owner | Life of the account | Identity deletion workflow, including sessions |
| [Field] | [Context] | [purpose] | [roles] | [duration] | [path] |

## Rules
- Collect the minimum. A new personal field needs this row and a threat note in the pull request.
- Another context does not read this table. It asks the owning context.
- Store instants in UTC. Store money as minor units plus a currency.
- Debug logs are not the audit log. Financial and admin changes go to the audit stream: who, what, target, when, where.
- Deletion must be possible, including backups inside the published retention window.
