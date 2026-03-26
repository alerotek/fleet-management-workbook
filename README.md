# 🚗 Fleet Management Workbook — Automated Monthly Reporting

A fully automated Excel/Google Sheets workbook for managing field fleet performance, built for operations teams running mixed vehicle fleets (cars + motorcycles) across multiple regions.

Paste three raw data exports each month. Everything else — KPIs, compliance flags, discipline tracking, and regional dashboards — calculates automatically.

---

## 📋 What It Does

| Feature | Detail |
|---|---|
| **Auto KPI calculation** | Distance, fuel efficiency, compliance % per vehicle and region |
| **Speed violation flags** | Every trip exceeding 90 km/h pulled from raw GPS data — full audit trail |
| **Late-night driving flags** | Separate severity tiers for past 10pm and past midnight |
| **Permission management** | Dropdown to mark approved late-night trips — removes them from the action queue |
| **Discipline tracker** | Cumulative log across all months — Verbal → Official Warning → Show Cause → Hearing |
| **KPI dashboard** | Regional breakdown table + summary metrics — auto-updates on paste |
| **XLOOKUP-driven** | All tabs link back to a single Fleet Register — update driver once, reflects everywhere |

---

## 📁 Repository Structure

```
fleet-tracker/
│
├── Fleet_Management_Workbook_TEMPLATE.xlsx   ← Main workbook (open this)
│
├── sample_data/
│   ├── sample_odometer_report.csv            ← Example odometer export format
│   ├── sample_fuel_analysis.csv              ← Example fuel data format  
│   └── sample_trips_export.csv              ← Example trip-level GPS format
│
├── docs/
│   └── data_dictionary.md                   ← Column definitions for all raw tabs
│
└── README.md
```

---

## 🗂️ Workbook Tabs

| Tab | Type | Description |
|---|---|---|
| 📖 README — HOW TO USE | Guide | Built-in user guide — read before first use |
| 📋 FLEET REGISTER | Static master | All vehicles, drivers, regions. Update only on fleet changes. |
| 📥 RAW ODOMETER | **Paste monthly** | GPS odometer export — distance per vehicle |
| 📥 RAW FUEL DATA | **Paste monthly** | Fuel consumption and PJP compliance export |
| 📥 RAW TRIPS | **Paste monthly** | Trip-level GPS data — every journey logged |
| ⚡ MONTHLY ANALYSIS | Auto-calculated | Full metrics per vehicle — do not edit |
| 🚨 SPEED FLAGS | Auto-filtered + manual | All trips >90 km/h with permission & discipline columns |
| 🌙 LATE HOURS FLAGS | Auto-filtered + manual | Past 10pm and past midnight violations |
| 📝 DISCIPLINE TRACKER | Cumulative manual log | All warnings and actions — never cleared |
| 📊 KPI DASHBOARD | Auto-calculated | Regional summary — do not edit |

---

## 🚀 Getting Started

### Option A — Google Sheets (recommended)
1. Download `Fleet_Management_Workbook_TEMPLATE.xlsx`
2. Go to [Google Sheets](https://sheets.google.com) → **File → Import → Upload**
3. Select the file → choose **Replace spreadsheet**
4. All XLOOKUP and FILTER formulas are Google Sheets compatible

### Option B — Excel
1. Download `Fleet_Management_Workbook_TEMPLATE.xlsx`
2. Open in Excel 2019 or later (XLOOKUP and FILTER require Excel 2019+)

---

## 📅 Monthly Workflow

Each month, you update **three tabs only**. Everything else auto-calculates.

### Step 1 — Paste Odometer Data
- Open the **RAW ODOMETER** tab
- Clear rows 4 downward, **columns B to M only** ← important
- Paste the new GPS export starting at **cell B4** (values only — `Ctrl+Shift+V`)
- ⚠️ **Never overwrite column A** — it contains auto-formulas that extract the Reg No key

### Step 2 — Paste Fuel Data
- Open the **RAW FUEL DATA** tab
- Clear rows 4 downward (all columns)
- Paste the fuel analysis export starting at **cell A4**
- No protected columns in this tab — paste the full export as-is

### Step 3 — Paste Trip Data
- Open the **RAW TRIPS** tab
- Clear rows 4 downward, **columns A to G only** ← important
- Paste the trip-level GPS export starting at **cell A4**
- ⚠️ **Never overwrite columns H and I** — these calculate Past Midnight and Date automatically

### Step 4 — Review Flags
- Check **SPEED FLAGS** tab — fill in Permission and Discipline Action for each row
- Check **LATE HOURS FLAGS** tab — same process
- Add new entries to **DISCIPLINE TRACKER** for any actions issued

### Step 5 — Share Dashboard
- View the **KPI DASHBOARD** for the management summary — no editing needed

---

## 🚨 Flag Logic

### Speed Flags
- Threshold: **90 km/h**
- Source: every individual trip from RAW TRIPS (not just monthly max)
- Rows with no action taken glow red
- Rows marked "Permission Granted" turn gray and are excluded from counts

### Late Hours Flags
- **Past 10pm** → Amber severity
- **Past Midnight** → Red severity (higher escalation)
- Permission dropdown: Not Granted / Permission Granted / Under Investigation / Escalated to HR

### Fuel Compliance Thresholds
| Vehicle Type | Threshold |
|---|---|
| Motorcycle (Honda) | ≥ 40.5 KM/L |
| Car (Toyota Probox) | ≥ 12.6 KM/L |

---

## 📝 Discipline Progression

| Offence | Action | Dropdown Value |
|---|---|---|
| 1st | Verbal warning — log it | `Verbal Warning` |
| 2nd | Formal written warning | `Official Warning` |
| 3rd | Driver responds in writing | `Show Cause Letter` |
| 4th+ | Formal HR hearing | `Disciplinary Hearing` |

The **All-time Offence Count** column in the Discipline Tracker auto-calculates via COUNTIFS — it updates across all rows the moment a driver appears a second time anywhere in the tracker.

---

## 🛠️ Customisation

### Changing the Speed Threshold
The 90 km/h threshold is hardcoded in:
- `SPEED FLAGS` tab — the FILTER formula in cell A4 (`>90`)
- `MONTHLY ANALYSIS` tab — the conditional formatting rule

### Changing Fuel Thresholds
In the **MONTHLY ANALYSIS** tab, column J contains:
```
=IF(F2="Honda", 40.5, 12.6)
```
Update the Honda and Probox values here to change the compliance threshold for all vehicles.

### Adding a New Vehicle
1. Open **FLEET REGISTER**
2. Add a new row at the bottom with all details
3. The vehicle will appear in MONTHLY ANALYSIS automatically on next data paste

### Adding a New Region
1. Open **FLEET REGISTER** — add the region name to the Region column for relevant vehicles
2. Open **KPI DASHBOARD** — manually add a new row to the regional breakdown table and copy the COUNTIFS/SUMIFS formulas from an existing region row

---

## ⚠️ Known Limitations

- The workbook is designed for monthly reporting cycles. For weekly or daily reporting, the RAW TRIPS tab structure can be reused but flag tabs would need additional date filters.
- GPS coordinate columns in RAW TRIPS are not used in calculations — they are present in the raw export and can be left in or deleted before pasting.
- If a vehicle's tracker is offline for the full month, it will show 0 KM and "No Data" for fuel compliance. Flag these for separate investigation.
- FILTER formulas require Google Sheets or Excel 2019+. Earlier versions of Excel are not supported.

---

## 📄 Licence

MIT — free to use, adapt, and distribute.

---

## 🤝 Contributing

Pull requests welcome. For major changes, open an issue first.

Suggested improvements:
- Power BI / Looker Studio connector for dashboard visualisation
- Python script to auto-clean and standardise the GPS export before pasting
- Multi-month comparison tab (trend analysis across 3–6 months)
