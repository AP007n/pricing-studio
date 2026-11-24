# Pricing Studio

Live: https://pricing.ilol.workers.dev  (Cloudflare Worker: `pricing`)

## Folder
```
pricing-site/
├── public/              <- everything the website serves
│   ├── index.html       <- the app (all code in one file)
│   ├── favicon.svg / favicon.ico / favicon-32.png
│   ├── apple-touch-icon.png, icon-192.png, icon-512.png
│   ├── site.webmanifest <- "Add to Home Screen" support
│   └── _headers         <- no-cache on index.html so updates show at once
├── wrangler.jsonc       <- Cloudflare config (Worker name + assets folder)
└── README.md
```

## Push a new version

### Option A: Cloudflare dashboard (no tools needed)
1. Workers & Pages > `pricing` > Deployments > upload a new version.
2. Drag in the **contents of `public/`** (index.html must be at the top level, not inside a folder).
3. Deploy, then open the live link and hard-refresh (Cmd+Shift+R / Ctrl+Shift+R).

### Option B: Terminal (fastest for repeat updates)
```bash
cd pricing-site
npx wrangler login      # first time only, opens the browser
npx wrangler deploy     # uploads ./public to the "pricing" Worker
```

### Option C: Auto-deploy from GitHub
Put this folder in a GitHub repo, then in Cloudflare: `pricing` > Settings > Builds > Connect repository.
Every push to `main` deploys automatically (build command: none, deploy command: `npx wrangler deploy`).

## What the app does
- **Overview**: price card, revenue, take-home, hours used, 12-month cash flow, where revenue goes, health check.
- **Price a job**: prices one job from your rate, costs, market and client value.
- **Quote**: business and client details with a live preview. Client quote or internal report as PDF.
- **Business**: expenses, job mix, profit and loss, break-even, capacity and cash.
- **Settings**: backup file (download/load), run setup again, erase data.
- First visit opens a 4-step setup. No accounts: data stays in the visitor's browser.
- **Excel** export: Summary, Expenses, Job mix, Profit and loss, Cash 12 months, Current job.

## Editing
- Job presets (websites, apps, marketing, etc.): search `const JOBS` in index.html.
- Default expenses and job mix: search `expenses: [` and `mix: {`.
- Default hourly rate (62 BHD): search `DEFAULTS.hourRate`.
- Colours: the `:root` block at the top of the `<style>` tag.

## Version and contact
Search `const APP =` in index.html to change the version, date and the contact line shown in Settings.
