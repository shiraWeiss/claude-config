# Writing Code: Check the Docs First

**Skill Name**: `writing-code`

## Description

Use before implementing any call into an external framework, library, or SDK — even something that looks trivial. Check the framework's own docs or source before writing the call, rather than assuming API behavior from memory, convention, or pattern-matching against other codebases.

## The rule

Before writing a call into an external framework, library, or SDK — including something that looks small enough to just type from memory — spend one lookup confirming the actual current API surface. Do this even when you're confident. Confidence about a framework's behavior is not evidence about it; the framework's own docs, type definitions, or source are.

This applies most where the mistake is easiest to make and hardest to notice:

- **Configuration object shapes and their defaults.** A field you set explicitly "to be safe" may already be the library's default — harmless until it multiplies across every call site as boilerplate that looks like completeness but is actually noise.
- **Overloaded functions or constructors.** When a factory function has multiple call signatures, the one that resolves depends on exactly how you call it — wrapping the callback, the inferred generic, the argument order. Assuming "the docs I remember" resolves the same overload you're about to trigger is a guess, not a fact.
- **"There must be a helper for this."** If you're about to hand-write something a mature library plausibly already does (a state reducer, a retry policy, a validation helper), check before writing it — the reason to check isn't just to avoid duplicate code, it's that the library's version is usually shaped by edge cases you don't know about yet.
- **Version-specific behavior.** An API's defaults, overload resolution, and even error messages can change between minor versions. What you remember may be true of a version you used before, not the one actually pinned in this project's lockfile.

## Why this is a rule and not a suggestion

Two concrete failures from real sessions, both avoidable with one doc check:

1. **A `tool()` factory call that only failed in CI.** `tool()` from `@langchain/core` is an overloaded factory. When the callback is wrapped in `traceable(...)`, its deferred `Parameters<T>` tuple isn't an inference site, so no schema-specific overload matches and the fallback overload returns a union of tool shapes — a union whose `.invoke()` is uncallable (TS2349). This never reproduced locally because the local dependency resolution happened to pick a looser type; CI's `yarn workspaces focus` resolved a stricter one. The fix was to match a sibling file's proven pattern (`new DynamicStructuredTool({...})`, a concrete class with one call signature) instead of the ambiguous factory. A five-minute read of `tool()`'s actual overload signatures before using it — not after CI turned red — would have caught this immediately.

2. **Boilerplate that assumed the library needed help it doesn't.** A LangGraph `Annotation.Root({...})` block spelled out `{ reducer: (_prev, next) => next, default: () => null }` on every single field, including fields whose actual desired behavior was just "last write wins" — which is `Annotation<T>()`'s built-in behavior with no config object at all. The explicit reducer wasn't wrong, just redundant: fourteen lines that could have been one-liners, written because it felt more defensive to spell out the reducer than to trust the library's own default. Checking `Annotation`'s type signature or a single example in LangGraph's docs before writing the first field would have shown the shorthand immediately, instead of it surfacing later as a review comment.

Neither of these was a knowledge gap that required deep research. Both were one doc lookup away from being caught before they were written, not after.

## How to check

In descending order of speed, use whichever is available:

1. **The library's own TypeScript types**, if it ships them — `node_modules/<package>/dist/*.d.ts`, or jump-to-definition. Types are ground truth for signatures, overloads, and optional fields; they don't go stale the way memory does.
2. **A docs/API lookup tool**, if one is configured (e.g., Context7, or a project's `docs-lookup` skill) — faster than raw web search and usually pulls the version-correct page.
3. **The library's official docs site or README**, especially for anything involving configuration objects, defaults, or "the recommended way to do X."
4. **The library's own source**, if docs are thin or you need to confirm actual runtime behavior rather than documented intent (the `tool()` overload case above was ultimately understood by reading the factory's own type overloads, not its prose docs).

Do **not** rely on:

- Memory of how a similar-sounding API worked in a different version, project, or library.
- Pattern-matching against how another file in the same codebase already calls the library — that file may itself be wrong, or may be calling a different overload than you're about to.
- Assuming a "sensible default" without confirming it's actually the library's default, not just a sensible-sounding guess.

## When this doesn't apply

This is about calls into code you don't control. It is not a mandate to research every line of first-party code you're writing yourself, and it's not a reason to stall on something genuinely trivial and well-known (e.g., `Array.prototype.map`). The line is: if getting it wrong would only be caught by a type checker or a test you're about to write anyway, memory is probably fine. If getting it wrong could pass local checks and only surface in CI, in production, or as a subtle behavioral difference nobody notices until much later — check first.
