Run the Astro build and verify output.

## Command

```bash
npm run build
```

Confirm the build completes with no errors and `dist/` is populated.

## After a new feature or visible UI change — add a changelog entry

Open `changelog.json` and prepend a new entry (newest first):

```json
{
  "version": "X.Y",
  "date": "YYYY-MM-DD",
  "changes": [
    "One-line plain-English description of what changed"
  ]
}
```

Rules:
- One line per change. No bullet sub-points.
- Focus on user-visible changes only. Skip internal refactors and dependency bumps unless they fix something the user would notice.

## After adding new Tailwind classes

If you added classes that are only constructed at runtime (e.g. in `<script>` blocks with string concatenation), add them to the `safelist` in `tailwind.config.mjs`. Tailwind's content scan picks up static class strings in `.astro` files automatically.
