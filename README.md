# WiFi Occupancy Report — User Guide

## What it does

`occupancy-report (index.html)` is a self-contained browser tool that turns CSV exports from your WiFi management system into an occupancy dashboard. No installation, no server — just open the file and upload your data.

---

## Getting started

1. **Open `occupancy-report.html`** in any modern browser (Chrome, Edge, Firefox, Safari).
2. **Upload your CSV files** — one file per access point. Drag and drop onto the upload zone, or click **Browse files**.
3. **Configure rooms** — assign a friendly name (e.g. "Room 101") and a capacity (number of people) to each access point. These settings are remembered in your browser for next time.
4. Click **Apply & Generate Report** to build the dashboard.

---

## CSV file format

Each file must be a CSV export from your router or WiFi management platform with at minimum:

| Column | Description |
|---|---|
| A column containing **"time"** in the header | Timestamp of each reading |
| A column containing **"client"** in the header | Number of connected clients at that time |

The tool is flexible about exact column names — it searches for headers that contain the words "time" and "client" (case-insensitive), so exports from most systems will work without modification.

**File naming matters.** The tool extracts the access point ID from the filename. It looks for patterns like:

- `NE00P002010AP01 Active Clients trend during ...csv` → AP ID: `NE00P002010AP01`
- `AP-01_data.csv` → AP ID: `AP-01`
- `AP_Room3.csv` → AP ID: `AP_ROOM3`

If the name doesn't match a recognisable pattern, the filename (minus extension) is used as the ID, truncated to 20 characters.

---

## File upload constraints

| Constraint | Detail |
|---|---|
| **File type** | `.csv` only. Other formats are rejected. |
| **Number of files** | No hard limit. Upload as many access points as you have. Performance may slow with very large datasets (many files × many rows). |
| **File size** | No enforced limit, but very large files (tens of thousands of rows) will take longer to parse in the browser. |
| **One file per access point** | Each file is treated as a single access point. If you have multiple files for the same AP, upload them separately — they will appear as distinct entries in the configuration table. |
| **Adding files later** | You can upload additional files at any time. The new access points are merged into the existing report. |
| **Removing files** | Click the **✕** next to any file in the file list to remove it from the report. |

---

## Filters

Once data is loaded, use the **Filters** panel to narrow the report:

- **Date range** — restrict to a specific period.
- **Hours** — defaults to 7 AM – 7 PM. Adjust to match your operating hours.
- **Days** — All days, Weekdays only, or Weekends only.
- **Room** — view a single room or all rooms combined.

---

## Room configuration

The configuration table appears after upload. For each access point:

- **Room / Space Name** — a human-readable label shown throughout the report.
- **Capacity** — used to calculate peak utilisation (%) in the summary table.

Settings are saved automatically in your browser's local storage, so they persist between sessions on the same computer.

---

## Report sections

| Section | Description |
|---|---|
| **Summary KPIs** | Peak clients, average occupancy, busiest day and hour — for all rooms combined or per room. |
| **Occupancy Over Time** | Line chart showing client counts across the date range, per room plus combined total. |
| **Peak Clients by Room** | Bar chart comparing peak demand across spaces. |
| **Average Occupancy by Hour** | Shows typical demand at each hour of day. Toggle between Average and Peak, and between Combined and Per Room views. |
| **Heatmap** | Day-of-week × hour grid. Darker = busier. Filter by room using the dropdown. |
| **Room Summary Table** | Per-room breakdown: peak clients, average clients, busiest day/hour, capacity, and peak utilisation %. |

---

## Printing and PDF export

Click **🖨 Print / Save PDF** in the Filters panel. Upload controls and configuration panels are hidden in print view; only the report content prints. Use your browser's "Save as PDF" option to export.

---

## Privacy and data handling

All processing happens locally in your browser. No data is uploaded to any server. Closing the browser tab clears all loaded data; only room names and capacities are persisted (in browser local storage).

---

## Troubleshooting

| Issue | Solution |
|---|---|
| "Please select CSV files" alert | The file must end in `.csv`. Rename if needed. |
| Report shows no data after upload | Check that your CSV has columns containing "time" and "client" in the header names. |
| Access point ID looks garbled | Rename the file to include a clear AP identifier, e.g. `AP-01_export.csv`. |
| Room names reset after closing | Room config is stored per browser/device. If using a different browser or private/incognito mode, settings will not carry over. |
| Charts are slow or blank | Very large CSV files may take several seconds to parse. Wait for the page to settle before interacting. |
