# Large CheckFile — Python Script

This Jupyter Notebook processes the **Large CheckFile Report** — a comprehensive end-to-end shipment analysis for large category (FSD_LARGE) orders covering Forward, RTO, and RVP shipment types.

## What This Script Does

### 1. Data Loading & Merging
- Reads multiple FDP CSV exports (e.g., `FDP_17 Jul.csv`, `FDP_02 June.csv`)
- Concatenates and deduplicates on `tracking_id`
- Filters out invalid statuses (PickupCancelled, OutForPickupEvent, etc.)

### 2. Data Cleaning & Transformation
- Parses and normalizes date columns: `dispatch_date`, `ph_in_time`, `created_at`, `last_updated_at`
- Filters by dispatch month (excludes Oct–Mar)
- Calculates **ageing** (PH_IN date to end date)
- Calculates **Ph_in_Ageing** (PH_IN date to today)
- Assigns **ageing buckets**: `<=45` and `>45`
- Assigns **week number** based on year start (Jan 1, 2026)

### 3. Status Classification
- Derives `FDP_status`: `delivered`, `Returned_to_seller`, `Open`, `Open-RTO`
- Derives `Final_Status` by merging with Multitrack and ERP data
- Classifies `loss` categories: `Lost`, `RTS>45_60`, `RTS>60`, `conditional_Loss`, `Null`

### 4. Data Enrichment (Merges)
| Source File | Purpose |
|---|---|
| `Multitrack_<date>.csv` | Latest shipment location & status from Ekart Hub Ops |
| `Dispute_Check.csv` | Dispute status (open/accepted/partial) |
| `Rev 16May.xlsx` | Merchant-wise shipment value mapping |
| `Large XD mapping.xlsx` | Hub name normalization & XD mapping |
| `Zone Mapping.xlsx` | Destination pincode → zone mapping |
| `6a59aad3b066d77cf54396d2.csv` | Eagle Eye facility scan data |

### 5. Hub & Location Analysis
- Normalizes hub names (lowercase, strip suffixes)
- Identifies current location using latest Eagle Eye scan vs Multitrack
- Maps pickup hub to current location for pendency check
- Flags shipments at satellite/bulk hubs

### 6. Summary Pivot Reports
- Shipment count by `loss` category × `dispatch_month`
- Logistics breach amount by loss category × month
- Total shipment value by month
- Loss % of total volume

### 7. Output Files
- `large_check_<date> Rev.csv` — Full enriched CheckFile data
- `large_sum_<date> Rev.csv` — Summary pivot report

## Input Files Required

| File | Source |
|---|---|
| `FDP_<date>.csv` | FDP portal (`Ext_large_bulk_raw`) |
| `Multitrack_<date>.csv` | Ekart Hub Ops tracking portal |
| `Dispute_Check.csv` | Dispute tracker |
| `Rev 16May.xlsx` | Mapping Files folder |
| `Large XD mapping.xlsx` | Mapping Files folder |
| `Zone Mapping.xlsx` | Mapping Files folder |
| `6a59aad3b066d77cf54396d2.csv` | Eagle Eye portal |

## Requirements

- Python 3
- JupyterLab / Jupyter Notebook
- Libraries: `pandas`, `numpy`, `openpyxl`, `selenium`, `beautifulsoup4`

## Usage

1. Place all input files in the working directory.
2. Open the `.ipynb` file in JupyterLab.
3. Run all cells sequentially.
4. Output CSVs will be saved in the same directory.
