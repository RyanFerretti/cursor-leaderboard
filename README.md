Cursor Metrics Leaderboard

What this is
- A single static page (`index.html`) that visualizes Cursor usage exported to CSV.
- Shows a line chart of tokens/cost over time (daily/weekly) by top users.
- Shows a doughnut chart of model usage share by tokens or cost.

Data
- CSVs live in `stats/jan-jun.csv` and `stats/jul-sep.csv` (Jan–Sep 2025).
- Expected columns: `Date`, `User`, `Model`, `Total Tokens`, `Cost`.
- Rows with missing or non-numeric values for tokens/cost are treated as 0.
- Dates are down-leveled to date-only (UTC); weekly buckets start Monday (UTC).

Run locally
Because browsers block `file://` fetches, serve the directory via a simple server:

Option A: Python (preinstalled on macOS)
```bash
python3 -m http.server 5173
```
Open `http://localhost:5173`.

Option B: Node (if installed)
```bash
npx http-server -p 5173 . --silent
```
Open `http://localhost:5173`.

Controls
- Metric: toggle Tokens vs Cost for the line chart.
- Granularity: toggle Daily vs Weekly aggregation.
- Top Users: limit series to the top N by total tokens (All = no limit).
- Pie Metric: choose Tokens or Cost for model share.

Notes
- Styling is intentionally minimal and modern; adjust in `<style>` if desired.
- To add more data, place additional CSVs in `stats/` and add to `CSV_PATHS` in `index.html`.
- If you prefer a different start-of-week, change `getWeekStartUtc()` in `index.html`.


