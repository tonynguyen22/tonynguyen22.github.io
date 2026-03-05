# Tonivest Research — CLAUDE.md

## Project Overview
Personal investment research website hosted at **tonynguyen22.github.io**.
Plain HTML + CSS only — no frameworks, no build step, no JavaScript libraries.
All pages link to `styles.css` (single global stylesheet).

---

## Site Structure

```
/
├── index.html          # Homepage — analysis card grid
├── port.html           # Portfolio tracker
├── about.html          # About Me page
├── tools.html          # Tools & Projects page
├── styles.css          # Single global stylesheet (do not create additional CSS files)
├── assets/             # Logos, PDFs
│   ├── logo-nav.png
│   ├── logo.png
│   └── *.pdf
├── stocks/             # One HTML file per stock research report
│   ├── adbe.html
│   ├── gxo.html
│   ├── syna.html
│   ├── sas.html
│   └── pvt.html
├── excel/              # Source financial models (Excel, read-only reference)
├── stock-note/         # Raw research notes (.txt) per ticker
└── data/               # JSON data files (trades, portfolio)
```

---

## Design System

| Token | Value |
|-------|-------|
| Primary dark navy | `#0f1d2f` / `#1E3A5F` |
| Gradient dark | `#182f4d` → `#07223c` |
| Silver accent | `#A9B4C2` |
| Positive green | `#6ee7b7` / `#059669` |
| Negative red | `#fca5a5` / `#dc2626` |
| Neutral amber | `#fcd34d` / `#d97706` |
| Body font | `system-ui` / `Segoe UI` |

- All colors and layout classes live in `styles.css` — **never use inline styles for colors or layout** except minor one-off adjustments.
- No emojis in code or content unless explicitly requested.

---

## Required Structure for Every `stocks/*.html` Page

Every stock research page **must** include all of the following, in this order:

### 1. `<head>` block
```html
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<link rel="icon" href="../assets/logo-nav.png" type="image/png">
<link rel="stylesheet" href="../styles.css">
```
- Paths use `../` (one level up from `stocks/`)
- No additional `<link>` stylesheet tags — all styles go in `styles.css`

### 2. Sidebar — REQUIRED, all 4 nav items
The sidebar must always contain **all four nav items** plus the **sidebar toggle button**:
```html
<aside class="sidebar">
    <div class="sidebar-logo">
        <span class="logo-text">Tonivest Research</span>
        <button id="sidebar-toggle" class="sidebar-toggle" aria-label="Toggle Sidebar">
            <svg width="20" height="20" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16"/>
            </svg>
        </button>
    </div>
    <nav class="sidebar-nav">
        <a href="../index.html" class="nav-item active"> ... Company Analysis</a>
        <a href="../port.html" class="nav-item"> ... My Portfolio</a>
        <a href="../tools.html" class="nav-item"> ... Tools &amp; Projects</a>
        <a href="../about.html" class="nav-item"> ... About Me</a>
    </nav>
</aside>
```

### 3. Page header
```html
<header class="analysis-detail-header">
    <div class="header-content">
        <h1 class="detail-title">Equity Research: [Company Name] ([TICKER])</h1>
        <p class="detail-meta">Initiated: [Month Year] &nbsp;|&nbsp; Last Updated: [Date] &nbsp;|&nbsp; Author: Tony Nguyen</p>
    </div>
    <a href="../index.html" class="back-button">← Back</a>
</header>
```

### 4. Content sections (13-section template)
Full research pages follow this numbered structure inside `<article class="detail-content">`:

| # | Section | Key Component |
|---|---------|---------------|
| 01 | Overview Card | `.stock-overview-card` with ticker, verdict badge, thesis, quick stats |
| 02 | Recent News & Earnings | `.update-timeline` / `.update-card` |
| 03 | Business Summary | `<h4>` sub-segments, prose |
| 04 | Industry Context | Prose |
| 05 | Competitive Advantages / Moat | `<h4>` numbered points |
| 06 | Revenue Growth & Profitability | `.metric-pair-grid` + historical table |
| 07 | Earnings Per Share | `.metric-pair-grid` + EPS table |
| 08 | Balance Sheet | `.metric-pair-grid` |
| 09 | 3-Year Forecast | `.scenario-comparison-grid` (Bear/Base/Bull) + `.forecast-table` |
| 10 | Valuation | `.metric-pair-grid` with DCF summary |
| 11 | Sensitivity Analysis | `.sensitivity-wrap` + `.sensitivity-table` (WACC × TGR matrix) |
| 12 | Key Risks | `<h4>` numbered risks |
| 13 | Final Verdict | `.final-verdict-box` |

Section headers always use `.section-label` above `<h3>`:
```html
<span class="section-label">06 — Revenue Growth &amp; Profitability</span>
<h3>Revenue Growth &amp; Profitability</h3>
```

### 5. Sidebar toggle script — REQUIRED at end of `<body>`
```html
<script>
    document.addEventListener('DOMContentLoaded', () => {
        const sidebar = document.querySelector('.sidebar');
        const toggleBtn = document.getElementById('sidebar-toggle');
        if (localStorage.getItem('sidebar-collapsed') === 'true') {
            sidebar.classList.add('collapsed');
        }
        toggleBtn.addEventListener('click', () => {
            sidebar.classList.toggle('collapsed');
            localStorage.setItem('sidebar-collapsed', sidebar.classList.contains('collapsed'));
        });
    });
</script>
```

---

## index.html — Card Requirements

Every stock page must have a corresponding card in `index.html`. Each card requires:
- `.card-rating-badge` with `crb-buy` / `crb-hold` / `crb-sell` class
- `.stock-symbol` with the ticker
- A 1–2 sentence description with key thesis and target price
- Link to `stocks/[ticker].html` (lowercase ticker)
- "View Report" button text (not "View Details")

---

## Key CSS Classes Reference

### Stock Overview Card
`.stock-overview-card`, `.stock-ticker`, `.stock-ticker-row`, `.stock-company-name`, `.info-tags`, `.info-tag`, `.stock-thesis`, `.verdict-badge`, `.verdict-buy`, `.verdict-hold`, `.verdict-sell`, `.quick-stats-grid`, `.quick-stat`, `.quick-stat-value`, `.quick-stat-label`, `.qs-positive`, `.qs-negative`, `.qs-neutral`

### Earnings Timeline
`.update-timeline`, `.update-card`, `.latest-update`, `.update-card-header`, `.update-card-title`, `.update-card-meta`, `.update-date-pill`, `.rating-pill`, `.rating-buy`, `.rating-hold`, `.rating-sell`, `.update-upside-row`, `.upside-value.positive/.negative/.neutral`

### Metric Grid
`.metric-pair-grid`, `.metric-pair`, `.mp-label`, `.mp-value`, `.mp-context`

### Scenario Comparison (Bull/Base/Bear)
`.scenario-comparison-grid`, `.scenario-col.bull/.base/.bear`, `.scenario-col-label`, `.scenario-col-pt`, `.scenario-col-upside`, `.scenario-col-rows`, `.scenario-col-row`, `.scenario-col-row-label`, `.scenario-col-row-val`

### Forecast & Sensitivity Tables
`.forecast-table`, `.ft-section` (section header row), `.sensitivity-wrap`, `.sensitivity-table`, `.st-corner`, `.st-row-head`, `.st-highlight` (base case cell)

### Final Verdict
`.final-verdict-box`, `.fvb-inner`, `.fvb-rating-label`, `.fvb-rating`, `.fvb-stats-row`, `.fvb-stat-label`, `.fvb-stat-value`, `.fvb-positive`, `.fvb-negative`, `.fvb-neutral`, `.fvb-reasoning`

### Layout
`.analysis-detail-container` (max-width 900px white card), `.detail-section` (2.5rem bottom margin), `.section-label` (silver uppercase numbering), `.detail-section h3` (silver bottom border)

---

## Workflow for New Stock Pages

1. Place any financial model in `excel/[TICKER].xlsx` or `excel/[TICKER] - DCF.xlsx`
2. Place research notes in `stock-note/[ticker].txt`
3. Extract financial data from Excel using Python + openpyxl (`py -c "import openpyxl..."`)
4. Create `stocks/[ticker].html` following the 13-section template above
5. Add the card to `index.html` with correct rating badge and link
6. Append any new CSS classes to the **end** of `styles.css` only

---

## DO NOT

- Do not create separate CSS files — all styles go in `styles.css`
- Do not use JavaScript frameworks or external libraries
- Do not add emojis unless explicitly asked
- Do not omit any of the 4 sidebar nav items or the toggle button/script on any stock page
- Do not use `stocks/adobe.html` — the correct path is `stocks/adbe.html`
