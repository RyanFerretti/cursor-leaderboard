# Cursor Metrics Leaderboard

A single-page dashboard to visualize Cursor usage metrics exported from the Cursor dashboard.

---

## Features

- **Line Chart**: Track tokens/cost over time with Daily/Weekly aggregation
- **Top Users Filter**: Focus on the most active users
- **Smart Date Trimming**: Auto-removes early zero-only dates for cleaner visualization
- **Right Panel**: Toggle between Model Usage donut chart and Top Users bar chart
- **Interactive Legend**: Click users to show/hide series and re-compute date ranges

---

## Quick Start

Since browsers block `file://` fetches, you'll need to serve the directory locally:

### Option 1: Python (preinstalled on macOS)

```bash
python3 -m http.server 5173
```

Then open [http://localhost:5173](http://localhost:5173)

### Option 2: Node.js

```bash
npx http-server -p 5173 . --silent
```

Then open [http://localhost:5173](http://localhost:5173)

---

## Data Setup

### 1. Add Your CSV Files

Place your Cursor usage exports in the `stats/` directory.

> **Note**: `stats/*.csv` is gitignored by default, so your data won't be committed.

### 2. Configure File Paths

Update the `CSV_PATHS` array in `index.html`:

```js
const CSV_PATHS = [
  'stats/jan-jun.csv',
  'stats/jul-sep.csv',
];
```

Add or replace filenames as needed. Since this is a static page, file paths must be explicitly specified.

### 3. CSV Format Requirements

Your CSV must include these columns (case-sensitive):

| Column | Type | Description | Example |
|--------|------|-------------|---------|
| `Date` | ISO datetime | Timestamp of the usage event | `2025-06-30T21:24:32.214Z` |
| `User` | string | Email address (only part before `@` is shown in UI) | `user@example.com` |
| `Model` | string | Model identifier | `claude-3-5-sonnet` |
| `Total Tokens` | number | Token count | `1234` |
| `Cost` | number | USD cost | `0.05` |

**Notes**:
- Non-numeric or empty token/cost values are treated as `0`
- Dates are truncated to date-only in UTC
- Weekly aggregation uses Monday as start-of-week (UTC)

---

## Controls & Interactions

| Control | Description |
|---------|-------------|
| **Tokens / Cost** | Switch the metric for all charts |
| **Daily / Weekly** | Change time aggregation granularity |
| **Top Users** | Limit the number of user series in the line chart |
| **Date Menu** | Choose `Current month`, `Last month`, `Last 3 months`, or `Range` |
| **Auto Range** | Reset line chart to auto-start at earliest non-zero bucket |
| **Legend Toggles** | Click users to show/hide series and re-compute visual start date |

---

## Customization

### Change Start of Week

Edit the `getWeekStartUtc()` function in `index.html`.

### Change Minimum Date

Modify the `MIN_DATE` constant in `index.html`.

### Show More Models/Users

Adjust the slice/limit in the right panel rendering code.

---

## Example

An `example.csv` is included in `stats/` with sample data. The page loads this by default, so you can explore the dashboard immediately after starting the local server.



