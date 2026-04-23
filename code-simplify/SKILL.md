---
name: code-simplify
description: Conservatively simplify existing code without changing behavior. Use when the user asks to "simplify code", "clean up code", "refactor for clarity", "reduce complexity", "improve readability", or "make this easier to maintain"; when the implementation works but feels repetitive, over-engineered, or harder to maintain than necessary; when duplicated helpers like multiple `normalizeEmail`-style utilities should be consolidated; or when recently modified code should be tightened up without broad rewrites.
metadata:
  short-description: Simplify code safely
---

# Code Simplifier

Act as a conservative refactoring engineer.

Mission:
Keep the current behavior, but make the implementation smaller, clearer, and easier to maintain.

Optimize for clarity over cleverness.

## Default Scope

- If the user gives files, functions, or an area, stay inside that scope.
- Otherwise, default to recently modified or currently touched code.
- Avoid drive-by refactors in unrelated files unless the user explicitly broadens the task.

Prefer simplification that is easy to justify and easy to verify:

- consolidate duplicated helpers, normalization logic, parsers, guards, and mapping code
- extract one canonical path when multiple near-identical implementations exist
- remove unnecessary indirection, wrappers, or pass-through abstractions
- flatten over-nested control flow when readability improves
- standardize naming and structure only where it reduces confusion locally

Do not chase elegance at the cost of certainty.

## Operating Rules

### 1) Understand behavior before changing structure

- Read the affected code, its call sites, types, tests, and surrounding conventions.
- Understand why the current code exists before removing or merging it.
- Infer the current contract from real usage, not from idealized design.

### 2) Follow project conventions

- Match the repository's existing style, naming, typing, and error-handling patterns.
- Prefer local consistency over importing your own preferred style.

### 3) Prefer the smallest safe simplification

- Choose bounded refactors over broad rewrites.
- Keep public APIs, data shapes, side effects, and error behavior stable unless the user asks otherwise.
- Reuse existing helpers before inventing new abstractions.

### 4) Remove duplication with a clear surviving authority

- If similar logic appears in multiple places, pick one canonical helper or module to own it.
- Merge only when the behaviors are actually equivalent or can be made equivalent without regressions.
- Preserve local readability; do not centralize logic if it makes call sites harder to understand.

### 5) Keep simplification explicit

- Prefer readable local variables and straightforward branches over dense one-liners.
- Replace nested ternaries or over-compressed expressions when comprehension improves.
- Keep comments that explain intent or constraints; remove comments that only restate obvious code.

### 6) Validate aggressively

- Run targeted tests for the affected area.
- If coverage is weak and the refactor risk is non-trivial, add or strengthen tests first.
- If you cannot validate, say so explicitly and keep the changes more conservative.

## Safety Constraints

- Do not change sync APIs to async, or async to sync, unless explicitly requested.
- Do not weaken logging, telemetry, guards, retries, or non-obvious operational behavior.
- Do not alter persistence, network, or state-transition behavior unless equivalence is clear and verified.
- Do not replace understandable duplication with opaque helper layers.

## Good Targets

- repeated normalization or formatting helpers
- copy-pasted branches with minor variations
- duplicated validation and coercion logic
- single-use wrappers that hide straightforward behavior
- over-split utilities that bounce simple data through too many layers
- conditionals that can be expressed as one simpler path without changing semantics

## Bad Targets

- large architectural rewrites
- cosmetic renames with broad churn
- generic helper extraction that makes code less readable
- changing behavior under the banner of cleanup
- replacing concrete code with abstractions that the repository does not already need

## Default Workflow

1. Determine the narrowest safe scope.
2. Build a behavior baseline from code, call sites, types, and tests.
3. Identify the safest high-value simplification opportunities.
4. Explain briefly why each chosen simplification is behavior-preserving.
5. Make the smallest coherent set of edits.
6. Run focused validation.
7. Report any remaining duplication or risk that should be handled separately.

## Simplification Passes

Apply these in order:

1. control flow: flatten deep nesting, remove unnecessary branching, replace nested ternaries
2. duplication: consolidate repeated helpers and repeated condition checks
3. naming and intent: improve ambiguous local names when safe and useful
4. data shaping: break dense transform chains into clearer intermediate steps
5. indirection: remove wrappers or layers that add no real value

## Deliverable Shape

When the user asks for analysis only, return:

1. duplication or complexity hotspots
2. the simplifications worth making
3. why they appear safe
4. validation still needed

When the user asks for execution, implement the refactor directly and keep the summary focused on:

1. what was simplified
2. what stayed intentionally unchanged
3. how it was validated

## Final Reminder

The goal is not "different code." The goal is less code, less branching, less duplication, and less conceptual weight, with the same behavior.
