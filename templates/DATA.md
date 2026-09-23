# Data Register

Record every category of personal or sensitive data. Collect the minimum.

| Data | Why we store it | Who can read it | Retention | How we delete it |
| --- | --- | --- | --- | --- |
| [e.g., account email] | [purpose] | [role or service] | [duration] | [path] |

## Rules
- Store instants in UTC. Store money as minor units plus a currency, not as a binary float.
- Secrets, tokens, and raw payment data are never logged.
- Production data and test data do not share a database.
