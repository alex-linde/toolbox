Complete checklist to ship a finished feature or fix to production.

## From a feature branch

```bash
git checkout main
git merge feature/<name>
npm run deploy        # astro build + wrangler deploy to Cloudflare
```

Then open `changelog.json` and prepend an entry if the version was bumped (see `/build` for format rules).

## From main (bug fix, copy change, small tweak)

```bash
npm run deploy
```

## Checklist before deploying

- [ ] Build completed with no errors
- [ ] All tool pages present in `dist/tools/`
- [ ] Changelog entry added for any user-visible change
- [ ] Tested in browser via `npm run preview` — golden path works, no console errors

## Cloudflare deployment note

`wrangler deploy` publishes `dist/` via the Worker in `worker/counter.js`. The Worker routes all non-API requests to the static `dist/` assets and handles `/fontpair` for AI font pairing. If the deploy succeeds but the live site looks wrong, check that `astro build` ran successfully before deploying.

The `ANTHROPIC_KEY` secret must be set in the Cloudflare dashboard (Workers → lindetoolbox → Settings → Variables) for font pairing to work.
