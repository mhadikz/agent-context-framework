# Data Register

| Data | Why we store it | Who can read it | Retention | How we delete it |
| --- | --- | --- | --- | --- |
| Account email and password hash | Sign-in | The account owner, the app database role | Life of the account | Delete the user row and sessions |
| [Other personal field] | [purpose] | [who] | [duration] | [path] |

## Rules
- Store instants in UTC. Store money as minor units plus a currency.
- Passwords are hashed with argon2 or bcrypt. Tokens and raw payment data are never logged.
- Local, CI, and production use PostgreSQL. Test data does not live in the production database.
