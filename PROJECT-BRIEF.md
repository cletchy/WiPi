# WiFi Occupancy Report — Project Brief

## Purpose

This project is a self-contained browser-based tool that reads WiFi client data exported from a router management system and produces a room occupancy report. It is designed for use at TAFE NSW campuses and must run on locked-down Windows machines with no software installation — colleagues open the single HTML file in Chrome or Edge.

## Deliverable

**File:** `occupancy-report.html`
A single self-contained HTML file. No server, no install, no dependencies beyond a browser. Libraries loaded from CDN (Chart.js 4.4.1, PapaParse 5.4.1).

## Data Format

Exports come from the router management system (one CSV file per access point). Each file follows this naming convention:

```
Active Clients trend {AP_ID} during {date range}.csv
```

**Example:**
```
Active Clients trend NE00P002010AP01 during 2026-04-28 12.00.00 AM to 2026-06-12 10.00.00 PM.csv
```

CSV structure (two columns, hourly intervals):

```
Time,Active Clients
"May 1, 2026 10:00:00 AM",5
"May 1, 2026 11:00:00 AM",3
```

The AP ID is extracted automatically from the filename using regex.

## Key Features

### Upload
- Drag-and-drop or file picker; accepts multiple CSV files simultaneously
- AP ID extracted from filename automatically
- Files can be removed individually

### Room Configuration
- Each AP maps to a room name and capacity (people)
- Configuration persists in browser localStorage — colleagues only set it up once
- Room names and capacities are editable in a table before generating the report

### Filters (applied globally)
- Date range (from / to)
- Hour range (default: 7 am – 7 pm)
- Day type (all days / weekdays only / weekends only)
- Room selector (all rooms or a single room)

### Summary (KPI section)
Two toggle modes:
- **Total** — single row of 5 cards: Peak simultaneous clients, Average (active hours), Peak utilisation %, Active days in range, Busiest day.
- **Per Room** — one full-width card per room, single horizontal row of 5 stats: Peak clients, Average (active hours), Peak utilisation %, Active days, Busiest day. Stats are computed per room, not combined.

### Occupancy Over Time
Single line chart showing all rooms as individual coloured lines plus a dashed dark grey "Combined total" line overlaid. No toggle — always shows per-room + combined.

### Peak Clients by Room
Grouped bar chart: peak clients vs. capacity per room.

### Occupancy by Hour of Day
Two independent toggles:
- **Metric:** Average (suitable for workshops/social spaces) or Peak (suitable for offices/classrooms)
- **Scope:** Combined (one bar, all rooms summed) or Per Room (grouped bars, one series per room)

### Heatmap
Day × hour average occupancy grid. Room selector dropdown: "All rooms (combined)" or any individual room. Colour scale from white → TAFE blue (darker = busier).

### Room Summary Table
One row per room: Room name, AP ID, Peak clients, Avg clients (active hours), Busiest day, Busiest hour, Capacity, Peak utilisation % (colour-coded pill: green <60%, yellow 60–89%, red ≥90%).

### Print / Export
Browser print → Save as PDF. Upload and config sections are hidden in print output.

## Design Decisions

### Colour palette
Wong (2011) colorblind-safe palette — distinguishable across deuteranopia, protanopia, and tritanopia:

| Index | Hex | Name |
|-------|-----|------|
| 0 | `#0072B2` | Blue |
| 1 | `#D55E00` | Vermillion |
| 2 | `#009E73` | Green |
| 3 | `#CC79A7` | Mauve/Pink |
| 4 | `#E69F00` | Orange |
| 5 | `#56B4E9` | Sky blue |
| 6 | `#F0E442` | Yellow |
| 7 | `#000000` | Black |

Brand colour (TAFE blue `#003087`) is used for headers, labels, and UI chrome — not for data series.

### Single-file distribution
The app is shared as an email attachment or via Teams/OneDrive. No hosting required.

### No backend
All data processing happens client-side in JavaScript. No data leaves the machine.

## Known Constraints

- Designed for up to ~8 access points (one colour per AP; wraps beyond 8)
- Heatmap shows averages only (not peak) — peak heatmap not yet implemented
- Hour range filter options are limited to preset values in dropdowns (not free-form time pickers)
- Currently no export to Excel/CSV from within the app

## Possible Next Steps

- Add TAFE NSW logo to the header
- Add a "week selector" shortcut to the date filter
- Peak heatmap option (alongside average)
- Export report data to CSV/Excel
- Support for more than 8 APs (extended colour cycling or legend pagination)
