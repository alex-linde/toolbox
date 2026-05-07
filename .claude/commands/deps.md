Procedure for updating dependencies and keeping the audit clean.

## Workflow

```bash
npm outdated                  # see what's behind
npm install <pkg>@latest      # update one package at a time
npm run build                 # verify build passes after each update
npm audit                     # check for vulnerabilities
npm audit fix                 # auto-fix safe (non-breaking) issues
npm audit fix --force         # force-fix including breaking upgrades — review diff carefully
```

Always run `npm run build` after any package change and open `npm run preview` to confirm the output works in a browser before deploying.

## Per-package notes

| Package | Notes |
|---|---|
| `wrangler` | Patch/minor bumps always safe — deploy-only, no build API surface |
| `astro` | Minor bumps generally safe; check Astro release notes for breaking changes |
| `@astrojs/tailwind` | Keep in sync with `tailwindcss` major version |
| `alpinejs` | Client-side only; test all interactive tool pages after upgrade |
| `opentype.js` | Loaded in browser via `import`; test font-converter and font-inspector after upgrade |
| `pdf-lib` | Loaded in browser via `import`; test pdf-compress after upgrade |
| `jszip` | Used by xd-to-figma; test ZIP download after upgrade |

## After a force-fix that introduces a major bump

1. Run `npm run build` — confirm no build errors.
2. Run `npm run preview` — open in browser, exercise the affected tool(s), check console for errors.
3. If the bundle works correctly, commit the lockfile change. If it breaks, pin to the last working version.

## Vulnerability triage

Vulnerabilities in **devDependencies** (build tooling) only affect the build machine, not the shipped bundle. Still fix them to keep the audit clean, but the risk to users is zero. Vulnerabilities in anything whose output ends up in `dist/` (e.g. a bundled vendor library) are higher priority.
