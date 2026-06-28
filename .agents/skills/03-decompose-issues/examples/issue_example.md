---
status: todo
depends_on: []
type: domain
---

# Issue: Define User Aggregate & Validation Invariants

## Context
We need to model the domain entity representing the user registration state inside the domain layer of the User Service.

## Technical Requirements
- Implement the `User` class inside `src/domain/user.ts`.
- Invariant rules:
  - Email format verification (must contain `@` and a valid domain suffix).
  - Password strength validation (minimum 8 characters).
  - Status transitions: `pending` can transition to `verified`, but once `verified`, it cannot return to `pending`.

## How to Verify
- **Unit/Integration**:
  - Test instantiation with invalid email formats (expect `ValidationError`).
  - Test state change transitions from `verified` to `pending` (expect `InvalidOperationError`).
- **Manual/Automated Step**:
  - Run the unit test suite: `npm run test:unit tests/unit/user_test.ts`.

## Observability Check
- **Logging**: Emit a debug log `"User aggregate initialized"` containing the hashed user email on successfully running the constructor.
