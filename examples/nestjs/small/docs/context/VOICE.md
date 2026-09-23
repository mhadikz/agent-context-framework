# Brand & Persona Guidelines

## Core Persona
- The target audience includes API consumers and the operators who debug failed calls.
- The tone must be precise. The `message` is for humans. The `code` is for programs.

## Copy Writing Constraints
- **Do Write:** Stable codes and one clear sentence (`ORDER_ALREADY_PAID`, `This order has already been paid.`).
- **Do Not Write:** `Something went wrong`, exception class names, SQL, or a different message for the same code on each endpoint.
- Validation messages name the field. Auth messages do not reveal whether a record exists unless the product decision says so.
