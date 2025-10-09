Cursor Metrics Leaderboard

Overview
- Single static page (`index.html`) to visualize Cursor usage exports from the Cursor dashboard.
- Line chart: tokens/cost over time (Daily/Weekly), with Top Users filter and auto-trim of early zero-only dates.
- Right panel: toggle between Model Usage donut and Top Users bar chart for the active window/metric.

Run locally
Browsers block `file://` fetches, so serve the directory:

Python (preinstalled on macOS)
```bash
python3 -m http.server 5173
```
Open http://localhost:5173

Node (if installed)
```bash
npx http-server -p 5173 . --silent
```
Open http://localhost:5173

Data files
- Put CSV exports under `stats/`.
- The page currently loads two files, hard-coded in `index.html`:
  ```js
  const CSV_PATHS = [
    'stats/jan-jun.csv',
    'stats/jul-sep.csv',
  ];
  ```
- To use different files, replace or add to `CSV_PATHS` with your filenames. A static page cannot list a directory, so filenames must be specified here.
- `.gitignore` ignores `stats/*.csv` so your exports won’t be committed.

Expected CSV format
- Header columns (case-sensitive):
  - `Date` (ISO datetime, e.g. `2025-06-30T21:24:32.214Z`). The page truncates to date-only in UTC.
  - `User` (email); UI displays only the part before `@`.
  - `Model` (string)
  - `Total Tokens` (number)
  - `Cost` (number)
- Non-numeric or empty token/cost values are treated as 0.
- Weekly aggregation uses Monday as start-of-week (UTC). Adjust in `getWeekStartUtc()` if needed.

Controls & interactions
- Tokens / Cost: switches metric for all charts.
- Daily / Weekly: time aggregation.
- Top Users: limits the number of user series in the line chart.
- Date menu: `Current month`, `Last month`, `Last 3 months`, or `Range`.
  - For presets, the `From`/`To` inputs are hidden and auto-selected.
  - `Range` shows `From`/`To` date pickers.
- Auto range: reverts the line chart to auto-start at the earliest non-zero bucket among visible series.
- Legend toggles: click a user in the line chart legend to show/hide and re-compute the visual start date.

Customization notes
- Change start-of-week: edit `getWeekStartUtc()`.
- Change the initial earliest allowed date: edit `MIN_DATE`.
- To show more models/users in the right panel, adjust the slice/limit where noted in code.



