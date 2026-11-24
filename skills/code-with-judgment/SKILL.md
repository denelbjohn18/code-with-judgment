---
name: code-with-judgment
description: Write features, fix bugs, refactor, and review code by tracing caller contracts, simplifying logic, and checking real behavior. Applies to ordinary code changes and reviews. Skip for research, prose, and other work unrelated to code changes or review.
---

# Code With Judgment

Work from the behavior callers rely on. Make the smallest complete change that fits the codebase and verify it through the way people or programs actually use it. Scale inspection and checks to the reach and risk of the change.

## Understand the code first

- Read the relevant project instructions and inspect the current changes. Preserve unrelated work.
- Start at the affected entry points and follow the actual callers, state, and error paths. Check tested behavior, existing data, and compatibility requirements where relevant.
- Identify what must stay true through the change. For stateful code, pay attention to which code owns the state, which transitions are allowed, and when effects become visible. Follow retry, partial failure, and concurrency paths when the change can affect them.
- Look for an existing implementation or convention that fits. Confirm APIs, return values, and types from source or documentation before relying on them.

## Implement the requested behavior

- Wire the full change through the normal entry points. Reuse existing interfaces and shared rules when their contracts fit. Follow the project's language and framework idioms.
- When branches multiply, ask which cases reflect real differences. Shape the data and state around the invariant so incidental cases can disappear. Keep distinctions with observable effects, including input validation, permissions, errors, ordering, and cleanup.
- Prefer direct code. Add a helper, type, or adapter when it gives a meaningful rule a home, makes an invariant harder to violate, or removes duplication that must stay in sync.
- Give each guard and fallback a concrete input or failure it handles. Validate untrusted input at the boundary and preserve intentional failures. Avoid swallowing errors, hiding type mismatches with casts, and inventing default values that change the contract.
- Keep production wiring on the real path. Use test doubles at existing boundaries as appropriate. Leave unrelated formatting, cleanup, and dependency changes out of the diff.

## Refactor safely

- Establish the current behavior through relevant tests or a focused baseline check. Cover the behavior that the transformation could disturb.
- Make small, behavior-preserving transformations and run the relevant checks as you go. Keep intentional behavior changes in a separate step or diff when practical.
- Preserve caller and stored-data compatibility, error behavior, evaluation order, and side effects such as cleanup. Check this directly when moving, combining, or deleting code.
- For concrete transformations and their safety checks, see [Refactoring recipes](references/refactoring.md).

## Review the actual diff

- Walk each meaningful hunk back to a caller or requirement. Look for concrete failures such as a missing integration path, a changed contract, an invalid state, a lost error, or an interrupted operation left half-done.
- When implementing, fix the concrete issues within scope before handing off. For a review-only request, leave files untouched.
- Report review findings in severity or impact order. Give the file and line, the trigger and resulting failure, and a concrete fix. Omit style-only or speculative findings. If there are no supported findings, say so and identify material gaps in what you checked.

## Verify the result

- Choose targeted checks that exercise the changed behavior through real entry points. For bug fixes, use a regression check that fails before the fix and passes after it when feasible. Prefer extending a relevant existing test over adding a parallel test setup.
- Run the checks that match the risk. For example, check old inputs after a format change and use the project's integration checks for wiring or persistence changes.
- Inspect the final diff for accidental changes. Summarize the result, the checks you actually ran and their outcomes, and any material verification gap.
