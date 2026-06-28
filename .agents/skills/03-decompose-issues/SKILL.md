---
name: 03-decompose-issues
description: Decomposes PRD into atomized technical issues with a focus on testability and validation.
---

# Issue Decomposer

Break the PRD into independent, atomized technical tasks.

## Rules
0. **Source of Truth**: Generate one independent issue file for every task listed in the `TASK_LIST` section of the PRD.
1. **File Pathing**: Use the format `---FILE: docs/issues/<feature-context>/<issue-name>.md---`.
2. **Required Frontmatter**: Each issue file MUST start with the following frontmatter:
    ```yaml
    ---
    status: todo # Options: todo | in_progress | done
    depends_on: [previous-issue-id] # Reference other issue filenames without extension
    type: domain # Options: domain | application | infrastructure | e2e
    ---
    ```
3. **Structure**: Each issue must include: 
    - **Context**: Why this task exists.
    - **Technical Requirements**: Precise implementation details.
    - **QA & Validation**: Mapping from PRD `QA Criteria` + specific testing steps.
    - **Observability Check**: What logs or metrics should appear after this task is done?
4. **Ubiquitous Language**: Cross-reference `docs/ubiquitous_language.md`.
5. **Architectural Isolation & SDLC Types**:
    - Decompose tasks strictly along Hexagonal / Clean Architecture boundaries and testing phases:
      - **`type: domain`**: Pure business logic (Entities, Value Objects, Domain Services). Target: `src/domain/`. MUST be tested via TDD.
      - **`type: application`**: Use Cases (Interactors) and Ports (interfaces). Target: `src/application/`. MUST be tested via TDD.
      - **`type: infrastructure`**: DB schema definitions, repository implementations, controllers, route handlers, or external clients. Target: `src/infrastructure/`. Tested via integration/contract testing.
      - **`type: e2e`**: End-to-end user journey tests. Target: `tests/e2e/`. Tested via the `e2e` skill.
    - Ensure infrastructure/e2e issues depend on application/domain issues via the `depends_on` frontmatter.

## QA-Driven decomposition
For each issue, you MUST generate a **"How to Verify"** section:
1. **Unit/Integration**: What specific logic needs coverage? (e.g., "Mock DB error and verify retry logic").
2. **Manual/Automated Step**: A step-by-step guide for a human or agent to verify the change.
3. **Negative Test**: What input should cause a controlled 4xx/5xx error?
4. **Boundary Check**: If the task involves data, specify the limits to test.

## Observability Requirement
If the task involves logic or data flow, the issue MUST include a requirement for:
- **Logging**: Specific log message or event name.
- **Metrics**: Which counter or histogram must be updated.

## Limitations & Safeguards
- **Zero Redundancy**: Do NOT create overlapping issues. Every task in the PRD `TASK_LIST` must correspond to exactly one issue file.
- **Strict Dependency Tree**: Ensure all issues have correct `depends_on` values referencing parent issue ids. An infrastructure issue must never be marked as independent if it relies on application/domain entities.

## Pipeline Transition
- When the issues are decomposed, you MUST execute `node .agents/scripts/sdlc.js transition` from the workspace root to validate and transition to the next phase.