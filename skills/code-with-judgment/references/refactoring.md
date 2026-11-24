# Refactoring recipes

Use the recipe that addresses a concrete problem in the current code. Identify what callers observe before the first edit. A useful baseline may include return values, errors, calls across a boundary, and the order those calls happen.

## Inline a wrapper

When a private `loadSettings(id)` just forwards to `settings.get(id)`, update its callers to use the existing API if they already depend on that boundary. Check middleware, instrumentation, argument defaults, error translation, and whether the wrapper is used as a callback before removing it. Keep a wrapper that holds an independent contract, such as a stable public API or a common authorization check.

## Share a rule that must change together

If creation and editing both enforce the same name length policy, put the policy in one place and let each caller keep its existing error handling. Before extracting, compare how the callers count length and treat empty input. Two similar branches can encode different policy, such as accepting archived items for history while hiding them in search. Keep those outcomes distinct.

## Flatten branching with a guard clause

If the successful path runs with a loaded record, return the existing missing-record result first and let the remaining path work with the record. Preserve checks that come earlier in the original code. In an endpoint that checks permission before looking up a record, moving the record check ahead of permission can expose which records exist. Also check short-circuit evaluation, resource cleanup, and logs when relocating a return.

## Extract an operation with a meaningful boundary

Extract a group of related actions when the group has a clear job and contract. For example, `writeInvoice` can own the invoice and line-item writes when they must commit together. Pass the existing transaction through when callers own it. Check commit and rollback timing, which errors propagate, and which work runs after commit. Keep code together when splitting it would force readers to reconstruct one operation across several functions.

## Represent a real invariant in the data

If two booleans allow combinations that cannot occur, consider modeling the domain states directly. A task with states `queued`, `running`, and `finished` can use one status field when those states are mutually exclusive. First check whether old records contain combinations the model would exclude and whether clients read the current fields. Preserve those contracts or make a requested schema change separately.

## Delete code after checking how it is reached

Search both names and externally used identifiers. Check exports, public routes, event subscriptions, registries, configuration, reflection, and generated bindings as applicable. A handler referenced only by a configuration string can have live callers even when an identifier search finds none. Remove obsolete registration or configuration alongside dead private code and exercise the affected entry point. Keep a public contract unless its removal is in scope.
