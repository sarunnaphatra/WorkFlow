---
name: ada-multi-agent-orchestrator
description: "Central workflow controller for ADA multi-agent execution. Handles dynamic skill discovery, agent spawning, scoped execution, QA governance, parallel coordination, and structured delivery."
---

# WORKFLOW: ADA MULTI AGENT

TRIGGER: /manage-flow

PHASES:
1. TASK INTAKE
2. SKILL DISCOVERY
3. AGENT SPAWN
4. EXECUTION
5. REVIEW
6. DELIVERY
7. LOGGING

EXECUTION RULES:
- 1 Skill = 1 Agent
- Strict domain isolation
- Parallel execution allowed if no dependency conflict
- QA validation required before delivery
