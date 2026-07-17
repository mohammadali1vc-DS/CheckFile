# Data Sources & Download Links

This file contains the source links used to download input data for the Large CheckFile report processing.

## Data Sources

### 1. FDP Report (Ext_large_bulk_raw)
- **URL:** https://fdp.fkinternal.com/reports/view/scp/ekl/Ext_large_bulk_raw
- **Description:** Raw bulk shipment data for large category orders.
- **Filters Applied:**
  - `created_at`: Last 3 Months + Current Month
  - `vendor_code`: Default (Don't Touch)
  - `merchant_code`: Default (Don't Touch)
  - `ekl_shipment_type`: Default (Don't Touch)

### 2. Ekart Hub Ops — Shipment Tracking
- **URL:** http://10.24.1.71/hub-ops/tracking/shipments
- **Description:** Internal Ekart logistics portal for multiple shipment tracking. Enter shipment IDs to track or download current status.

### 3. Eagle Eye Portal
- **URL:** https://eagleeye.portalonewifi.com/login
- **Description:** Eagle Eye admin portal used to fetch hub-level and facility-level shipment data.

## Notes
- FDP data is the primary source for the CheckFile report.
- Ekart Hub Ops and Eagle Eye are used for cross-verification and tracking updates.
- All links are internal and require authorized access.
