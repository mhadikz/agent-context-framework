# Data Register

| Data | Why we store it | Who can read it | Retention | How we delete it |
| --- | --- | --- | --- | --- |
| Account email and password hash | Sign-in | Account owner, app role | Life of the account | Delete user, sessions, and owned rows |
| [Domain record, e.g. order contact] | [purpose] | Owning module only | [duration] | [path in that module] |

## Rules
- Each module records the personal data it introduces. Another squad does not add a column of personal data to a table it does not own.
- Store instants in UTC. Store money as minor units plus a currency.
- Audit admin edits and permission changes (who, what, target, when) without putting secrets in the audit row.
- Logs may contain ids. They must not contain passwords, tokens, or raw payment data.
