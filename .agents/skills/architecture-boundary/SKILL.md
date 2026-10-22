---
name: architecture-boundary
description: Analyze existing code behavior, responsibilities, state ownership, dependencies, and lifecycle before changing architecture. Use for code reviews, refactoring, debugging involving multiple objects, or changes that may affect system boundaries.
---

# Architecture Boundary

## Goal

Understand the existing system before changing it. Preserve established behavior and ownership boundaries. Make only changes justified by the request and code evidence.

## 1. Inspect the Real Implementation

Read relevant source files and follow actual call and dependency chains. Identify:

- Entry points, callers, and observable behavior.
- Object creation, dependency injection, and collaboration.
- State ownership, state transitions, and shared mutable state.
- Resource ownership, concurrency, and lifecycle/disposal.
- Dependencies between modules and relevant extension points.

Do not infer behavior from names, filenames, interfaces, or comments alone. If the inspected code does not establish a fact, mark it as unknown rather than filling the gap.

## 2. Establish Responsibilities

For each relevant object, determine what it does, what state it owns, what it depends on, who calls it, and what belongs elsewhere.

Look for duplicated behavior or state, misplaced business logic, hidden control flow, unnecessary indirection, and responsibilities moved to the wrong object.

Do not create a type for every noun or an interface for every class. Do not impose a preferred architecture or design pattern on the codebase.

## 3. Preserve Boundaries and Semantics

Before moving behavior, establish who owns the behavior, state, lifecycle, and resource, and which callers rely on it.

Keep behavior with its actual owner. Introduce shared abstractions only when the responsibility is genuinely shared and the need is supported by concrete use cases. Do not generalize a capability from one implementation to unrelated implementations.

Do not silently change public behavior, state semantics, ordering, concurrency guarantees, error handling, or disposal behavior.

## 4. Minimize Complexity and Scope

Prefer the simplest design that satisfies the confirmed requirements.

- Modify only what the request requires.
- Reuse existing mechanisms when they fit; do not preserve a flawed design just because it exists.
- Do not add speculative interfaces, base classes, managers, configuration layers, or extension points.
- Avoid unnecessary delegates such as `Func<T>` and `Action<T>`, opaque callback control flow, duplicate state, and synchronization primitives without a demonstrated need.
- Before adding a lock, identify the shared state and concurrent access that require protection. Do not remove existing synchronization without verifying its guarantees.
- Distinguish file organization from responsibility separation; moving code alone does not fix a boundary.

## 5. Design Before Material Changes

For architecture reviews, report evidence-based findings before editing.

For a substantial architectural refactor, first describe:
1. Existing behavior and responsibility boundaries.
2. Problems and the code evidence supporting them.
3. Proposed ownership and object collaboration.
4. Required changes and explicit scope exclusions.
5. Lifecycle, state-transition, compatibility, and concurrency implications.
6. Focused verification criteria.

Separate confirmed facts from design proposals. If an unresolved requirement or architectural decision materially changes the design, ask for clarification or present alternatives and wait for confirmation. For clear, localized changes, avoid unnecessary planning ceremony.

## 6. Implement and Verify

After the design is established:

- Change only the necessary files and responsibilities.
- Preserve public contracts, lifecycle, disposal, and integration behavior unless the request explicitly changes them.
- Follow existing naming, formatting, dependency-injection, and organization conventions.
- Avoid unrelated cleanup and speculative improvements.
- Verify affected success paths, error paths, lifecycle, and concurrency as relevant.
- Use focused tests and existing test conventions when appropriate; do not introduce a large testing or documentation workflow without a concrete requirement.

Never claim a build, test, or check passed unless it was actually run. State what could and could not be verified.

## Output

Keep the response proportional to the task.

For reviews, summarize confirmed behavior, responsibility/state ownership, dependencies/lifecycle, evidence-backed problems, proposed changes, risks, and unknowns.

For implementation, summarize what changed and why, affected boundaries, behavior preservation, actual verification results, and remaining uncertainty. Do not repeat the full analysis in the final summary.

## Final Constraint

Derive the design from actual code, confirmed requirements, and established decisions—not from a preferred pattern. Make the smallest justified change that corrects the identified problem.
