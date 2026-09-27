---
name: bacon-review
description: A structured approach to review code and take opportunities to improve. Use when major bugs were fixed, libraries got updated, big refactors or code changes took place or on a regular basis/ when asked.
---

# Bacon Review

Stories, Chores, and Bugs are tracked in the project's bacon-tracker.
Create findings as chores in the icebox, then prompt to iterate over them.
Pull requests will not be merged until all Findings are closed (if applicable).

For each finding include:
- **Severity** (CRITICAL/ WARNING/ SUGGESTION)
- **Location** (file: line)
- **Issue**
in the existing templates.

## Discovery Categories

> Using the `team-anchor` subagent

### Architecture
- All implementations follow Clean Architecture guidelines (logic in use-cases not in controllers or views, etc.)
- All implementations are accessible from all interfaces (e.g. API and Web) if not stated otherwise

### Specs
- Feature file exists and describes the full scope
- Specs were written before implementation (red-green-refactor)
- Edge cases and failure paths are covered
- No specs are marked skip/ pending without a documented reason
- Coverage is sufficient for an outsider to understand the feature from specs alone

### i18n
- No user-visible strings are hardcoded in views or controllers
- All new i18n keys exist (e.g. in a each locale YAML file - no missing keys)
- Date, time, number and currency values use localization helpers

### Security
- No SQL/ NoSQL injection
- No endpoints are publicly accessible that should require authentication
- No sensitive data (tokens, passwords, PII) appears in commit messages, logs or error messages
- No hardcoded secrets or API keys in source code
- No input is unvalidated
- New models with sensitive data have appropriate access controls
- No obvious OWASP top 10 vulnerabilities introduced (SQLi, XSS, CSRF, mass assignment)
- No CORS issues
- Security audit log captures all relevant events

### Error Handling
- Async operations in try/ catch or similar
- Errors logged with context
- Consistent Error Format
- No empty catches

### Performance
- N+1 queries
- Missing indexes
- Unbound queries
- Large payloads without pagination

### Code quality
- No code duplication
- No corners were cut
- No dead code, TODOs, skipped specs or commented-out code (without good and communicated reason)
- Code is readable and maintainable (nothing clever that a future developer would need to decode)

### Dependencies
- No known CVEs in any part of the code
- Flag dependencies that are more than one major version behind (e.g. `bundle outdated`)
- No dependencies were added without a clear documented reason
- No dependencies that are unmaintained or deprecated

### Completeness
- The feature works as described in the specifications

## Framing
- Present a ranked list of improvement opportunities, always include:
  - which files/ modules are involved
  - why the current implementation is causing friction
  - what should be changed
  - what are the benefits (long and short term)
- Challenge what is worth taking into implementation phase
- Do not apply the code changes yet

## Implementation
- Create the opportunities as separate chores and add them to the backlog
- Implement chores 1 by 1 (follow the `bacon-workflow` skill)

## Cleanup
- The previous vs. improved solution is presented in the commit/ PR message
- Document lessons learned (what are the improvements?)
