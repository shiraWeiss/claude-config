---
name: writing-code
description: Use when writing or modifying code. Keep code clean - short single-abstraction functions, precise names, no magic values - and don't reinvent common functionality; look for an existing or open-source solution and verify its API in the docs before using it.
---

# Writing Code

## 1. Keep Code Clean

### Functions

- **10 lines max.** If it's longer, extract.
- **One level of abstraction per function.** A high-level function reads as a sequence of steps. It hands the tactical details to well-named inner functions.
- **Names are verbs or actions.** For example, `calculateTotal` or `fetchUser`, not `total` or `userData`.

### Variables

- **Naming is crucial.** A name must make it obvious what value the variable holds. If a reader needs context to understand it, rename it.

### Constants

- **No magic strings or numbers.** Extract them into named constants.
- **Keep constants organized.** If a file defines more than a few constants, move them to a dedicated constants file.

## 2. Don't Reinvent - Check First

### Before implementing, look for an existing solution

Don't hand-write common functionality like retries, parsing, validation, date handling, state management, or data structures. Someone has almost certainly solved it already, and their version covers edge cases you haven't hit yet. Look in this order:

1. **This codebase.** Is there already a helper or utility for it?
2. **The language's standard library and the frameworks already in use.**
3. **Public open-source packages.** Search the package registry or GitHub for a maintained, widely used option.

Write it yourself only when nothing fits, or when adding a dependency costs more than the code it replaces.

### Before calling an external API, verify it

Confirm the actual API with one lookup, even when you feel sure. Being confident is not proof. The library's types, docs, or source are.

Watch for these especially:

- **Config objects and defaults.** Don't set a field "to be safe" if it's already the default. It just adds noise.
- **Overloads.** The signature that resolves depends on how you call it. Check it, don't guess.
- **Versions.** Defaults and behavior change between versions. Check the version pinned in this project.

How to check (fastest first):

1. The library's type definitions or signatures (jump-to-definition)
2. A docs lookup tool, if configured (e.g., Context7)
3. The official docs or README
4. The library's source, when docs are thin or runtime behavior matters

Don't rely on:

- Memory of a similar API, or of another version
- How another file in this codebase calls it. That file may be wrong.
- A "sensible default" you haven't confirmed
