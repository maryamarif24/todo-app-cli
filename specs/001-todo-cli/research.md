# Research & Technology Decisions: Phase I Todo CLI

**Branch**: `001-todo-cli` | **Date**: 2024-12-24
**Status**: Complete

## Overview

This document captures research findings and technology decisions for Phase I of the Todo CLI application. All decisions are derived from the Phase I specification and global constitution.

## Decision 1: Python Version

### Options Considered
1. Python 3.11 (LTS)
2. Python 3.12 (stable)
3. Python 3.13+ (latest)

### Decision: Python 3.13+

**Rationale**: Constitution mandates Python 3.13+ for all backend code. Python 3.13 offers modern syntax (match statements, walrus operators), improved type hints, and dataclass enhancements that support clean, maintainable code.

**Alternatives Considered**:
- Python 3.11: Stable LTS but lacks latest language features
- Python 3.12: Good middle ground but not the constitution-mandated version

## Decision 2: External Dependencies

### Options Considered
1. Click - CLI framework
2. argparse - Standard library
3. No framework - Manual input parsing

### Decision: Python Standard Library Only (argparse or manual parsing)

**Rationale**:
- Constitution prefers standard library for simple tasks
- Phase I scope is intentionally simple (menu-based, 7 options)
- No dependencies reduces complexity and aligns with "simple functions" principle
- argparse provides sufficient functionality for menu-based CLI

**Alternatives Considered**:
- Click: Over-engineering for Phase I scope. Click is great but adds external dependency.
- Manual parsing: Too much code for input validation. argparse handles this better.

## Decision 3: Application Architecture

### Options Considered
1. Multi-file (separate modules for data, CLI, main)
2. Single-file (all code in one file)

### Decision: Single File (`src/todo_app.py`)

**Rationale**:
- Phase I is intentionally simple with limited scope
- All functionality fits in ~200-300 lines with clear separation
- Avoids over-engineering and abstraction
- Aligns with constitution "simple functions" principle
- Easier to understand and maintain for Phase I scale

**Alternatives Considered**:
- Multi-file: Adds unnecessary complexity for Phase I. Module boundaries add overhead without benefit at this scale. May be appropriate for Phase II+.

## Decision 4: Data Structures

### Options Considered
1. List only (iterate to find by ID)
2. Dictionary only (ID as key)
3. Hybrid: List for order, dictionary for lookup

### Decision: List for Ordered Tasks

**Rationale**:
- Spec requires ordered tasks by creation order
- List provides natural ordering
- O(n) lookup is acceptable for ~100 tasks (Phase I scale)
- Simple and straightforward implementation

**Alternatives Considered**:
- Dictionary only: Doesn't preserve insertion order naturally (though Python 3.7+ dicts maintain order, this is an implementation detail)
- Hybrid: Over-engineering for Phase I. Two data structures to keep synchronized adds complexity without measurable benefit at this scale.

## Decision 5: Task ID Generation

### Options Considered
1. Sequential integers (1, 2, 3...)
2. UUID/GUID
3. Timestamp-based

### Decision: Sequential Integers Starting at 1

**Rationale**:
- Spec requirement: "unique sequential numeric ID"
- Simple for users to reference (type "1" not "550e8400...")
- Easy to implement and understand
- Aligns with spec's user-friendly CLI approach

**Alternatives Considered**:
- UUID: Over-engineering for single-user in-memory app. UUIDs are unnecessary complexity.
- Timestamp-based: Same as UUID - unnecessary complexity for Phase I scope.

## Decision 6: Task Representation

### Options Considered
1. Simple class with __init__
2. dataclass decorator
3. NamedTuple
4. Dictionary

### Decision: Simple Class or dataclass

**Rationale**:
- Both provide clear structure and type hints
- dataclass is more concise and provides __eq__, __repr__ automatically
- Simple class gives more control (optional for Phase I)
- Constitution mentions "dataclasses for state" as permitted pattern

**Selected Approach**: dataclass for conciseness and automatic methods

**Alternatives Considered**:
- NamedTuple: Immutable, but we need mutability for status updates
- Dictionary: No type safety, harder to catch errors at development time

## Decision 7: Menu Input Handling

### Options Considered
1. argparse with subcommands
2. Simple input() with manual validation
3. Third-party CLI library (click, typer)

### Decision: Simple input() with Manual Validation

**Rationale**:
- Menu-based CLI (not command-line arguments)
- input() prompts are more user-friendly for menu navigation
- Manual validation is straightforward (check if 1-7, check if numeric)
- No external dependencies

**Alternatives Considered**:
- argparse: Designed for command-line arguments, not interactive menus
- Third-party library: Over-engineering for simple menu loop

## Decision 8: Error Output Destination

### Options Considered
1. stdout (same as normal output)
2. stderr (standard error)
3. Separate logging mechanism

### Decision: Error Messages to stderr

**Rationale**:
- Standard practice for CLI applications
- Allows users to redirect normal output separately from errors
- Spec mentions "errors → stderr" in CLI interaction flow
- Simple and aligns with Unix conventions

**Alternatives Considered**:
- stdout: Mixes errors with normal output, makes redirection difficult
- Separate logging: Over-engineering for Phase I (no logging framework requirement)

## Decision 9: Task Description Length Limit

### Options Considered
1. No limit (accept any length)
2. Truncate to 500 chars with warning
3. Reject if over 500 chars

### Decision: Truncate to 500 Characters with Warning

**Rationale**:
- Spec requirement: descriptions longer than 500 chars are truncated with warning
- Prevents memory issues from extremely long descriptions
- User-friendly (still accepts input, just shortens)
- Warning message keeps user informed

**Alternatives Considered**:
- No limit: Security risk (could cause memory issues with malicious input)
- Reject if over 500: Less user-friendly, requires user to re-type

## Decision 10: Unicode and Special Characters

### Options Considered
1. ASCII only (reject Unicode)
2. UTF-8 encoding support
3. Platform-dependent default

### Decision: UTF-8/Unicode Support (Python Default)

**Rationale**:
- Spec requirement: accept Unicode characters and emojis
- Python 3 strings are Unicode by default
- No special handling needed
- Modern CLI applications should support Unicode

**Alternatives Considered**:
- ASCII only: Rejects spec requirement (emojis, Unicode text not allowed)
- Platform-dependent: Unnecessary complexity, Python handles Unicode consistently

## Summary of Decisions

| Decision | Selected | Rationale |
|----------|----------|-----------|
| Python Version | 3.13+ | Constitution mandate, modern features |
| External Dependencies | Standard Library Only | Phase I simplicity, no complexity |
| Architecture | Single File | Fits Phase I scope, clean separation within file |
| Data Structures | List for Ordered Tasks | Natural ordering, simple, acceptable performance |
| ID Generation | Sequential Integers (1, 2, 3...) | Spec requirement, user-friendly |
| Task Representation | dataclass | Concise, type-safe, automatic methods |
| Menu Input | input() with validation | Menu-based, simple, no dependencies |
| Error Output | stderr | Standard practice, spec requirement |
| Description Limit | Truncate to 500 + warning | Spec requirement, user-friendly |
| Unicode Support | Python default (Unicode) | Spec requirement, modern standard |

## Compliance Verification

All decisions align with:
- ✅ Phase I specification (spec.md)
- ✅ Global constitution (constitution.md)
- ✅ Phase I technology matrix (Python, CLI, in-memory only)
- ✅ "Simple functions" principle (no over-engineering)
- ✅ No feature invention (only spec requirements)
- ✅ No future phase concepts (no persistence, web, multi-user)

## Research Complete

✅ All unknowns resolved. Proceed to Phase 1: Design & Contracts.
