Review changed files for hacky or low-quality code patterns.

## Steps

1. Run `git diff` to get the changed files and their content.
2. For each changed file, check for:
   - **Redundant state** — module-level variables that duplicate what's already in the DOM or derivable from other state (e.g. storing a filename separately when the `File` object is already held)
   - **Copy-paste with variation** — near-duplicate blocks that should be unified (e.g. three places that update the same counter label)
   - **Stringly-typed IDs** — tab/mode keys used as raw string literals in both HTML and JS with no shared constant; a typo will fail silently at runtime
   - **Unnecessary HTML nesting** — wrapper `<div>` or `<span>` elements that add no layout value (check if inner-element props already provide the needed behaviour)
   - **Nested conditionals** — ternary chains or `if/else` 3+ levels deep; flatten with early returns, guard clauses, or a lookup table
   - **Unnecessary comments** — comments that explain *what* the code does (well-named identifiers do that), reference the task/caller, or narrate the change; keep only non-obvious *why* (hidden constraints, workarounds, subtle invariants)
   - **`class:list` with a single condition** — `class:list={[{ 'foo': expr }]}` can just be `class={expr ? 'foo' : ''}`
   - **`hidden` + layout class in same element** — Tailwind `hidden` sets `display: none`; pairing it with `flex-col` or `grid` in the same initial class list means removing `hidden` won't apply `flex`/`grid` — the layout class must be added in JS instead
3. Report each finding as: **file:line · pattern · suggested fix**. If the code is clean, say so.
4. Fix each issue directly.
