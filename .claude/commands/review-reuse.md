Review changed files for code that duplicates existing utilities or helpers.

## Steps

1. Run `git diff` to get the changed files and their content.
2. For each changed file, check for:
   - Functions that duplicate anything already in `src/lib/utils.ts` (`fmtBytes`, `baseName`, `dl`, `flashBtn`) or `src/lib/color.ts` (`hexToRgb`, `rgbToHex`, `rgbToHsl`, `hslToRgb`, `isLight`, `relativeLuminance`, `mixColors`)
   - New helper functions that also appear in other tool pages (e.g. `loadPdfJs`, drop-zone wiring, `showStatus`/`showSt`)
   - Inline logic that hand-rolls string manipulation, file downloads, or color math when a shared util already covers it
3. Read `src/lib/utils.ts` and `src/lib/color.ts` to confirm what's available before flagging anything.
4. Report each finding as: **file · function name · what it should use instead**. If the code is clean, say so.
5. Fix any issues found — replace duplicates with imports from the shared lib. If the shared lib is missing the function, add it there first.

## Project context

- Shared utilities live in `src/lib/utils.ts` and `src/lib/color.ts` — they are rarely imported by tool pages (pre-existing debt), but new code should use them.
- Tool `<script>` blocks use standard ESM `import` — imports from `@/lib/utils.ts` work fine at build time.
- The PDF.js lazy-loader (`loadPdfJs`, `PDFJS_CDN` constant, `pdfjsReady` flag) is duplicated across PDF tool pages — flag but don't extract unless a `src/lib/pdfjs.ts` already exists.
