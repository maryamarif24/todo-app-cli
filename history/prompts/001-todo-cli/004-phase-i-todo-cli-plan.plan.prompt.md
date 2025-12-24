---
id: 4
title: Phase I Todo CLI plan
stage: plan
date: 2024-12-24
surface: agent
model: claude-sonnet-4-5-20250929
feature: 001-todo-cli
branch: 001-todo-cli
user: N/A
command: sp.plan
labels: ["phase-i", "todo", "cli", "plan", "architecture"]
links:
  spec: specs/001-todo-cli/spec.md
  ticket: null
  adr: null
  pr: null
files:
  - specs/001-todo-cli/plan.md
  - specs/001-todo-cli/research.md
  - specs/001-todo-cli/data-model.md
  - specs/001-todo-cli/contracts/cli-contract.md
  - specs/001-todo-cli/quickstart.md
  - CLAUDE.md (updated with agent context)
tests:
  - Constitution validation: All gates passed
  - Post-design validation: All gates passed
---

## Prompt

Create Phase I technical plan for Todo in-memory Python console application.

The plan must be derived strictly from Phase I specification and global constitution.

Include:
1. High-level application structure (single Python program)
2. In-memory data structures to store tasks
3. Task identification strategy (ID generation)
4. CLI control flow (menu loop, user input handling)
5. Separation of responsibilities (data handling vs CLI)
6. Error handling strategy for invalid input and missing tasks

Constraints:
- No databases
- No file storage
- No web frameworks
- No external services
- No future phase concepts

The plan must not introduce new features.
It must only describe HOW approved Phase I requirements will be implemented.

## Response snapshot

Created complete technical plan at `specs/001-todo-cli/plan.md` with:

**Architecture**:
- Single-file application: `src/todo_app.py`
- Clean separation: Data layer functions and CLI layer functions
- Menu loop with routing to action functions
- In-memory data structures (list for tasks, counter for IDs)

**Key Decisions** (from research.md):
- Python 3.13+ (constitution requirement)
- Standard library only (no external dependencies)
- List-based task storage (ordered, simple)
- Sequential ID generation (1, 2, 3..., never reused)
- dataclass for Task representation
- Manual input() with validation (menu-based, not argparse)

**Data Model** (data-model.md):
- Task entity: id (int), description (str, 1-500 chars), status (enum: completed/incomplete), created_at (datetime)
- TaskList entity: tasks (list[Task]), next_id (int counter)
- Validation rules, state transitions, relationship diagrams

**CLI Contract** (contracts/cli-contract.md):
- 7 menu options (Add, View, Update, Delete, Mark Complete, Mark Incomplete, Exit)
- All flows defined with message formats
- Error handling contract (validation errors, not found errors, empty list)
- Unicode support (emojis, special characters)
- Performance targets (1s launch, 5s add, 2s view)

**Constitution Check**:
- ✅ Core Principles: All passed (spec-driven, agent behavior, phase governance, quality)
- ✅ Phase I Technology Matrix: All passed (Python, CLI, in-memory only)
- ✅ No future phase concepts (databases, web frameworks, multi-user)
- ✅ No new features (strictly from spec.md)

**Supporting Files**:
- research.md: 10 technology decisions with rationales
- data-model.md: Complete entity definitions with validation
- cli-contract.md: Full interface specification
- quickstart.md: User-facing getting started guide

**Agent Context**: Updated CLAUDE.md with:
- Language: Python 3.13+
- Framework: Standard library only
- Database: In-memory data structures
- Project type: Single console application

## Outcome

- ✅ Impact: Complete technical architecture defined for Phase I Todo CLI application
- 🧪 Tests: Constitution validation passed (pre-design and post-design), no unit tests required (Phase I)
- 📁 Files: Created plan.md, research.md, data-model.md, contracts/cli-contract.md, quickstart.md; updated CLAUDE.md
- 🔁 Next prompts: `/sp.tasks` to generate implementation tasks
- 🧠 Reflection: Plan strictly derives from spec and constitution. Clean separation of data/CLI layers. No over-engineering, no new features, no future phase concepts. Ready for task generation.

## Evaluation notes (flywheel)

- Failure modes observed: None
- Graders run and results (PASS/FAIL): Constitution Check - PASS (pre-design), PASS (post-design)
- Prompt variant (if applicable): N/A
- Next experiment (smallest change to try): N/A
