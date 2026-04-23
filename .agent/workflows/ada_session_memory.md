---
name: ada-session-memory
description: Persist conversation context into structured project memory for reuse in next sessions.
version: 1.0
trigger: manual
agents:
  - orchestrator
  - analyst
  - archivist
---

objective:
  Extract, structure, and persist reusable session memory into project storage.

inputs:
  - conversation_history
  - project_name
  - module_name (optional)

process:

  - step: summarize_context
    agent: analyst
    action: |
      Analyze full conversation_history.
      Extract only reusable technical context.
      Ignore casual dialogue.
      Produce structured output in this format:

        Executive Summary:
        Architecture Decisions:
        Technical Constraints:
        Assumptions:
        Risks:
        Open Questions:
        Action Items:

  - step: validate_memory_quality
    agent: orchestrator
    action: |
      Ensure:
      - No duplication from previous logs
      - No trivial content
      - Content is implementation-relevant
      - Clear, concise, non-narrative

  - step: persist_memory
    agent: archivist
    action: |
      Save output to:

      .ai/session-log/{{date}}-{{module_name|default("general")}}.md

      Rules:
      - Append if file exists
      - Add date header
      - Never overwrite previous logs

outputs:
  - memory_file_path
  - summary_block