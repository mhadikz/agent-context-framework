# Data Register

The owning context fills a row before the column ships.

| Data | Context | Why we store it | Who can read it | Retention | How we delete it |
| --- | --- | --- | --- | --- | --- |
| Account email, password hash | Identity | Sign-in | Identity context, the account owner | Life of the account | Identity deletion workflow, including sessions |
| [Field] | [Context] | [purpose] | [roles] | [duration] | [path] |

## Rules
- Collect the minimum. A new personal field needs this row and a threat note in the pull request.
- Other contexts do not query this table. They use the facade or the versioned contract.
- Store instants in UTC. Store money as minor units plus a currency. Passwords use argon2 or bcrypt.
- The audit stream records financial and admin changes. Debug logs do not.
- Deletion includes backups inside the published retention window.
