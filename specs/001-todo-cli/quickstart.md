# Quickstart Guide: Phase I Todo CLI

**Branch**: `001-todo-cli` | **Date**: 2024-12-24
**Spec**: [spec.md](spec.md) | **Plan**: [plan.md](plan.md)

## Overview

This guide provides quick instructions for running and using the Phase I Todo CLI application.

## Prerequisites

### Required

- **Python 3.13+**: Download from [python.org](https://python.org/downloads/)
- **Operating System**: Windows, macOS, or Linux

### Verify Installation

```bash
python --version
# Output should be Python 3.13.x or higher
```

## Installation

### Step 1: Clone or Navigate to Project

If repository exists:
```bash
cd path/to/hackathon-todo
```

### Step 2: Verify Project Structure

```bash
# Should see:
# src/
# └── todo_app.py
# specs/
# └── 001-todo-cli/
```

## Running the Application

### Start the Application

```bash
python src/todo_app.py
```

**Expected Output**:
```
=== Todo Application ===
1. Add Task
2. View Tasks
3. Update Task
4. Delete Task
5. Mark Complete
6. Mark Incomplete
7. Exit

Enter your choice (1-7):
```

## Usage Examples

### Example 1: Adding Tasks

```
=== Todo Application ===
1. Add Task
2. View Tasks
3. Update Task
4. Delete Task
5. Mark Complete
6. Mark Incomplete
7. Exit

Enter your choice (1-7): 1
Enter task description: Buy groceries 🛒
Task added successfully! (ID: 1)

=== Todo Application ===
1. Add Task
2. View Tasks
3. Update Task
4. Delete Task
5. Mark Complete
6. Mark Incomplete
7. Exit

Enter your choice (1-7): 1
Enter task description: Walk the dog
Task added successfully! (ID: 2)

=== Todo Application ===
1. Add Task
2. View Tasks
3. Update Task
4. Delete Task
5. Mark Complete
6. Mark Incomplete
7. Exit

Enter your choice (1-7): 1
Enter task description: Do laundry
Task added successfully! (ID: 3)
```

### Example 2: Viewing Tasks

```
=== Todo Application ===
1. Add Task
2. View Tasks
3. Update Task
4. Delete Task
5. Mark Complete
6. Mark Incomplete
7. Exit

Enter your choice (1-7): 2

=== Your Tasks ===
1. [ ] Buy groceries 🛒
2. [ ] Walk the dog
3. [ ] Do laundry

=== Todo Application ===
1. Add Task
2. View Tasks
3. Update Task
4. Delete Task
5. Mark Complete
6. Mark Incomplete
7. Exit

Enter your choice (1-7):
```

### Example 3: Marking Tasks Complete

```
=== Todo Application ===
1. Add Task
2. View Tasks
3. Update Task
4. Delete Task
5. Mark Complete
6. Mark Incomplete
7. Exit

Enter your choice (1-7): 5
Enter task ID to mark complete: 1
Task marked as complete!

=== Todo Application ===
1. Add Task
2. View Tasks
3. Update Task
4. Delete Task
5. Mark Complete
6. Mark Incomplete
7. Exit

Enter your choice (1-7): 5
Enter task ID to mark complete: 3
Task marked as complete!

=== Todo Application ===
1. Add Task
2. View Tasks
3. Update Task
4. Delete Task
5. Mark Complete
6. Mark Incomplete
7. Exit

Enter your choice (1-7): 2

=== Your Tasks ===
1. [X] Buy groceries 🛒
2. [ ] Walk the dog
3. [X] Do laundry

=== Todo Application ===
1. Add Task
2. View Tasks
3. Update Task
4. Delete Task
5. Mark Complete
6. Mark Incomplete
7. Exit

Enter your choice (1-7):
```

### Example 4: Updating a Task

```
=== Todo Application ===
1. Add Task
2. View Tasks
3. Update Task
4. Delete Task
5. Mark Complete
6. Mark Incomplete
7. Exit

Enter your choice (1-7): 3
Enter task ID to update: 2
Enter new description: Walk the dog in the park
Task updated successfully!

=== Todo Application ===
1. Add Task
2. View Tasks
3. Update Task
4. Delete Task
5. Mark Complete
6. Mark Incomplete
7. Exit

Enter your choice (1-7): 2

=== Your Tasks ===
1. [X] Buy groceries 🛒
2. [ ] Walk the dog in the park
3. [X] Do laundry

=== Todo Application ===
1. Add Task
2. View Tasks
3. Update Task
4. Delete Task
5. Mark Complete
6. Mark Incomplete
7. Exit

Enter your choice (1-7):
```

### Example 5: Deleting a Task

```
=== Todo Application ===
1. Add Task
2. View Tasks
3. Update Task
4. Delete Task
5. Mark Complete
6. Mark Incomplete
7. Exit

Enter your choice (1-7): 4
Enter task ID to delete: 2
Task deleted successfully!

=== Todo Application ===
1. Add Task
2. View Tasks
3. Update Task
4. Delete Task
5. Mark Complete
6. Mark Incomplete
7. Exit

Enter your choice (1-7): 2

=== Your Tasks ===
1. [X] Buy groceries 🛒
3. [X] Do laundry

=== Todo Application ===
1. Add Task
2. View Tasks
3. Update Task
4. Delete Task
5. Mark Complete
6. Mark Incomplete
7. Exit

Enter your choice (1-7):
```

### Example 6: Marking Incomplete

```
=== Todo Application ===
1. Add Task
2. View Tasks
3. Update Task
4. Delete Task
5. Mark Complete
6. Mark Incomplete
7. Exit

Enter your choice (1-7): 6
Enter task ID to mark incomplete: 1
Task marked as incomplete!

=== Todo Application ===
1. Add Task
2. View Tasks
3. Update Task
4. Delete Task
5. Mark Complete
6. Mark Incomplete
7. Exit

Enter your choice (1-7): 2

=== Your Tasks ===
1. [ ] Buy groceries 🛒
3. [X] Do laundry

=== Todo Application ===
1. Add Task
2. View Tasks
3. Update Task
4. Delete Task
5. Mark Complete
6. Mark Incomplete
7. Exit

Enter your choice (1-7):
```

### Example 7: Exiting

```
=== Todo Application ===
1. Add Task
2. View Tasks
3. Update Task
4. Delete Task
5. Mark Complete
6. Mark Incomplete
7. Exit

Enter your choice (1-7): 7
Goodbye!

[Application exits, all data lost]
```

## Error Handling Examples

### Example: Empty Task Description

```
=== Todo Application ===
1. Add Task
2. View Tasks
3. Update Task
4. Delete Task
5. Mark Complete
6. Mark Incomplete
7. Exit

Enter your choice (1-7): 1
Enter task description:
Error: Task description cannot be empty.

=== Todo Application ===
1. Add Task
2. View Tasks
3. Update Task
4. Delete Task
5. Mark Complete
6. Mark Incomplete
7. Exit

Enter your choice (1-7):
```

### Example: Invalid Task ID

```
=== Todo Application ===
1. Add Task
2. View Tasks
3. Update Task
4. Delete Task
5. Mark Complete
6. Mark Incomplete
7. Exit

Enter your choice (1-7): 3
Enter task ID to update: 99
Error: Task not found with ID: 99

=== Todo Application ===
1. Add Task
2. View Tasks
3. Update Task
4. Delete Task
5. Mark Complete
6. Mark Incomplete
7. Exit

Enter your choice (1-7):
```

### Example: Non-Numeric Input

```
=== Todo Application ===
1. Add Task
2. View Tasks
3. Update Task
4. Delete Task
5. Mark Complete
6. Mark Incomplete
7. Exit

Enter your choice (1-7): abc
Error: Please enter a valid numeric task ID.

=== Todo Application ===
1. Add Task
2. View Tasks
3. Update Task
4. Delete Task
5. Mark Complete
6. Mark Incomplete
7. Exit

Enter your choice (1-7):
```

### Example: Invalid Menu Choice

```
=== Todo Application ===
1. Add Task
2. View Tasks
3. Update Task
4. Delete Task
5. Mark Complete
6. Mark Incomplete
7. Exit

Enter your choice (1-7): 99
Error: Invalid choice. Please enter 1-7.

=== Todo Application ===
1. Add Task
2. View Tasks
3. Update Task
4. Delete Task
5. Mark Complete
6. Mark Incomplete
7. Exit

Enter your choice (1-7):
```

### Example: Empty Task List

```
=== Todo Application ===
1. Add Task
2. View Tasks
3. Update Task
4. Delete Task
5. Mark Complete
6. Mark Incomplete
7. Exit

Enter your choice (1-7): 2

No tasks found.

=== Todo Application ===
1. Add Task
2. View Tasks
3. Update Task
4. Delete Task
5. Mark Complete
6. Mark Incomplete
7. Exit

Enter your choice (1-7):
```

## Common Tasks

| Action | Menu Option | Example |
|--------|-------------|---------|
| Add new task | 1 | Type "1", enter description |
| See all tasks | 2 | Type "2" |
| Edit task | 3 | Type "3", enter ID, enter new description |
| Remove task | 4 | Type "4", enter ID |
| Mark done | 5 | Type "5", enter ID |
| Mark not done | 6 | Type "6", enter ID |
| Quit | 7 | Type "7" |

## Important Notes

### Data Persistence

⚠️ **WARNING**: All data is lost when application exits. Phase I uses in-memory storage only. Do not rely on data persisting between sessions.

### Task ID Behavior

- IDs are sequential (1, 2, 3...)
- IDs are never reused
- After deleting a task, its ID is not reassigned

### Unicode Support

- Emojis and special characters are fully supported
- Example: "Buy groceries 🛒", "Café visit ☕"
- No special encoding needed

### Performance

- Application starts within 1 second
- Add task completes within 5 seconds
- View tasks completes within 2 seconds

## Troubleshooting

### Issue: Python not found

**Error**: `python: command not found`

**Solution**: Install Python 3.13+ from [python.org](https://python.org/downloads/) or use `python3` instead of `python`

### Issue: Wrong Python version

**Error**: Python version is 3.11 or earlier

**Solution**: Update to Python 3.13+ as required by constitution

### Issue: File not found

**Error**: `No such file or directory: 'src/todo_app.py'`

**Solution**: Ensure you're in the project root directory and the file exists

## Next Steps

After completing Phase I usage:

1. **Review Specification**: See [spec.md](spec.md) for complete requirements
2. **Review Plan**: See [plan.md](plan.md) for architecture details
3. **Proceed to Implementation**: Use `/sp.tasks` to generate implementation tasks

## Support

For questions or issues:
- Review [spec.md](spec.md) for requirements
- Review [plan.md](plan.md) for technical details
- Review [data-model.md](data-model.md) for data structures
- Review [contracts/cli-contract.md](contracts/cli-contract.md) for CLI interface
