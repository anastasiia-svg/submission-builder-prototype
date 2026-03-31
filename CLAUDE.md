# Submission Builder Prototype — Context for Claude

## What this is
A single-file HTML prototype for ResiQuant's Submission Builder tool.
Built for Anastasiia (UX designer/researcher at ResiQuant) to demo to founders and test with underwriters.

## The file
`/Users/anastasiia/submission-builder-prototype/index.html`
Single file — all HTML, CSS, and JavaScript in one place. ~6000+ lines.

## Live URL
https://anastasiia-svg.github.io/submission-builder-prototype/
GitHub repo: https://github.com/anastasiia-svg/submission-builder-prototype

## Tab structure (6 tabs, index 0–5)
- **Tab 0** — Submission Information (form, non-grid)
- **Tab 1** — Address & Geolocation (grid: address, lat/lng, geo accuracy, map column)
- **Tab 2** — Values (grid: building limit, hard/soft cost, TIV, etc.)
- **Tab 3** — Primary Risk Factors (grid: ATC construction, occupancy, stories, year built)
- **Tab 4** — Secondary Risk Factors (grid: EQSL, flood, anchoring, cladding, etc.)
- **Tab 5** — Hazard Metrics (placeholder, non-grid)

## Layout
- Left sidebar nav (170px, collapsible to 48px) — ResiQuant logo, Dashboard, Notifications, Inboxes, APIs, Settings, Admin. **No stats** — sidebar is navigation only.
- Top bar: breadcrumbs + undo/redo/changelog/save
- Tabs bar below topbar
- Content area: left panel (source doc viewer) | resizer | right panel (extracted data grid)
- Ribbon toolbar above grid (Excel-style, collapsible groups)

## Toolbar groups (left → right)
1. **Text** — Paste, Copy, Cut, Clip, Wrap
2. **Rows** — Row height, Text size
3. **View** — Columns (with hidden count badge), Valid filter (Valid / Needs Attention only — no "All Rows"), Auto-organize
4. **Alerts** — Alerts button (opens warning panel, orange badge), Removed button (toggle to show/hide dead rows, pink badge)
5. **Table** — Totals, Add row

## Key systems

### Warning / Alert system
- `TAB_WARNINGS` — per-tab cell warnings with `sources[]` arrays (SOV value, ACORD value)
- `WARNING_TYPE_META` — 10 warning types with colors
- Orange background `#fff7ed` on warning cells + orange triangle top-right
- Alerts slide panel: groups collapsed by default, per-item resolve buttons, bulk "Use all SOV/ACORD" buttons
- Alert badge shows warning count only (not excluded rows)

### Dead row / Removed row system
- `DEAD_ROWS[]` — rows excluded from submission (duplicate, vacant lot, below threshold, etc.)
- **Removed button** in toolbar = toggle: click shows dead rows in table (sets `_rowFilter=null`), click again hides them
- Dead rows always render at 22px height (not user-resizable height)
- Dead rows have hatched background pattern + pink "EXCLUDED" badge

### View filter
- `_viewMode`: `'valid'` (default) or `'attention'`
- Valid = hides dead rows, shows all live rows including warnings
- Attention = shows only rows with issues
- **"All Rows" removed** — use the Removed button in toolbar instead

### Geolocation (tab 1)
- Map column cells with issues use `warn-cell` class → orange bg + orange triangle (same as other warnings)
- Pin icon is 16px
- `GEO_ISSUES` = rows 9, 12, 15 (high priority)

### Issues bar (above grid)
- Shows "17 properties · 3 excluded" with CSS class `.issues-bar`
- Updates on every tab switch and cell edit

## Data
- 17 mock properties (ADDRS array), Morehart Land Co. SOV
- 3 dead rows (duplicate, vacant lot, below threshold)
- 21 total warnings across all tabs

## Design system
- Font: Inter (UI), Calibri (grid cells)
- Accent green: `#107c41`
- Warning orange: `#f59e0b` / `#b45309`
- Borders: `#e6e4e2`
- Ribbon bg: `#fafafa`
- Warning cell bg: `#fff7ed`
- Save button: dark `#202020`, no icon

## Next task (pending)
Implement the sidebar from Figma designs. The user shared these Figma URLs:
- https://www.figma.com/design/1tpI2cXvxejSTkTPliAdLU/Inbox-view?node-id=14203-313058
- https://www.figma.com/design/1tpI2cXvxejSTkTPliAdLU/Inbox-view?node-id=14205-616527
- https://www.figma.com/design/1tpI2cXvxejSTkTPliAdLU/Inbox-view?node-id=14206-625699
- https://www.figma.com/design/1tpI2cXvxejSTkTPliAdLU/Inbox-view?node-id=14209-289547

The Figma MCP plugin (Desktop Bridge) needs to be open in Figma for it to work.
The plugin connects to ws://localhost:9223 but was hitting a port conflict (9225 from another session).
**To fix**: close other Claude Code terminal windows, reopen the Desktop Bridge plugin in Figma.

## How to push changes to live URL
```bash
cd /Users/anastasiia/submission-builder-prototype
git add index.html
git commit -m "your message"
git push origin main
```
GitHub Pages auto-deploys in ~1 minute.
