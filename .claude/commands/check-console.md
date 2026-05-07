Check for browser console errors and fix all of them before continuing.

## Step 1 — run a clean build

```bash
npm run build
```

The build must complete with no errors. If it fails, fix the root cause — do not skip or suppress.

## Step 3 — start the preview server

```bash
npm run preview
```

## Step 4 — check for runtime console errors with a headless browser

```bash
npx puppeteer browsers install chrome 2>/dev/null; node -e "
const p = require('puppeteer');
(async () => {
  const browser = await p.launch({ args: ['--no-sandbox'] });
  const pages = [
    '/',
    '/tools/image-compress/',
    '/tools/image-resize/',
    '/tools/image-convert/',
    '/tools/ico-generator/',
    '/tools/pdf-compress/',
    '/tools/file-diff/',
    '/tools/font-converter/',
    '/tools/font-inspector/',
    '/tools/font-pairs/',
    '/tools/wcag-contrast/',
    '/tools/color-palette/',
    '/tools/color-gradient/',
    '/tools/tints-shades/',
    '/tools/code-formatter/',
    '/tools/code-minifier/',
    '/tools/scss-compiler/',
    '/tools/semantic-html/',
    '/tools/brand-assets/',
    '/tools/xd-to-figma/',
  ];
  let clean = true;
  for (const url of pages) {
    const pg = await browser.newPage();
    const errors = [];
    pg.on('console', m => { if (m.type() === 'error') errors.push(m.text()); });
    pg.on('pageerror', e => errors.push(e.message));
    await pg.goto('http://localhost:3000' + url, { waitUntil: 'networkidle0', timeout: 15000 });
    if (errors.length) {
      console.error('ERRORS on ' + url + ':');
      errors.forEach(e => console.error('  ' + e));
      clean = false;
    } else {
      console.log('OK: ' + url);
    }
    await pg.close();
  }
  await browser.close();
  process.exit(clean ? 0 : 1);
})();
"
```

If puppeteer is not available, fall back to checking each page manually in a browser and noting any red entries in the Console tab.

## Step 5 — fix every error found

For each error:
1. Identify the source file and line from the stack trace.
2. Fix the root cause — do not silence errors with `try/catch` unless the error is genuinely recoverable.
3. Re-run steps 1–4 after fixing to confirm clean.

## Pass criteria

All 20 pages (home + 19 tool pages) must load with zero console errors and zero uncaught exceptions before continuing to the next action.
