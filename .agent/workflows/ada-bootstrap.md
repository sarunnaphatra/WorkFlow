---
name: ada-bootstrap
description: Load and reconstruct project memory state at the beginning of a new session.
version: 1.0
trigger: manual
agents:
  - orchestrator
  - analyst
---

objective:
  Rebuild working context from persisted session logs before executing new tasks.

inputs:
  - current_task

process:

  - step: read_memory_logs
    agent: analyst
    action: |
      Read all files in:
        .ai/session-log/

      Sort by date (latest first).
      Ignore corrupted or empty files.

  - step: extract_relevant_state
    agent: analyst
    action: |
      From logs extract:
        - Latest Architecture Decisions
        - Active Technical Constraints
        - Unresolved Risks
        - Open Action Items

      Discard outdated or completed items.

  - step: build_working_context
    agent: orchestrator
    action: |
      Construct working memory state.
      Align context with current_task.
      Do not assume knowledge outside stored logs.
      Output: "Context Loaded" before executing next workflow.

outputs:
  - working_context_summary
  - context_status