---
id: 2
title: Global constitution phase I-V
stage: constitution
date: 2024-12-24
surface: agent
model: claude-sonnet-4-5-20250929
feature: none
branch: master
user: N/A
command: sp.constitution
labels: ["constitution", "multi-phase", "global", "governance"]
links:
  spec: null
  ticket: null
  adr: null
  pr: null
files:
  - .specify/memory/constitution.md
  - .specify/templates/plan-template.md
  - .specify/templates/spec-template.md
  - .specify/templates/tasks-template.md
tests:
  - Template validation completed
---

## Prompt

Create a single global constitution for the entire "Evolution of Todo" project
covering Phase I through Phase V.

This constitution must define:

1. Spec-Driven Development as mandatory
   - No agent may write code without approved specs and tasks
   - All work must follow: Constitution → Specs → Plan → Tasks → Implement

2. Agent Behavior Rules
   - No manual coding by humans
   - No feature invention
   - No deviation from approved specifications
   - Refinement must occur at spec level, not code level

3. Phase Governance
   - Each phase is strictly scoped by its specification
   - Future-phase features must never leak into earlier phases
   - Architecture may evolve only through updated specs and plans

4. Technology Constraints
   - Python for backend
   - Next.js for frontend (later phases)
   - FastAPI, SQLModel, Neon DB
   - OpenAI Agents SDK, MCP
   - Docker, Kubernetes, Kafka, Dapr (later phases)

5. Quality Principles
   - Clean architecture
   - Stateless services where required
   - Clear separation of concerns
   - Cloud-native readiness

This constitution must remain stable across all phases
and act as the supreme governing document for all agents.

## Response snapshot

Updated constitution from Phase I-only to global constitution covering Phase I-V (Version 2.0.0).

**Core Principles (4)**:
1. Spec-Driven Development - Constitution → Specs → Plan → Tasks → Implement flow mandatory
2. Agent Behavior Rules - No manual coding, no feature invention, no deviation, spec-level refinement
3. Phase Governance - Strict phase boundaries, phase-first design, scope locking
4. Quality Principles - Clean architecture, stateless services, separation of concerns, cloud-native readiness

**Technology Stack Defined**:
- Backend: Python, FastAPI, SQLModel, Neon DB
- Frontend: Next.js, TypeScript
- Agent/Integration: OpenAI Agents SDK, MCP
- Infrastructure: Docker, Kubernetes, Kafka, Dapr

**Phase Technology Matrix**: Defined technology availability across Phases I-V (Phase I: Python, CLI, in-memory; Phase II: +FastAPI; Phase III: +SQLModel, Neon DB, OpenAI Agents SDK, MCP; Phase IV: +Next.js, Docker, Kubernetes, Kafka; Phase V: +Dapr)

**Governance**: Amendment procedure, compliance review, constitution supremacy, scope boundaries for Phases I-V

Template validation: No updates required (templates remain generic and compatible)

## Outcome

- ✅ Impact: Global constitution established for entire Evolution of Todo project; replaced Phase I-only constraints with multi-phase governance
- 🧪 Tests: Template validation completed; no conflicts with existing templates
- 📁 Files: Updated `.specify/memory/constitution.md` (v2.0.0)
- 🔁 Next prompts: Create Phase I feature spec using `/sp.spec <feature>` (must respect Phase I technology constraints)
- 🧠 Reflection: Successfully expanded from single-phase to multi-phase governance; Phase Technology Matrix provides clear technology progression; constitution now acts as supreme governing document for all phases

## Evaluation notes (flywheel)

- Failure modes observed: None
- Graders run and results (PASS/FAIL): N/A
- Prompt variant (if applicable): N/A
- Next experiment (smallest change to try): N/A
