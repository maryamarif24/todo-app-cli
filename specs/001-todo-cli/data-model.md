# Data Model: Phase I Todo CLI

**Branch**: `001-todo-cli` | **Date**: 2024-12-24
**Spec**: [spec.md](spec.md) | **Constitution**: [constitution.md](../../.specify/memory/constitution.md)

## Overview

This document defines the data model for Phase I Todo CLI application. The model is intentionally simple, using in-memory data structures with clear types and validation rules.

## Entities

### Task

Represents a single todo item in the system.

#### Attributes

| Attribute | Type | Description | Constraints | Example |
|-----------|------|-------------|-------------|---------|
| id | int | Unique numeric identifier | 1, 2, 3... |
| description | str | Text content of the task | 1-500 characters |
| status | enum | Completion state | "completed" or "incomplete" |
| created_at | datetime | Timestamp when task was created | 2024-12-24T10:30:00 |

#### Validation Rules

| Attribute | Validation | Error Handling |
|-----------|------------|----------------|
| id | Must be unique | Handled by ID generation (sequential counter) |
| description | Cannot be empty (zero length or whitespace only) | Reject input, show error message |
| description | Maximum 500 characters | Truncate to 500, show warning message |
| status | Must be "completed" or "incomplete" | Internal constraint - never set by user |
| created_at | Set at creation time, never modified | Internal constraint |

#### State Transitions

```
                   ┌────────────────────┐
                   │   Initial State    │
                   │  status =          │
                   │  "incomplete"      │
                   └──────────┬─────────┘
                              │
                    ┌─────────┴─────────┐
                    │                   │
                    ▼                   ▼
            ┌───────────────┐    ┌───────────────┐
            │ mark_complete │    │   No Change    │
            │  (user action) │    │ (no action)   │
            └───────┬───────┘    └───────────────┘
                    │
                    ▼
            ┌───────────────┐
            │  status =        │
            │  "completed"      │
            └───────────┬─────┘
                        │
                 ┌─────────┴─────────┐
                 │                   │
                 ▼                   ▼
         ┌───────────────┐    ┌───────────────┐
         │mark_incomplete │    │   No Change    │
         │  (user action) │    │ (no action)   │
         └───────┬───────┘    └───────────────┘
                 │
                 ▼
         ┌───────────────┐
         │  status =        │
         │  "incomplete"      │
         └──────────────────┘
```

#### Python Representation

```python
from dataclasses import dataclass
from datetime import datetime
from enum import Enum

class TaskStatus(str, Enum):
    COMPLETED = "completed"
    INCOMPLETE = "incomplete"

@dataclass
class Task:
    """Represents a single todo item."""
    id: int
    description: str
    status: TaskStatus
    created_at: datetime

    def __post_init__(self):
        """Validate and initialize derived values."""
        # Truncate description to 500 characters if needed
        if len(self.description) > 500:
            self.description = self.description[:500]
```

### TaskList

Represents the collection of all tasks in the application.

#### Attributes

| Attribute | Type | Description | Constraints | Initial Value |
|-----------|------|-------------|-------------|---------------|
| tasks | list[Task] | Ordered list of all tasks | [] (empty) |
| next_id | int | Counter for generating unique IDs | 1 |

#### Invariants

| Invariant | Description | Enforcement |
|-----------|-------------|--------------|
| IDs are unique | No two tasks share the same ID | next_id increments on each add, never reused |
| Tasks are ordered | List maintains creation order | Tasks appended to end of list |
| next_id > max(task IDs) | next_id is always greater than existing IDs | next_id incremented after each task creation |

#### Python Representation

```python
@dataclass
class TaskList:
    """In-memory collection of tasks."""
    tasks: list[Task] = None  # Initialized to [] in __post_init__
    next_id: int = 1

    def __post_init__(self):
        if self.tasks is None:
            self.tasks = []

    def add_task(self, description: str) -> Task:
        """Create and add a new task."""
        task = Task(
            id=self.next_id,
            description=description,
            status=TaskStatus.INCOMPLETE,
            created_at=datetime.now()
        )
        self.tasks.append(task)
        self.next_id += 1
        return task

    def get_task(self, task_id: int) -> Task | None:
        """Find task by ID, return None if not found."""
        for task in self.tasks:
            if task.id == task_id:
                return task
        return None

    def update_task(self, task_id: int, description: str) -> bool:
        """Update task description, return True if successful."""
        task = self.get_task(task_id)
        if task is None:
            return False
        task.description = description
        return True

    def delete_task(self, task_id: int) -> bool:
        """Delete task by ID, return True if successful."""
        task = self.get_task(task_id)
        if task is None:
            return False
        self.tasks.remove(task)
        return True

    def mark_complete(self, task_id: int) -> bool:
        """Mark task as complete, return True if successful."""
        task = self.get_task(task_id)
        if task is None:
            return False
        task.status = TaskStatus.COMPLETED
        return True

    def mark_incomplete(self, task_id: int) -> bool:
        """Mark task as incomplete, return True if successful."""
        task = self.get_task(task_id)
        if task is None:
            return False
        task.status = TaskStatus.INCOMPLETE
        return True

    def list_all(self) -> list[Task]:
        """Return all tasks in creation order."""
        return self.tasks.copy()

    def is_empty(self) -> bool:
        """Check if task list is empty."""
        return len(self.tasks) == 0
```

## Relationships

```
TaskList
    │
    │ 1..* (contains)
    ▼
   Task
```

- **TaskList** contains zero or more **Task** objects
- Tasks maintain no reference to TaskList (unidirectional)
- Task order in TaskList represents creation order

## Data Flow Examples

### Adding a Task

```
Initial State:
  TaskList.tasks = []
  TaskList.next_id = 1

User Input: "Buy groceries"

Flow:
  1. Validate description: not empty ✅
  2. Validate description: length ≤ 500 ✅
  3. Create Task:
       - id = 1
       - description = "Buy groceries"
       - status = "incomplete"
       - created_at = 2024-12-24T10:30:00
  4. Append to TaskList.tasks
  5. Increment next_id: 1 → 2

Final State:
  TaskList.tasks = [Task(id=1, description="Buy groceries", ...)]
  TaskList.next_id = 2
```

### Updating a Task

```
Initial State:
  TaskList.tasks = [
      Task(id=1, description="Buy groceries", ...),
      Task(id=2, description="Walk the dog", ...)
  ]
  TaskList.next_id = 3

User Input: task_id=1, new_description="Buy groceries and milk"

Flow:
  1. Find task by ID: Task(id=1) found ✅
  2. Update description
  3. Return success = True

Final State:
  TaskList.tasks = [
      Task(id=1, description="Buy groceries and milk", ...),
      Task(id=2, description="Walk the dog", ...)
  ]
```

### Deleting a Task

```
Initial State:
  TaskList.tasks = [
      Task(id=1, description="Buy groceries", ...),
      Task(id=2, description="Walk the dog", ...),
      Task(id=3, description="Do laundry", ...)
  ]
  TaskList.next_id = 4

User Input: task_id=2

Flow:
  1. Find task by ID: Task(id=2) found ✅
  2. Remove from TaskList.tasks
  3. Return success = True

Final State:
  TaskList.tasks = [
      Task(id=1, description="Buy groceries", ...),
      Task(id=3, description="Do laundry", ...)
  ]
  TaskList.next_id = 4  (NOT decremented - next task is 4)
```

## Storage and Persistence

### In-Memory Only

- **No File I/O**: Tasks are never written to disk
- **No Database**: No SQL, NoSQL, or ORM usage
- **No Serialization**: No pickle, JSON, YAML files
- **Volatility**: All data lost when application terminates

### Memory Footprint Estimate

```
Per Task (Python dataclass):
  - id (int): ~28 bytes
  - description (str, avg 50 chars): ~67 bytes
  - status (enum): ~28 bytes
  - created_at (datetime): ~48 bytes
  - Overhead: ~56 bytes
  Total: ~227 bytes per task

Phase I Scale (~100 tasks):
  - Tasks: ~22.7 KB
  - TaskList overhead: ~1 KB
  Total: ~24 KB (negligible)
```

## Validation Summary

| Entity | Attribute | Validation | Action |
|--------|-----------|------------|--------|
| Task | id | Unique integer | Handled by TaskList.next_id |
| Task | description | Not empty | Reject input, show error |
| Task | description | Length ≤ 500 | Truncate, show warning |
| Task | status | Valid enum value | Internal constraint |
| Task | created_at | Not modified after creation | Internal constraint |
| TaskList | next_id | Always incrementing | Handled by add_task() |
| TaskList | tasks | Ordered by creation | Handled by list append |

## Compliance Verification

| Requirement | Data Model Compliance |
|-------------|----------------------|
| FR-002: Unique sequential IDs | ✅ TaskList.next_id provides sequential IDs |
| FR-003: In-memory storage | ✅ List-based, no persistence |
| FR-004: 500 char limit | ✅ Truncation in __post_init__ |
| FR-005: No empty descriptions | ✅ Validation in add_task() |
| FR-006: Display ID, description, status | ✅ All attributes present on Task |
| FR-013: Unicode support | ✅ Python str is Unicode by default |

✅ All functional requirements supported by data model.
