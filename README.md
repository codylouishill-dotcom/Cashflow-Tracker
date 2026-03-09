# Cashflow Stacking — Calculator, Tracker & IBC

A single-page React app for modeling cashflow stacking strategies, tracking real-world flip performance against projections, and managing Infinite Banking Concept (IBC) whole life policies — all synced to Google Sheets.

![Version](https://img.shields.io/badge/version-5.0-06b6d4?style=flat-square)
![License](https://img.shields.io/badge/license-MIT-22c55e?style=flat-square)
![Stack](https://img.shields.io/badge/stack-React%20%2B%20Babel%20%2B%20Google%20Sheets-a78bfa?style=flat-square)

---

## What It Does

### Cashflow Stacking Calculator

Models how deploying capital into interest-bearing flips (notes, lending, etc.) compounds over time. The simulator tracks LOC balance paydown, flip stacking/growth, and projects a **"Freedom Date"** — the earliest month where asset cashflow sustains your desired withdrawal indefinitely without further contributions.

- Configurable yield, duration, LOC interest, growth multiplier, and flip sizing
- Automatic flip tier progression (deploys larger flips as LOC pays down)
- Side-by-side comparison against a traditional index fund 4% rule strategy
- 401(k) integration for combined retirement modeling

### Flip Tracker

Records real monthly cashflow against each flip's expected payments so you can see how actual performance diverges from projections.

- Split tracking (multiple sub-positions per flip)
- Index fund–backed flip support with FIFO capital gains tax modeling
- Per-flip expected vs. actual summaries with variance reporting
- Actuals automatically feed into the forward simulation

### IBC Policy Tracker

Tracks whole life insurance policies used in an Infinite Banking strategy, including cash value growth, policy loans, premium schedules, and borrowable equity.

- **PDF Parsing** — upload an Ameritas (or similar) policy illustration PDF and auto-extract policy details + projected CV/DB tables
- **Annual CV/DB Tracking** — pre-filled from illustration data, mark as actual after each annual statement
- **Monthly Loan Balances** — track policy loan positions month by month
- **Premium Alerts** — upcoming renewal notifications with days-away countdown
- **Projected Values** — full illustration table with CV/Premium efficiency ratios
- **Borrowable Equity** — ~90% CV minus policy loans, projected forward by year
- **Credit Line Management** — track HELOCs, personal LOCs, business LOCs with balances and utilization
- **DIBS Deals** — model Deal Into Banking Strategy flows (policy-to-policy capital movement with amortized repayment)
- **Gap Analysis** — 20-year forward projection of borrowable equity vs. committed capital from active flips and deals, with gap detection and deal capacity planning

### Cross-Module Integration

The cashflow and IBC sides talk to each other:

- **DIBS toggle on flips** — link a flip's source LOC to a destination IBC policy
- **Gap analysis pulls flip data** — active flips from the cashflow tracker appear as committed capital in the IBC borrowable equity forecast
- **Shared storage** — all data (cashflow settings, flips, actuals, IBC policies, LOCs, deals) saves to the same Google Sheet
- **Unified export/import** — single JSON backup covers everything

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| UI | React 18 via CDN + Babel standalone (no build step) |
| Styling | Inline styles, CSS variables, dark theme |
| Fonts | Bricolage Grotesque (display) + DM Mono (data) |
| Storage | Google Sheets API v4 (primary) + localStorage (fallback) |
| Auth | Google OAuth 2.0 implicit flow |
| PDF Parsing | PDF.js (client-side) |
| Charts | Hand-rolled SVG |

Everything runs client-side — no server, no build toolchain. Open the HTML file in a browser and go.

---

## Setup

### 1. Get the file

Download `cashflow-stacking-ibc-v5.html` and open it in any modern browser.

### 2. Google Sheets sync (optional)

To enable cross-device sync via Google Sheets:

1. **Create a Google Cloud project** at [console.cloud.google.com](https://console.cloud.google.com)
2. **Enable the Google Sheets API**
3. **Create an OAuth 2.0 Client ID** (Web application type)
   - Add your domain to "Authorized JavaScript origins" (e.g. `http://localhost` for local dev, or your hosted URL)
4. **Create a Google Sheet** to use as your data store
5. **Update the config** at the top of the HTML file:

```javascript
const SHEETS_CONFIG = {
  CLIENT_ID: 'your-client-id.apps.googleusercontent.com',
  SHEET_ID: 'your-google-sheet-id',
  SCOPES: 'https://www.googleapis.com/auth/spreadsheets',
  DISCOVERY_DOC: 'https://sheets.googleapis.com/$discovery/rest?version=v4',
};
```

The app creates these tabs automatically in your sheet:

| Tab | Contents |
|-----|----------|
| `Settings` | All calculator parameters (key-value pairs) |
| `Flips` | Tracked flips with splits and DIBS metadata |
| `Actuals` | Monthly personal cash, withdrawals, notes |
| `IBC_Policies` | Full policy objects (JSON per row) |
| `IBC_LOCs` | Credit line objects |
| `IBC_Deals` | Deal/DIBS objects |

Without Google auth, everything saves to localStorage and works fine on a single device.

---

## Navigation

| Tab | What's there |
|-----|-------------|
| **◇ Dashboard** | Freedom date, cashflow trajectory chart, sparklines, IBC summary card |
| **📊 Actuals** | Record monthly personal cash contributions and withdrawals |
| **🔄 Flips** | Add/edit flips with splits, DIBS toggle, monthly CF tracking |
| **▤ Detail** | Full month-by-month table (actuals + forecast) with CSV export |
| **🛡️ IBC** | Sub-tabs: Dashboard, Policies, Credit Lines, Projections, Borrowable, Gap Analysis |
| **⚙ Settings** | All parameters, Google Sheets connection, export/import, reset |

---

## IBC Sub-Tabs

| Sub-Tab | Purpose |
|---------|---------|
| **Dashboard** | Premium alerts, summary cards (CV, loans, net CV, death benefit, borrowable), policy table |
| **Policies** | Expandable policy cards with annual tracking, monthly loan entry, forecast vs actual status |
| **Credit Lines** | HELOC/LOC table with inline balance editing, utilization metrics |
| **Projections** | Illustration data table from parsed PDF, guaranteed vs non-guaranteed toggle |
| **Borrowable** | Projected borrowable equity by year across all policies + LOCs |
| **Gap Analysis** | 20-year LOC forecast, deal tracking, flip capital overlay, gap detection, capacity planner |

---

## Key Concepts

**Freedom Date** — The earliest month where you could stop contributing personal cash, begin withdrawing your desired monthly amount, and the system's flip cashflow sustains itself indefinitely (tested by running 50 years forward).

**Flip Stacking** — Each time your LOC pays down to zero, a new flip deploys. After N flips at the same size, the flip amount grows by the configured multiplier (up to a max). This creates compounding cashflow layers.

**DIBS (Deal Into Banking Strategy)** — Borrow from one source (policy loan or HELOC) to fund a premium or loan payoff on another policy. The destination policy's growth then funds repayment back to the source via an amortized schedule.

**Gap Analysis** — Projects your total borrowable equity (IBC + LOCs) forward using illustration data, overlays committed capital from active flips and deals, and flags any future months where commitments exceed capacity.

---

## Data & Privacy

- All computation happens in your browser
- No data leaves your machine unless you opt into Google Sheets sync
- Google Sheets sync uses your own sheet in your own Google account
- No analytics, tracking, or third-party services beyond Google Fonts, CDN libraries, and (optionally) Google Sheets API
- Export creates a portable JSON blob you can back up anywhere

---

## Development

There's no build step. To modify:

1. Open the HTML file in your editor
2. Edit the JSX inside the `<script type="text/babel">` block
3. Refresh the browser

The entire app is a single file. Babel transpiles JSX in the browser at runtime.

---

## Disclaimer

This tool is for personal modeling and tracking purposes only. It is **not financial advice**. Consult qualified professionals before making investment, insurance, or borrowing decisions. Projected values from insurance illustrations are not guaranteed. Past performance of any strategy does not guarantee future results.

---

## License

MIT
