---
name: next-best-slop-removal
description: Deeply explore a software repository, generate and compare serious candidate features, modes, workflows, or subsystems that should be synthesized into stronger existing primitives or removed outright, then choose the single highest-value cut. Use when the user asks what this repo should stop carrying, which feature is unnecessary long term, where product or architecture slop has accumulated, what should be consolidated into a single path, or which surface is most worth deleting instead of extending.
---

# Next Best Slop Removal

Act as a principal engineer and product-minded simplifier inside the target repository.

Mission:
Discover the single highest-expected-value feature, mode, workflow, abstraction, or subsystem the project should stop carrying right now. The winner should be something whose long-term maintenance cost, conceptual weight, testing burden, or product dilution exceeds its strategic value, and that is better removed or synthesized into a stronger existing authority.

Do not default to dead-code cleanup. Earn the answer by understanding the repo in depth.

Do not create helper scripts for this skill. Use direct code exploration and the repository's existing tooling only.

## What Counts As Slop

- Parallel surfaces that solve the same user problem.
- Features whose carry cost spans code, docs, tests, config, CI, support, or mental overhead out of proportion to their value.
- Modes, flags, or workflows that preserve a second system instead of a sharper single path.
- Subsystems that dilute the product story because a stronger primitive already exists.
- Optional seams that look flexible in theory but mostly create branching and uncertainty in practice.

## What Does Not Count

- Mere ugliness, naming issues, or local cleanup.
- Necessary complexity in the core differentiator, safety model, or platform boundary.
- Low-level helpers that are messy but not materially shaping the product or architecture.
- "Rarely used" guesses without evidence from code, docs, tests, or real workflow traces.

## Definition Of Success

- The chosen cut is surprising but obviously right in hindsight.
- The repo becomes easier to understand, maintain, and extend because the surface area shrinks.
- The recommendation sharpens the product story instead of just shrinking code.
- The surviving path is stronger and more coherent after the synthesis or removal.

## Required Process

### 1) Reconnaissance first

Build a real mental model of the system before deciding what should go away.

- If the repo is small, read most of it.
- If the repo is large, inspect every top-level directory, main entrypoints, core abstractions, configs, tests, docs, schemas or migrations, scripts, CI, examples, and representative files across each major subsystem.
- Trace real execution paths end to end so user workflows, control flow, data flow, persistence, APIs, extension points, and operational constraints are understood.
- Keep an evidence log with concrete file paths, duplicated pathways, feature flags, alternate modes, stale docs, extra build targets, supporting tests, and other signs that a feature is being carried.

### 2) Generate serious synthesis or removal candidates

Produce multiple strong candidates before choosing one.

For each candidate, evaluate:

- user value versus carry cost
- overlap with stronger existing primitives
- maintenance, testing, and documentation burden
- conceptual drag and product-story dilution
- implementation blast radius and removal risk
- future leverage recovered if the feature disappears
- how specifically the candidate matches this repository's shape

Explicitly discard ideas that are cosmetic, speculative, weakly evidenced, or merely personal preference.

### 3) Choose exactly one

Select the single best synthesis or removal target: the highest-value thing for this codebase to stop carrying right now.

Justify the choice with concrete repo evidence.

If synthesis beats hard deletion, name the surviving authority and explain why it should own the problem alone.

### 4) Produce a concrete cut plan

Default to findings plus a concrete plan only. Do not remove code unless the user explicitly asks for execution.

When planning the cut:

- list the entrypoints, config, docs, tests, and call sites that keep the feature alive
- describe the clean single-path end state
- prefer replacement in place over compatibility shims unless the user explicitly asks for compatibility
- note what evidence is still missing if the repo cannot prove safe removal yet

## Hard Constraints

- Do not choose a trivial cleanup, dependency bump, or local refactor unless the repo overwhelmingly proves it is the best cut.
- Do not choose many small deletions. Pick one serious candidate.
- Do not confuse "hard to understand" with "should be deleted"; prove dispensability.
- Do not recommend removing a core differentiator, required safety seam, or foundational platform capability without very strong evidence.
- Do not stop at "there seems to be overlap." Prove the overlap, the carry cost, and the better surviving authority.

## Preferred Shape Of The Winning Cut

- a parallel feature that is already losing to a stronger path
- an auxiliary mode that fragments the product
- a second control surface, protocol, or workflow that keeps duplicating support costs
- a feature flag or compatibility seam that preserves a world the project should leave behind
- a subsystem that wants to be absorbed into a clearer authority instead of surviving as its own island

## Deliverable Format

Use this output structure unless the user asks for something else:

1. Codebase understanding
2. Evidence gathered from exploration
3. Candidate synthesis or removal opportunities considered
4. Chosen cut and why it wins
5. Concrete synthesis or removal plan
6. Validation already performed or proof still needed
7. Risks and tradeoffs
8. Next highest-value follow-ups after the cut

## Final Reminder

Spend more effort understanding and selecting than rushing into a verdict. Make the final answer show clearly that the recommendation emerged from repository evidence, not generic anti-feature bias.
