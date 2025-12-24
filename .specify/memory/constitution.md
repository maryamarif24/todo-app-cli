<!--
Sync Impact Report:
- Version change: 1.0.0 → 2.0.0
- Modified principles:
  - I. Spec-Driven Development (expanded with Constitution → Specs → Plan → Tasks → Implement flow)
  - II. In-Memory Storage → REMOVED (now phase-specific)
  - III. CLI-First Interface → REMOVED (now phase-specific)
  - IV. Simple Functions and Clean Architecture → IV. Quality Principles (expanded)
  - V. Python 3.13+ Only → REMOVED (merged into Technology Constraints)
  - VI. Phase I Boundaries → III. Phase Governance (expanded to multi-phase)
- Added sections: II. Agent Behavior Rules (new), IV. Technology Constraints (expanded), Phase Technology Matrix
- Removed sections: None (reorganized)
- Templates requiring updates:
  ✅ .specify/templates/plan-template.md (Constitution Check section remains generic)
  ✅ .specify/templates/spec-template.md (no constitution-specific mandates)
  ✅ .specify/templates/tasks-template.md (no constitution-specific mandates)
- Follow-up TODOs: (none)
-->

# Evolution of Todo Constitution

## Core Principles

### I. Spec-Driven Development (NON-NEGOTIABLE)
All code MUST be generated from approved specifications. NO manual coding by humans is permitted. The development flow is strictly: **Constitution → Specs → Plan → Tasks → Implement**. Code written without corresponding spec and task artifacts is invalid and MUST be rejected.

**Rationale**: Ensures traceability, prevents ad-hoc decisions, and maintains alignment between requirements and implementation. Spec-driven discipline is the foundation of multi-phase development.

### II. Agent Behavior Rules (NON-NEGOTIABLE)
All agents MUST adhere to strict behavior constraints:

1. **No Manual Coding**: Humans DO NOT write code directly. Humans define requirements, review artifacts, and approve implementations only.
2. **No Feature Invention**: Agents MUST NOT invent features, requirements, or capabilities not in approved specs.
3. **No Deviation**: Agents MUST implement specs exactly as written. Deviation without spec amendment is invalid.
4. **Spec-Level Refinement**: Any requirement clarification or refinement MUST occur at spec level, not during implementation.

**Rationale**: Uncontrolled agent behavior undermines multi-phase stability. Clear boundaries prevent scope drift and maintain architectural integrity across phases.

### III. Phase Governance (NON-NEGOTIABLE)
Each phase is strictly scoped by its specification:

1. **Phase Boundaries**: Each phase (I-V) has a defined scope. Features from future phases MUST NEVER leak into earlier phases.
2. **Phase-First Design**: Architecture evolves phase-by-phase through updated specs and plans. No anticipatory coding for future phases.
3. **Scope Locking**: Once a phase spec is approved, its scope is locked. Changes require formal amendment through spec revision.

**Rationale**: Phased evolution requires strict boundaries. Feature creep into earlier phases destabilizes development and creates technical debt.

### IV. Quality Principles (NON-NEGOTIABLE)
All code and architecture MUST adhere to quality standards:

1. **Clean Architecture**: Separation of concerns, no tight coupling, clear boundaries between layers.
2. **Stateless Services**: Services MUST be stateless where required (especially for cloud deployment). State is managed explicitly.
3. **Separation of Concerns**: Each module, function, or service has a single, well-defined responsibility.
4. **Cloud-Native Readiness**: Architecture is designed for cloud deployment from the beginning (even in early phases).

**Rationale**: Quality principles ensure code maintains viability across phase evolution. Clean architecture supports incremental complexity without refactoring.

## Technology Constraints

### Core Technology Stack

#### Backend
- **Python**: All backend code MUST use Python
- **FastAPI**: Web framework for API layers (phases requiring web interfaces)
- **SQLModel**: ORM and data modeling (phases requiring database persistence)
- **Neon DB**: Managed PostgreSQL database (phases requiring persistence)

#### Frontend
- **Next.js**: Frontend framework for web interfaces (Phase III+)
- **TypeScript**: Type-safe JavaScript for frontend code

#### Agent & Integration
- **OpenAI Agents SDK**: Agent orchestration and tool usage
- **MCP (Model Context Protocol)**: Agent communication and tool integration

#### Infrastructure (Later Phases)
- **Docker**: Containerization (Phase III+)
- **Kubernetes**: Container orchestration (Phase IV+)
- **Kafka**: Event streaming and message queues (Phase IV+)
- **Dapr**: Distributed application runtime (Phase V)

### Prohibited Technologies
- **Unauthorized Persistence**: Any database or storage not in approved stack (e.g., MongoDB, Redis, filesystem persistence)
- **Unauthorized Web Frameworks**: Any web framework besides FastAPI (e.g., Flask, Django, Express.js)
- **Unauthorized Frontend**: Any frontend framework besides Next.js (e.g., React, Vue, Angular, Svelte)
- **Stateful Services**: Services that maintain implicit state (state MUST be explicit and managed)
- **Alternative Orchestration**: Kubernetes alternatives (e.g., Docker Swarm, Nomad) unless formally approved
- **Alternative Messaging**: Kafka alternatives (e.g., RabbitMQ, AWS SQS) unless formally approved

### Permitted Libraries
- **Python Standard Library**: Preferred for all simple tasks
- **Approved Third-Party Libraries**: Only those in the core technology stack above
- **Justified Additions**: Third-party libraries outside the stack MUST be explicitly justified in spec and approved

### Architectural Patterns
- **PERMITTED**:
  - Clean architecture (domain-driven, hexagonal, or layered)
  - Repository pattern (for database interactions)
  - Service layer pattern
  - CQRS (when appropriate for read/write separation)
- **PROHIBITED**:
  - Over-engineering patterns not justified by phase requirements
  - Design patterns that add unnecessary abstraction
  - Framework-specific lock-in patterns

## Phase Technology Matrix

| Technology | Phase I | Phase II | Phase III | Phase IV | Phase V |
|------------|---------|----------|-----------|----------|---------|
| Python | ✅ | ✅ | ✅ | ✅ | ✅ |
| CLI Interface | ✅ | ✅ | ✅ | ✅ | ✅ |
| In-Memory Storage | ✅ | ❌ | ❌ | ❌ | ❌ |
| FastAPI | ❌ | ✅ | ✅ | ✅ | ✅ |
| SQLModel | ❌ | ❌ | ✅ | ✅ | ✅ |
| Neon DB | ❌ | ❌ | ✅ | ✅ | ✅ |
| Next.js | ❌ | ❌ | ❌ | ✅ | ✅ |
| OpenAI Agents SDK | ❌ | ❌ | ✅ | ✅ | ✅ |
| MCP | ❌ | ❌ | ✅ | ✅ | ✅ |
| Docker | ❌ | ❌ | ❌ | ✅ | ✅ |
| Kubernetes | ❌ | ❌ | ❌ | ❌ | ✅ |
| Kafka | ❌ | ❌ | ❌ | ❌ | ✅ |
| Dapr | ❌ | ❌ | ❌ | ❌ | ✅ |

**Notes**:
- ✅ = Technology is in scope for this phase
- ❌ = Technology is out-of-scope for this phase
- Earlier phase technologies remain available in later phases (e.g., CLI remains in Phase V)

## Development Workflow

### Spec-Driven Flow (Mandatory)
1. **Constitution Check**: Verify requirements align with constitution before proceeding
2. **Spec Stage**: Define user stories, requirements, success criteria in `specs/<feature>/spec.md`
3. **Plan Stage**: Create architecture and design in `specs/<feature>/plan.md`
4. **Tasks Stage**: Generate implementation tasks in `specs/<feature>/tasks.md`
5. **Implement Stage**: Execute tasks via AI agents following approved tasks exactly

### Human Role
- Define requirements and approve specifications
- Review and approve plans, tasks, and implementations
- Request clarification or refinement at spec level
- Humans DO NOT write code directly—code written by humans MUST be rejected

### Quality Gates
- All specs MUST have testable acceptance criteria
- All plans MUST pass Constitution Check (spec-driven, agent behavior, phase governance, quality, technology)
- All implementations MUST match task specifications exactly
- Code violating principles MUST be rejected, even if it "works"
- Technology outside approved phase scope MUST be rejected

### Testing Discipline
- Tests are OPTIONAL unless explicitly requested in spec
- If tests are requested: Red-Green-Refactor MUST be followed strictly
- Test files MUST be in `tests/` directory at repository root
- Backend tests: `pytest` (standard Python testing tool)
- Frontend tests (Phase III+): Jest or Testing Library (to be defined in phase spec)

### Phase Transition
- Phase transition occurs ONLY after all planned features for current phase are complete
- Transition requires new spec and plan for next phase
- No code for next phase may be written before phase transition spec approval
- Technology stack additions for next phase must be explicitly justified

## Governance

### Amendment Procedure
- Constitution changes require explicit consensus via formal amendment spec
- Version follows semantic versioning (MAJOR.MINOR.PATCH)
  - MAJOR: Backward-incompatible changes to core principles, technology stack, or phase structure
  - MINOR: New sections, material expansion of guidance, non-breaking additions
  - PATCH: Wording fixes, clarifications, typo corrections
- All amendment proposals MUST be documented with rationale and impact analysis
- Changes affecting Core Principles (I-IV) require MAJOR version bump
- Changes to Technology Constraints require MAJOR version bump
- Changes to Phase Technology Matrix require MAJOR version bump

### Compliance Review
- All plan artifacts MUST include Constitution Check section
- Constitution violations in plans MUST be justified in Complexity Tracking
- Any unjustified violation results in rejection during review
- All PRs/reviews MUST verify alignment with principles and phase technology matrix
- Technology outside approved phase scope is a critical violation

### Supremacy
This constitution is the supreme governing document for the "Evolution of Todo" project. It supersedes all other practices, guidelines, or conventions. All agents, humans, and processes MUST comply with this constitution without exception.

### Scope Boundaries
This constitution governs Phase I through Phase V of the "Evolution of Todo" project. Each phase is scoped independently by its specification, but all phases remain bound by these core principles and governance rules. Phases beyond V require formal constitution amendment.

**Version**: 2.0.0 | **Ratified**: 2024-12-24 | **Last Amended**: 2024-12-24
