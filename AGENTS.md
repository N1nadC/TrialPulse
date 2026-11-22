# TrialPulse - Repository Instructions

## Project Context

TrialPulse is a Salesforce developer portfolio project focused on Apex, LWC, SOQL, data modeling, automation, and testing.

The authoritative project specification is:
docs/project-blueprint.pdf

Follow the blueprint. Do not invent requirements, fields, API names, or business rules.

## Development Rules

1. Inspect existing files before modifying them.
2. Follow the existing project structure and naming conventions.
3. Use Salesforce DX source format.
4. Keep Apex logic modular and maintainable.
5. Apply bulkification to Apex triggers and classes.
6. Respect Salesforce governor limits.
7. Follow secure Apex practices, including sharing and CRUD/FLS enforcement.
8. Use meaningful names and comments where they improve clarity.
9. Do not introduce unnecessary dependencies or features.
10. Never use real patient or clinical data. Use synthetic data only.

## Apex Standards

- Separate trigger logic from business logic.
- Prefer selector, service, handler, and controller patterns where appropriate.
- Handle collections and bulk operations.
- Write meaningful positive, negative, and bulk test scenarios.
- Do not rely only on code coverage; verify behavior.
- Do not use static Boolean recursion guards.

## LWC Standards

- Use the Lightning Web Components framework.
- Keep components focused and reusable.
- Use appropriate Apex controllers and error handling.
- Follow Salesforce naming and metadata conventions.

## Working Process

1. Read the relevant blueprint section.
2. Inspect related existing files.
3. Explain the implementation plan before making changes.
4. Make only the changes required for the current task.
5. Explain every created or modified file.
6. Run relevant tests or validation when possible.
7. Report what passed, failed, or remains unverified.

## Important Restrictions

- Do not deploy to Salesforce without explicit approval.
- Do not delete or overwrite existing work without approval.
- Do not mark a task complete unless it has been verified.
- Do not silently change the approved project architecture.
- Explain important code so the developer can understand and defend it in interviews.
