---

description: "Implementation tasks for Phase I Todo CLI application"
---

# Tasks: Phase I Todo CLI

**Input**: Design documents from `/specs/001-todo-cli/`
**Prerequisites**: plan.md (required), spec.md (required), data-model.md, contracts/cli-contract.md

**Tests**: NOT REQUIRED for Phase I - Tests are optional per spec.md, no test tasks included.

**Organization**: Tasks are grouped by user story to enable independent implementation and testing of each story.

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (e.g., US1, US2, US3)
- Include exact file paths in descriptions

## Path Conventions

- **Single project**: `src/todo_app.py` - All code in single file

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Project initialization and basic structure

- [X] T001 Create src/ directory and empty todo_app.py file in src/todo_app.py
  - **Preconditions**: None
  - **Expected Output**: src/ directory exists, src/todo_app.py is empty Python file
  - **References**: spec.md (FR-001: menu-based CLI), plan.md (single-file architecture)
  - **Artifacts**: src/ directory, src/todo_app.py

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Core data model and storage that MUST be complete before ANY user story can be implemented

**⚠️ CRITICAL**: No user story work can begin until this phase is complete

- [X] T002 Implement TaskStatus enum (completed, incomplete) in src/todo_app.py
  - **Preconditions**: T001 completed
  - **Expected Output**: TaskStatus enum with COMPLETED = "completed" and INCOMPLETE = "incomplete"
  - **References**: spec.md Key Entities, data-model.md Task entity
  - **Artifacts**: Modified src/todo_app.py

- [X] T003 Implement Task dataclass in src/todo_app.py
  - **Preconditions**: T002 completed
  - **Expected Output**: Task dataclass with id (int), description (str), status (TaskStatus), created_at (datetime)
  - **References**: spec.md Key Entities, data-model.md Task entity, plan.md (dataclass for conciseness)
  - **Artifacts**: Modified src/todo_app.py

- [X] T004 Implement TaskList class with tasks list and next_id counter in src/todo_app.py
  - **Preconditions**: T003 completed
  - **Expected Output**: TaskList dataclass with tasks: list[Task] (default empty), next_id: int (default 1)
  - **References**: spec.md Key Entities, data-model.md TaskList entity
  - **Artifacts**: Modified src/todo_app.py

- [X] T005 Implement TaskList.add_task(description) method in src/todo_app.py
  - **Preconditions**: T004 completed
  - **Expected Output**: Creates Task with next_id, INCOMPLETE status, datetime.now(); appends to tasks list; increments next_id; returns Task
  - **References**: spec.md FR-002 (sequential IDs), data-model.md TaskList.add_task()
  - **Artifacts**: Modified src/todo_app.py

- [X] T006 Implement TaskList.get_task(task_id) method in src/todo_app.py
  - **Preconditions**: T004 completed
  - **Expected Output**: Searches tasks list by id; returns Task if found or None if not found
  - **References**: data-model.md TaskList.get_task()
  - **Artifacts**: Modified src/todo_app.py

- [X] T007 Implement TaskList.list_all() and is_empty() methods in src/todo_app.py
  - **Preconditions**: T004 completed
  - **Expected Output**: list_all() returns copy of tasks list; is_empty() returns True if len(tasks) == 0
  - **References**: data-model.md TaskList.list_all(), data-model.md TaskList.is_empty()
  - **Artifacts**: Modified src/todo_app.py

**Checkpoint**: Foundation ready - data model complete, user story implementation can begin

---

## Phase 3: User Story 1 - Add Tasks (Priority: P1) 🎯 MVP

**Goal**: Users can add tasks to the todo list with unique IDs and default "incomplete" status

**Independent Test**: Launch application, select "Add Task", enter description, confirm task added with unique ID (verify by viewing list)

### Implementation for User Story 1

- [X] T008 [US1] Implement display_menu() function in src/todo_app.py
  - **Preconditions**: T001 completed
  - **Expected Output**: Function prints menu header and 7 options to stdout per spec.md Main Menu Structure
  - **References**: spec.md FR-001 (menu-based CLI), spec.md CLI Interaction Flow Main Menu Structure, contracts/cli-contract.md Menu Display
  - **Artifacts**: Modified src/todo_app.py

- [X] T009 [US1] Implement prompt_for_input(prompt_text) function in src/todo_app.py
  - **Preconditions**: T001 completed
  - **Expected Output**: Function uses input(prompt_text) to get user input, returns string
  - **References**: spec.md CLI Interaction Flow (User Selection step)
  - **Artifacts**: Modified src/todo_app.py

- [X] T010 [US1] Implement display_error(message) function to stderr in src/todo_app.py
  - **Preconditions**: T001 completed
  - **Expected Output**: Function prints "Error: {message}\n" to stderr using sys.stderr.write()
  - **References**: spec.md FR-008 (descriptive errors), contracts/cli-contract.md Error Message Format
  - **Artifacts**: Modified src/todo_app.py

- [X] T011 [US1] Implement display_success(message) function to stdout in src/todo_app.py
  - **Preconditions**: T001 completed
  - **Expected Output**: Function prints "{message}\n" to stdout
  - **References**: spec.md FR-009 (return to menu after operation), contracts/cli-contract.md Output Format Summary
  - **Artifacts**: Modified src/todo_app.py

- [X] T012 [US1] Implement handle_add_task() function in src/todo_app.py
  - **Preconditions**: T005, T007, T009, T010, T011 completed
  - **Expected Output**: Prompts for description; validates non-empty (error if empty); truncates to 500 chars (warning if truncated); calls TaskList.add_task(); displays success with ID
  - **References**: spec.md User Story 1, spec.md Add Task Flow, spec.md FR-005 (no empty descriptions), spec.md FR-004 (500 char limit), contracts/cli-contract.md Flow 1
  - **Artifacts**: Modified src/todo_app.py

- [X] T013 [US1] Implement basic run_menu_loop() structure in src/todo_app.py
  - **Preconditions**: T008, T012 completed
  - **Expected Output**: while True loop; display menu; get choice; route to handle_add_task for choice 1; break on exit (choice 7)
  - **References**: spec.md FR-001 (menu options), spec.md CLI Interaction Flow, plan.md CLI Control Flow
  - **Artifacts**: Modified src/todo_app.py

**Checkpoint**: At this point, User Story 1 is fully functional - users can add tasks and see menu

---

## Phase 4: User Story 2 - View Task List (Priority: P1) 🎯 MVP

**Goal**: Users can view all tasks with ID, description, and completion status

**Independent Test**: Add multiple tasks, select "View Tasks", confirm all tasks displayed correctly with [ ] for incomplete and [X] for completed

### Implementation for User Story 2

- [X] T014 [US2] Implement display_tasks(tasks_list) function in src/todo_app.py
  - **Preconditions**: T001 completed
  - **Expected Output**: If empty, prints "No tasks found.\n"; else prints "=== Your Tasks ===\n" then each task as "{id}. [{status}] {description}\n" where status is "X" for completed, " " for incomplete
  - **References**: spec.md User Story 2, spec.md FR-006 (display ID/description/status), contracts/cli-contract.md Flow 2
  - **Artifacts**: Modified src/todo_app.py

- [X] T015 [US2] Implement handle_view_tasks() function in src/todo_app.py
  - **Preconditions**: T007, T014 completed
  - **Expected Output**: Calls TaskList.list_all(); calls display_tasks(); returns True
  - **References**: spec.md User Story 2, spec.md View Tasks Flow
  - **Artifacts**: Modified src/todo_app.py

- [X] T016 [US2] Integrate handle_view_tasks() into menu loop (choice 2) in src/todo_app.py
  - **Preconditions**: T013, T015 completed
  - **Expected Output**: Menu loop routes choice 2 to handle_view_tasks()
  - **References**: spec.md FR-001 (menu option 2)
  - **Artifacts**: Modified src/todo_app.py

**Checkpoint**: At this point, User Stories 1 AND 2 are fully functional - users can add and view tasks

---

## Phase 5: User Story 3 - Update Task (Priority: P2)

**Goal**: Users can update task descriptions with validation

**Independent Test**: Add task, select "Update Task", enter ID and new description, confirm description changed

### Implementation for User Story 3

- [X] T017 [US3] Implement TaskList.update_task(task_id, description) method in src/todo_app.py
  - **Preconditions**: T006 completed
  - **Expected Output**: Calls get_task(); returns False if not found; validates description not empty; updates task.description; returns True
  - **References**: spec.md User Story 3, data-model.md TaskList.update_task()
  - **Artifacts**: Modified src/todo_app.py

- [X] T018 [US3] Implement handle_update_task() function in src/todo_app.py
  - **Preconditions**: T007, T009, T010, T017 completed
  - **Expected Output**: Checks if empty (error if empty); prompts for ID; prompts for new description; validates non-empty (error if empty); calls TaskList.update_task(); displays success or error
  - **References**: spec.md User Story 3, spec.md Update Task Flow, spec.md Edge Cases (empty list check)
  - **Artifacts**: Modified src/todo_app.py

- [X] T019 [US3] Integrate handle_update_task() into menu loop (choice 3) in src/todo_app.py
  - **Preconditions**: T013, T018 completed
  - **Expected Output**: Menu loop routes choice 3 to handle_update_task()
  - **References**: spec.md FR-001 (menu option 3)
  - **Artifacts**: Modified src/todo_app.py

**Checkpoint**: At this point, User Stories 1, 2, AND 3 are fully functional

---

## Phase 6: User Story 4 - Delete Task (Priority: P2)

**Goal**: Users can delete tasks from the list

**Independent Test**: Add task, select "Delete Task", enter ID, confirm task removed (verify by viewing list)

### Implementation for User Story 4

- [X] T020 [US4] Implement TaskList.delete_task(task_id) method in src/todo_app.py
  - **Preconditions**: T006 completed
  - **Expected Output**: Calls get_task(); returns False if not found; removes task from tasks list; returns True
  - **References**: spec.md User Story 4, data-model.md TaskList.delete_task()
  - **Artifacts**: Modified src/todo_app.py

- [X] T021 [US4] Implement handle_delete_task() function in src/todo_app.py
  - **Preconditions**: T007, T009, T010, T020 completed
  - **Expected Output**: Checks if empty (error if empty); prompts for ID; calls TaskList.delete_task(); displays success or error
  - **References**: spec.md User Story 4, spec.md Delete Task Flow, spec.md Edge Cases (empty list check)
  - **Artifacts**: Modified src/todo_app.py

- [X] T022 [US4] Integrate handle_delete_task() into menu loop (choice 4) in src/todo_app.py
  - **Preconditions**: T013, T021 completed
  - **Expected Output**: Menu loop routes choice 4 to handle_delete_task()
  - **References**: spec.md FR-001 (menu option 4)
  - **Artifacts**: Modified src/todo_app.py

**Checkpoint**: At this point, User Stories 1-4 are fully functional

---

## Phase 7: User Story 5 - Mark Task Complete/Incomplete (Priority: P2)

**Goal**: Users can toggle task completion status

**Independent Test**: Add task, mark complete, view to confirm [X]; mark incomplete, view to confirm [ ]

### Implementation for User Story 5

- [X] T023 [US5] Implement TaskList.mark_complete(task_id) method in src/todo_app.py
  - **Preconditions**: T006 completed
  - **Expected Output**: Calls get_task(); returns False if not found; sets task.status to TaskStatus.COMPLETED; returns True
  - **References**: spec.md User Story 5, data-model.md TaskList.mark_complete()
  - **Artifacts**: Modified src/todo_app.py

- [X] T024 [US5] Implement TaskList.mark_incomplete(task_id) method in src/todo_app.py
  - **Preconditions**: T006 completed
  - **Expected Output**: Calls get_task(); returns False if not found; sets task.status to TaskStatus.INCOMPLETE; returns True
  - **References**: spec.md User Story 5, data-model.md TaskList.mark_incomplete()
  - **Artifacts**: Modified src/todo_app.py

- [X] T025 [US5] Implement handle_mark_complete() function in src/todo_app.py
  - **Preconditions**: T007, T009, T010, T023 completed
  - **Expected Output**: Checks if empty (error if empty); prompts for ID; calls TaskList.mark_complete(); displays success or error
  - **References**: spec.md User Story 5, spec.md Mark Complete Flow, spec.md Edge Cases (empty list check)
  - **Artifacts**: Modified src/todo_app.py

- [X] T026 [US5] Implement handle_mark_incomplete() function in src/todo_app.py
  - **Preconditions**: T007, T009, T010, T024 completed
  - **Expected Output**: Checks if empty (error if empty); prompts for ID; calls TaskList.mark_incomplete(); displays success or error
  - **References**: spec.md User Story 5, spec.md Mark Incomplete Flow, spec.md Edge Cases (empty list check)
  - **Artifacts**: Modified src/todo_app.py

- [X] T027 [US5] Integrate handle_mark_complete/incomplete into menu loop (choices 5, 6) in src/todo_app.py
  - **Preconditions**: T013, T025, T026 completed
  - **Expected Output**: Menu loop routes choice 5 to handle_mark_complete(), choice 6 to handle_mark_incomplete()
  - **References**: spec.md FR-001 (menu options 5, 6)
  - **Artifacts**: Modified src/todo_app.py

**Checkpoint**: All user stories (1-5) are now fully functional

---

## Phase 8: Input Validation and Error Handling

**Purpose**: Complete validation for all inputs and error scenarios

- [X] T028 Implement numeric validation for menu choice input in src/todo_app.py
  - **Preconditions**: T013 completed
  - **Expected Output**: Validate choice is numeric; if not, call display_error("Please enter a valid numeric task ID.") and return to menu
  - **References**: spec.md FR-007 (numeric validation), spec.md Edge Cases (non-numeric input), contracts/cli-contract.md Input Validation Errors
  - **Artifacts**: Modified src/todo_app.py

- [X] T029 Implement range validation for menu choice (1-7) in src/todo_app.py
  - **Preconditions**: T028 completed
  - **Expected Output**: Validate choice is between 1-7; if not, call display_error("Invalid choice. Please enter 1-7.") and return to menu
  - **References**: spec.md FR-001 (7 options), spec.md Edge Cases (unrecognized menu option), contracts/cli-contract.md Input Validation Errors
  - **Artifacts**: Modified src/todo_app.py

- [X] T030 Implement numeric validation for task ID input in src/todo_app.py
  - **Preconditions**: T018, T021, T025, T026 completed
  - **Expected Output**: Validate ID input is numeric before get_task(); if not, call display_error("Please enter a valid numeric task ID.")
  - **References**: spec.md FR-007 (numeric validation), spec.md Edge Cases (non-numeric input), contracts/cli-contract.md Input Validation Errors
  - **Artifacts**: Modified src/todo_app.py

- [X] T031 Implement truncation warning for >500 char descriptions in src/todo_app.py
  - **Preconditions**: T012 completed
  - **Expected Output**: If description > 500 chars, truncate to 500 and call display_error("Warning: Description truncated to 500 characters.")
  - **References**: spec.md FR-004 (500 char limit), spec.md Edge Cases (long descriptions), contracts/cli-contract.md Flow 1 (truncation warning)
  - **Artifacts**: Modified src/todo_app.py

**Checkpoint**: Input validation and error handling complete

---

## Phase 9: Application Startup and Exit Flow

**Purpose**: Complete application lifecycle management

- [X] T032 Implement main entry point (if __name__ == "__main__") in src/todo_app.py
  - **Preconditions**: All previous tasks completed
  - **Expected Output**: Creates TaskList instance, calls run_menu_loop() on application start
  - **References**: plan.md High-Level Application Structure, spec.md FR-006 (launch within 1 second), spec.md SC-006
  - **Artifacts**: Modified src/todo_app.py

- [X] T033 Implement exit flow (choice 7) in src/todo_app.py
  - **Preconditions**: T013 completed
  - **Expected Output**: On choice 7, call display_success("Goodbye!") and break menu loop; application exits cleanly
  - **References**: spec.md FR-010 (exit cleanly), spec.md Exit Flow, spec.md SC-007 (exit within 1 second), contracts/cli-contract.md Flow 7
  - **Artifacts**: Modified src/todo_app.py

**Checkpoint**: Complete application lifecycle implemented

---

## Phase 10: Polish & Cross-Cutting Concerns

**Purpose**: Final integration and validation

- [X] T034 Verify complete menu loop with all choices (1-7) in src/todo_app.py
  - **Preconditions**: All previous tasks completed
  - **Expected Output**: Menu loop correctly routes all 7 choices to respective handlers; returns to menu after each operation (except exit)
  - **References**: spec.md FR-009 (return to menu), spec.md CLI Interaction Flow
  - **Artifacts**: Verified src/todo_app.py

- [X] T035 Verify Unicode and special character support in src/todo_app.py
  - **Preconditions**: All previous tasks completed
  - **Expected Output**: Application accepts and displays Unicode characters and emojis in task descriptions
  - **References**: spec.md FR-013 (Unicode support), spec.md Edge Cases (special characters), quickstart.md Unicode Support
  - **Artifacts**: Verified src/todo_app.py

- [X] T036 Run quickstart.md validation in src/todo_app.py
  - **Preconditions**: All previous tasks completed
  - **Expected Output**: Application matches all quickstart.md usage examples and error handling examples
  - **References**: quickstart.md (all examples)
  - **Artifacts**: Verified src/todo_app.py

**Checkpoint**: Phase I complete and validated

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: No dependencies - can start immediately
- **Foundational (Phase 2)**: Depends on Setup (T001) - BLOCKS all user stories
- **User Stories (Phases 3-7)**: All depend on Foundational phase completion
  - User stories proceed sequentially (US1 → US2 → US3 → US4 → US5)
  - Each story can be tested independently when complete
- **Input Validation (Phase 8)**: Depends on US1-US5 completion
- **Startup/Exit (Phase 9)**: Depends on all previous phases
- **Polish (Phase 10)**: Final validation of complete implementation

### User Story Dependencies

- **User Story 1 (Add Tasks - P1)**: Depends on Foundational (Phase 2) only - No dependencies on other user stories
- **User Story 2 (View Tasks - P1)**: Depends on Foundational (Phase 2) - Uses TaskList from US1 foundation, but can be tested independently with tasks added manually
- **User Story 3 (Update Task - P2)**: Depends on Foundational (Phase 2) and US1 (to have tasks to update)
- **User Story 4 (Delete Task - P2)**: Depends on Foundational (Phase 2) and US1 (to have tasks to delete)
- **User Story 5 (Mark Complete/Incomplete - P2)**: Depends on Foundational (Phase 2) and US1 (to have tasks to mark)

### Within Each User Story

- Foundational functions (data model) before handler functions
- Handler functions before menu integration
- Menu integration before testing/validation

### Parallel Opportunities

**Limited parallelism due to single-file architecture**: Since all code goes into src/todo_app.py, true parallel file editing is not possible. However, certain tasks can be conceptually parallel in development:

- **Phase 2 (Foundational)**: T002, T003, T004 are independent (different classes/enums)
- **Phase 3 (US1)**: T008, T009, T010, T011 are independent helper functions
- **Phase 8 (Validation)**: T028, T029, T030 are independent validation functions
- **Phase 10 (Polish)**: T034, T035, T036 can be validated independently

### Sequential Implementation Strategy (Recommended)

Given single-file architecture, sequential implementation is recommended:

1. **Complete Phase 1** (T001): Create file
2. **Complete Phase 2** (T002-T007): Data model foundation
3. **Complete Phase 3** (T008-T013): Add tasks + menu basics
4. **Complete Phase 4** (T014-T016): View tasks
5. **Complete Phase 5** (T017-T019): Update tasks
6. **Complete Phase 6** (T020-T022): Delete tasks
7. **Complete Phase 7** (T023-T027): Mark complete/incomplete
8. **Complete Phase 8** (T028-T031): Input validation
9. **Complete Phase 9** (T032-T033): Startup and exit
10. **Complete Phase 10** (T034-T036): Final validation

---

## Implementation Strategy

### MVP First (User Stories 1-2 Only)

1. Complete Phase 1: Setup (T001)
2. Complete Phase 2: Foundational (T002-T007) - CRITICAL
3. Complete Phase 3: User Story 1 (T008-T013)
4. **STOP and VALIDATE**: Users can add tasks and see menu
5. Add Phase 4: User Story 2 (T014-T016)
6. **STOP and VALIDATE**: Users can add and view tasks (MVP complete)

### Full Phase I Delivery

1. Complete Setup + Foundational (T001-T007) → Foundation ready
2. Add User Story 1 (T008-T013) → Add tasks capability
3. Add User Story 2 (T014-T016) → View tasks capability (MVP!)
4. Add User Story 3 (T017-T019) → Update tasks capability
5. Add User Story 4 (T020-T022) → Delete tasks capability
6. Add User Story 5 (T023-T027) → Mark complete/incomplete capability
7. Add Input Validation (T028-T031) → Robust error handling
8. Add Startup/Exit (T032-T033) → Complete application lifecycle
9. Final Validation (T034-T036) → Phase I complete

Each story adds value without breaking previous stories.

---

## Parallel Example (Conceptual)

```bash
# While limited due to single-file architecture, these groups can be developed in parallel
# and then merged sequentially:

# Group 1 - Foundational classes (Phase 2):
Task: "Implement TaskStatus enum in src/todo_app.py"
Task: "Implement Task dataclass in src/todo_app.py"
Task: "Implement TaskList class in src/todo_app.py"

# Group 2 - US1 helper functions (Phase 3):
Task: "Implement display_menu() function in src/todo_app.py"
Task: "Implement prompt_for_input() function in src/todo_app.py"
Task: "Implement display_error() function in src/todo_app.py"
Task: "Implement display_success() function in src/todo_app.py"

# Group 3 - Validation functions (Phase 8):
Task: "Implement numeric validation for menu choice in src/todo_app.py"
Task: "Implement range validation for menu choice in src/todo_app.py"
Task: "Implement numeric validation for task ID in src/todo_app.py"
```

---

## Notes

- **Single-file architecture**: All code in src/todo_app.py limits true parallelism
- **[Story] labels**: Map tasks to user stories for traceability
- **Checkpoint validation**: Each phase has checkpoints - stop and test before proceeding
- **No tests included**: Tests are optional per spec.md, no test tasks generated
- **Constitution compliance**: All tasks derived from spec.md and plan.md, no new features
- **No future phase concepts**: Only Phase I technologies (Python, CLI, in-memory)

---

## Task Summary

| Phase | Task Count | Description |
|-------|------------|-------------|
| Phase 1: Setup | 1 | Create project structure |
| Phase 2: Foundational | 6 | Data model and storage |
| Phase 3: US1 - Add Tasks | 6 | Add task functionality |
| Phase 4: US2 - View Tasks | 3 | View task list functionality |
| Phase 5: US3 - Update Task | 3 | Update task functionality |
| Phase 6: US4 - Delete Task | 3 | Delete task functionality |
| Phase 7: US5 - Mark Complete/Incomplete | 5 | Toggle completion status |
| Phase 8: Validation | 4 | Input validation and errors |
| Phase 9: Startup/Exit | 2 | Application lifecycle |
| Phase 10: Polish | 3 | Final validation |
| **TOTAL** | **36** | **Complete Phase I implementation** |

---

## User Story Task Breakdown

| User Story | Phase | Task IDs | Task Count |
|------------|-------|----------|------------|
| US1 - Add Tasks (P1) | Phase 3 | T008-T013 | 6 |
| US2 - View Tasks (P1) | Phase 4 | T014-T016 | 3 |
| US3 - Update Task (P2) | Phase 5 | T017-T019 | 3 |
| US4 - Delete Task (P2) | Phase 6 | T020-T022 | 3 |
| US5 - Mark Complete/Incomplete (P2) | Phase 7 | T023-T027 | 5 |

---

## Format Validation

✅ **All tasks follow the required format**: `- [ ] [TaskID] [P?] [Story?] Description with file path`

✅ **No test tasks included** - Tests are optional per spec.md

✅ **All tasks reference spec.md and plan.md sections**

✅ **All tasks specify artifacts to create or modify**

✅ **All tasks specify preconditions and expected outputs**
