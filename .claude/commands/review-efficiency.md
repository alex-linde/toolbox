Review changed files for performance and efficiency issues.

## Steps

1. Run `git diff` to get the changed files and their content.
2. For each changed file, check for:
   - **Unnecessary work** — values recomputed on every event when they could be cached after the triggering action (e.g. re-reading `ImageData` from a canvas on every count-pill click when the image hasn't changed)
   - **Missing debounce** — expensive operations (canvas reads, linear scans, DOM rebuilds) bound directly to `input` events that fire on every keystroke
   - **Redundant DOM queries** — `document.getElementById` or `querySelector` called multiple times inside the same closure for the same element; assign to a `const` once
   - **Listener churn** — `innerHTML = ''` + full DOM rebuild + re-attaching all event listeners on every state change; prefer in-place DOM updates or sorting existing nodes
   - **Sequential async where parallel is safe** — `await` inside a `for` loop when operations are independent (`Promise.all` would be faster)
   - **Memory**: object URLs created but never revoked, event listeners added on every navigation without a guard, unbounded arrays
   - **CDN/library re-loading** — dynamic `<script>` injection without a `loaded` flag guard; confirm the guard is in place before the injection
3. For each finding, note: **file:line · issue · severity (low / med / high) · suggested fix**. If the code is clean, say so.
4. Fix med/high severity issues directly. Flag low severity issues in your report but skip fixing them unless trivial.

## Project context

- Tool pages use vanilla TypeScript `<script>` blocks — no framework reactivity, so derived state must be manually cached.
- PDF.js is loaded from CDN on demand; the `pdfjsReady` flag pattern is correct — don't remove it.
- `astro:page-load` fires on every View Transition navigation; any listener registered on `document` inside a `<script>` block accumulates across navigations unless guarded with `data-initialized` or a one-time flag.
