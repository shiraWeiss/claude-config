# Writing Code

**Skill Name**: `writing-code`

## Description

Use when writing or modifying code. Covers two rules: verify external APIs before calling them, and keep code clean and self-explanatory.

## 1. Check the Docs First

Before calling an external framework, library, or SDK, confirm the actual API with one lookup, even when you feel sure. Being confident is not proof. The library's types, docs, or source are.

Watch for these especially:

- **Config objects and defaults.** Don't set a field "to be safe" if it's already the default. It just adds noise.
- **Overloads.** The signature that resolves depends on how you call it. Check it, don't guess.
- **Existing helpers.** Before hand-writing something a mature library likely provides, check. The library's version already handles edge cases you haven't hit yet.
- **Versions.** Defaults and behavior change between versions. Check the version pinned in this project.

### How to check (fastest first)

1. The library's type definitions or signatures (jump-to-definition)
2. A docs lookup tool, if configured (e.g., Context7)
3. The official docs or README
4. The library's source, when docs are thin or runtime behavior matters

### Don't rely on

- Memory of a similar API, or of another version
- How another file in this codebase calls it. That file may be wrong.
- A "sensible default" you haven't confirmed

### When to skip

Skip this for first-party code and for well-known language built-ins. Also skip it if a mistake would be caught right away by the compiler or by a test you're about to write. Check first when a mistake could pass locally and only show up in CI, in production, or as a subtle behavior change.

## 2. Keep Code Clean

### Functions

- **10 lines max.** If it's longer, extract.
- **One level of abstraction per function.** A high-level function reads as a sequence of steps. It hands the tactical details to well-named inner functions.
- **Names are verbs or actions.** For example, `calculateTotal` or `fetchUser`, not `total` or `userData`.

### Variables

- **Naming is crucial.** A name must make it obvious what value the variable holds. If a reader needs context to understand it, rename it.

### Constants

- **No magic strings or numbers.** Extract them into named constants.
- **Keep constants organized.** If a file defines more than a few constants, move them to a dedicated constants file.
