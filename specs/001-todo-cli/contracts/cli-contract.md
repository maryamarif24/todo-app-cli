# CLI Interface Contract: Phase I Todo CLI

**Branch**: `001-todo-cli` | **Date**: 2024-12-24
**Spec**: [spec.md](../spec.md) | **Data Model**: [data-model.md](../data-model.md)

## Overview

This document defines the CLI interface contract for the Phase I Todo CLI application. The contract specifies the exact input/output behavior, message formats, and error handling for all CLI interactions.

## Interface Summary

| Aspect | Specification |
|--------|---------------|
| **Interface Type** | Menu-based, interactive CLI |
| **Input Source** | stdin (keyboard) via input() |
| **Output Destination** | stdout (normal), stderr (errors) |
| **Menu Options** | 7 numbered options |
| **Exit Condition** | User selects option 7 |
| **Encoding** | UTF-8 (Unicode support) |

## Menu Display

### Main Menu Format

```
=== Todo Application ===
1. Add Task
2. View Tasks
3. Update Task
4. Delete Task
5. Mark Complete
6. Mark Incomplete
7. Exit

Enter your choice (1-7): _
```

**Requirements**:
- Menu displayed after application start and after each operation
- Cursor positioned after "Enter your choice (1-7): " prompt
- Newline after menu display before prompt
- Blank line separator before menu (except first display)

## User Flows

### Flow 1: Add Task

**Option**: 1. Add Task

**Sequence**:

```
[User selects 1]

Enter task description: [user types "Buy groceries"]

Task added successfully! (ID: 1)

[Menu displayed again]
```

**Messages**:

| Context | Output | Destination |
|---------|--------|-------------|
| Prompt | "Enter task description: " | stdout |
| Success | "Task added successfully! (ID: {id})\n" | stdout |
| Error (empty) | "Error: Task description cannot be empty.\n" | stderr |
| Warning (truncated) | "Warning: Description truncated to 500 characters.\n" | stderr |

**Validation Rules**:
1. Input cannot be empty or whitespace only
2. Input > 500 chars truncated to 500
3. Success message includes assigned task ID

---

### Flow 2: View Tasks

**Option**: 2. View Tasks

**Sequence**:

```
[User selects 2]

=== Your Tasks ===
1. [ ] Buy groceries
2. [X] Walk the dog
3. [ ] Do laundry

[Menu displayed again]
```

**Messages**:

| Context | Output | Destination |
|---------|--------|-------------|
| Header | "=== Your Tasks ===\n" | stdout |
| Task line | "{id}. [{status}] {description}\n" | stdout |
| Empty list | "No tasks found.\n" | stdout |

**Task Display Format**:
- `{id}`: Task ID (right-aligned, padded)
- `{status}`: "X" for completed, " " (space) for incomplete
- `{description}`: Task description text

**Examples**:

```
1. [ ] Buy groceries
2. [X] Walk the dog
3. [ ] Do laundry
```

---

### Flow 3: Update Task

**Option**: 3. Update Task

**Sequence**:

```
[User selects 3]

Enter task ID to update: [user types "1"]
Enter new description: [user types "Buy groceries and milk"]

Task updated successfully!

[Menu displayed again]
```

**Messages**:

| Context | Output | Destination |
|---------|--------|-------------|
| Prompt ID | "Enter task ID to update: " | stdout |
| Prompt Desc | "Enter new description: " | stdout |
| Success | "Task updated successfully!\n" | stdout |
| Error (not found) | "Error: Task not found with ID: {id}\n" | stderr |
| Error (empty) | "Error: Task description cannot be empty.\n" | stderr |
| Error (no tasks) | "Error: No tasks found in the list.\n" | stderr |
| Warning (truncated) | "Warning: Description truncated to 500 characters.\n" | stderr |

**Validation Rules**:
1. Check if task list is empty first
2. Validate ID is numeric
3. Validate task exists
4. Validate description is not empty

---

### Flow 4: Delete Task

**Option**: 4. Delete Task

**Sequence**:

```
[User selects 4]

Enter task ID to delete: [user types "2"]

Task deleted successfully!

[Menu displayed again]
```

**Messages**:

| Context | Output | Destination |
|---------|--------|-------------|
| Prompt | "Enter task ID to delete: " | stdout |
| Success | "Task deleted successfully!\n" | stdout |
| Error (not found) | "Error: Task not found with ID: {id}\n" | stderr |
| Error (no tasks) | "Error: No tasks found in the list.\n" | stderr |

**Validation Rules**:
1. Check if task list is empty first
2. Validate ID is numeric
3. Validate task exists

---

### Flow 5: Mark Complete

**Option**: 5. Mark Complete

**Sequence**:

```
[User selects 5]

Enter task ID to mark complete: [user types "1"]

Task marked as complete!

[Menu displayed again]
```

**Messages**:

| Context | Output | Destination |
|---------|--------|-------------|
| Prompt | "Enter task ID to mark complete: " | stdout |
| Success | "Task marked as complete!\n" | stdout |
| Error (not found) | "Error: Task not found with ID: {id}\n" | stderr |
| Error (no tasks) | "Error: No tasks found in the list.\n" | stderr |

**Validation Rules**:
1. Check if task list is empty first
2. Validate ID is numeric
3. Validate task exists

---

### Flow 6: Mark Incomplete

**Option**: 6. Mark Incomplete

**Sequence**:

```
[User selects 6]

Enter task ID to mark incomplete: [user types "2"]

Task marked as incomplete!

[Menu displayed again]
```

**Messages**:

| Context | Output | Destination |
|---------|--------|-------------|
| Prompt | "Enter task ID to mark incomplete: " | stdout |
| Success | "Task marked as incomplete!\n" | stdout |
| Error (not found) | "Error: Task not found with ID: {id}\n" | stderr |
| Error (no tasks) | "Error: No tasks found in the list.\n" | stderr |

**Validation Rules**:
1. Check if task list is empty first
2. Validate ID is numeric
3. Validate task exists

---

### Flow 7: Exit

**Option**: 7. Exit

**Sequence**:

```
[User selects 7]

Goodbye!

[Application exits]
```

**Messages**:

| Context | Output | Destination |
|---------|--------|-------------|
| Farewell | "Goodbye!\n" | stdout |

**Behavior**:
- Clean exit (no error message)
- Application terminates
- No confirmation prompt (per spec)

## Error Handling Contract

### Error Message Format

All error messages follow this pattern:

```
Error: {specific error message}\n
```

Examples:
- `Error: Task description cannot be empty.\n`
- `Error: Task not found with ID: 5.\n`
- `Error: Invalid choice. Please enter 1-7.\n`

### Input Validation Errors

| Input Type | Error Condition | Message | Destination |
|------------|-----------------|---------|-------------|
| Menu choice | Non-numeric | "Error: Please enter a valid numeric task ID.\n" | stderr |
| Menu choice | Not 1-7 | "Error: Invalid choice. Please enter 1-7.\n" | stderr |
| Task ID | Non-numeric | "Error: Please enter a valid numeric task ID.\n" | stderr |
| Task ID | Empty list check | "Error: No tasks found in the list.\n" | stderr |
| Task ID | Not found | "Error: Task not found with ID: {id}.\n" | stderr |
| Description | Empty or whitespace | "Error: Task description cannot be empty.\n" | stderr |

### Validation Flow

```
Input Received
    │
    ▼
┌─────────────────────────────┐
│ Check input type            │
│ (numeric for ID/choice)     │
└───────────┬─────────────────┘
            │
      ┌─────┴─────┐
      │           │
      ▼           ▼
   Valid      Invalid
   (numeric)   (non-numeric)
      │           │
      │           ▼
      │   Output error to stderr
      │   Return to menu
      │
      ▼
┌─────────────────────────────┐
│ Check value range           │
│ (1-7 for choice,            │
│  exists for task ID)        │
└───────────┬─────────────────┘
            │
      ┌─────┴─────┐
      │           │
      ▼           ▼
   Valid      Invalid
      │           │
      │           ▼
      │   Output error to stderr
      │   Return to menu
      │
      ▼
Perform operation
```

## Output Format Summary

| Message Type | Format | Destination | Ends With |
|--------------|--------|-------------|-----------|
| Menu display | Multi-line block | stdout | Newline before prompt |
| Prompt | "{text}: " | stdout | Space and colon |
| Success | "{text}\n" | stdout | Newline |
| Error | "Error: {text}\n" | stderr | Newline |
| Warning | "Warning: {text}\n" | stderr | Newline |
| Task list | "{id}. [{status}] {desc}\n" | stdout | Newline |
| Farewell | "Goodbye!\n" | stdout | Newline |

## Unicode and Special Characters

### Supported Characters

- **Unicode Text**: All UTF-8 characters (emojis, accents, non-Latin scripts)
- **Whitespace**: Spaces, tabs, newlines (in descriptions)
- **Punctuation**: All standard punctuation marks

### Examples

```
=== Your Tasks ===
1. [ ] Buy groceries 🛒
2. [ ] Café visit ☕
3. [ ] 買東西
4. [X] Réunion avec l'équipe
```

### Handling

- Input is read as-is (no encoding conversion)
- Output is written as-is (UTF-8 by default)
- No special escaping or sanitization required
- Python 3 strings handle Unicode natively

## Performance Contract

| Operation | Maximum Response Time | Requirement |
|-----------|----------------------|-------------|
| Application startup | < 1 second | SC-006 |
| Display menu | < 0.1 second | SC-006 |
| Add task | < 5 seconds (including user input) | SC-001 |
| View tasks | < 2 seconds | SC-002 |
| Update task | < 3 seconds (including user input) | Derived |
| Delete task | < 2 seconds | Derived |
| Mark complete/incomplete | < 2 seconds | Derived |
| Application exit | < 1 second | SC-007 |

## Statelessness Contract

The CLI is **stateless** between menu displays:
- No CLI state persists between operations
- Application state is in TaskList (in-memory)
- Each operation starts from known TaskList state
- Menu always reflects current TaskList state

## Compliance Verification

| FR Requirement | Contract Compliance |
|----------------|---------------------|
| FR-001: Menu-based CLI with 7 options | ✅ Menu flow defined |
| FR-002: Unique sequential IDs | ✅ IDs displayed in success messages |
| FR-003: In-memory storage | ✅ No file/database operations |
| FR-004: 500 char description limit | ✅ Truncation warning defined |
| FR-005: No empty descriptions | ✅ Validation error defined |
| FR-006: Display ID, description, status | ✅ Task list format defined |
| FR-007: Validate task ID numeric | ✅ Validation error defined |
| FR-008: Descriptive error messages | ✅ All errors specified |
| FR-009: Return to menu after operation | ✅ All flows return to menu |
| FR-010: Exit cleanly | ✅ Exit flow defined |
| FR-011: Single-user | ✅ No multi-user flows |
| FR-012: No persistence | ✅ No file operations |
| FR-013: Unicode support | ✅ Unicode handling defined |

✅ All functional requirements supported by CLI contract.
