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
- **Business**: expenses, job mix, team cost, profit and loss, break-even, capacity and cash.
- **Forecast** (v3, for SMEs): 12-month profit and loss in the GCC forecast-sheet layout (Income, COGS, Gross profit, Expenses with Staff cost lines, Net ordinary income, Grants, Net income). Team cost is worked out per country: SIO (BH), GOSI (SA), GPSSA (AE), SPF (OM), PIFSS (KW), GRSIA (QA), plus end of service, work permits, medical and housing. Excel export with live formulas (Forecast + Team & rates).
  - v3.1: GCC seasonality (Ramadan, summer, December), scenarios (worst/base/best), direct-method cash flow (payment delays, VAT quarters, yearly bills, end-of-service provision), one-off items (expenses, assets, loans), tax on profit by country, 3-year plan with a goal.
  - v3.2: pipeline deals weighted by chance (one-off or monthly), actual vs forecast tracking with a year-end outlook, payment month for each yearly bill. Workbook adds Pipeline and Actual vs forecast sheets.
  - v3.3: import actuals from Excel or CSV (QuickBooks, Xero, Zoho Books, Odoo, Tally, bank statements, the GCC forecast sheet or a simple template). Detects the layout, lets you remap rows or columns, previews and checks against the file's net income. Runs fully in the browser.
  - v3.4 (GCC): working calendar per country (weekends, national days, Islamic holidays from the Umm al-Qura calendar) driving working days and seasonality; labour notes (leave, Ramadan hours); nationalisation checks (Emiratisation rules and fines, Nitaqat, Bahrainisation, Omanisation, Kuwaitisation, Qatarisation); management-account categories with variance; import by account lines with English/Arabic account names, Hijri dates and a net-income reconciliation; Hijri date on Saudi quotes.
  - v3.5: seven service industries (tech, consulting, maintenance and facility, marketing and creative, events, training, health and wellness) with jobs, hourly rates, job mixes and scope wording; scope of work on quotes (included, not included, client provides, acceptance, warranty); quote revision with ISO 8601 dates; simpler Forecast with four views (Plan, Monthly table, Actual vs forecast, 3 years). Changing currency converts every amount (GCC pegs fixed; KWD, EUR, GBP at Nov 2026 rates: `const FX`).
- **Settings**: backup file (download/load), run setup again, erase data.
- First visit opens a 4-step setup. No accounts: data stays in the visitor's browser.
- **Excel** export: Summary, Expenses, Job mix, Profit and loss, Cash 12 months, Current job. On the Forecast view the Excel button downloads the 12-month forecast workbook.

## Editing
- Job presets: tech jobs in `const JOBS`; other industries, rates, mixes and scope wording in `const INDUSTRIES` and `SCOPE_IND`.
- Default expenses and job mix: search `expenses: [` and `mix: {`.
- Default hourly rate (62 BHD): search `DEFAULTS.hourRate`.
- Country employer rules (rates checked November 2026): search `const PAYROLL`. Sample team: `SAMPLE_TEAM`.
- Colours: the `:root` block at the top of the `<style>` tag.

## Version and contact
Search `const APP =` in index.html to change the version, date and the contact line shown in Settings.
