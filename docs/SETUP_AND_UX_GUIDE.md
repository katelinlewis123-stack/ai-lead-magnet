# Setup + UI/UX Guide: AI Demand Forecasting Spreadsheet

## 1) Workbook Structure (recommended tabs)
1. `Instructions`
2. `Historical_Data`
3. `Model_Inputs`
4. `Forecast_Output`
5. `Dashboard`

Import the CSV templates into matching tabs.

---

## 2) Quick Setup (Google Sheets or Excel)

1. Create a new workbook.
2. Import each CSV from `/templates` into a separate tab.
3. Keep tab names exactly as shown above, because formulas reference those names.
4. Copy formulas in `Forecast_Output` row 2 down to match your forecast horizon.

---

## 3) UI/UX Standards (for a clean operator experience)

### Layout and visual hierarchy
- Freeze top row on all tabs.
- Use one header style across all sheets:
  - Fill: dark blue (`#1F4E78`)
  - Font: white, bold
- Use soft blue (`#DCE6F1`) for label cells in control/KPI sections.
- Keep data-entry cells white and formula cells light gray (`#F2F2F2`).

### Input ergonomics
- Add dropdown validation (`0,1`) for all promo and stockout flags.
- Add date validation to `Month` columns.
- Protect formula ranges so users only edit input fields.

### Forecast readability
- Conditional format `Reorder_Point_Units`:
  - Red = highest values (replenishment risk)
  - Yellow = medium
  - Green = lowest
- Show bounds (`Lower_Bound`, `Upper_Bound`) to communicate uncertainty.

### Dashboard UX
- Create KPI cards at top:
  - Avg historical monthly sales
  - Avg forecast monthly sales
  - Projected annual demand
  - Avg reorder point
- Add two charts:
  1. Line chart: historical sales + adjusted forecast.
  2. Column chart: lower vs upper forecast bounds (next 12 months).

---

## 4) Model Logic (business-friendly)

- **Baseline forecast** = trailing 12-month average × month season multiplier.
- **Adjusted forecast** = baseline × (1 + promo flag × promo uplift %).
- **Safety stock** = trailing 12-month demand stdev × Z-score × sqrt(lead time).
- **Reorder point** = adjusted forecast × lead time + safety stock.

This provides a robust baseline for monthly planning and inventory policy.

---

## 5) Data Requirements

Minimum required:
- 24+ months of monthly demand history.
- Promo flags by month.
- Known stockout periods (if available).
- Seasonality assumptions.

Recommended enhancements:
- Price changes.
- Holidays/events.
- Regional/store segmentation.

---

## 6) AI Upgrade Path (production-ready)

Once baseline spreadsheet adoption is stable, use the same features in:

### Amazon Forecast
- Upload target series + related time series (promo, holiday/event).
- Train and compare predictors (e.g., DeepAR+).
- Export quantile forecasts and map them back to planning tabs.

### Azure AutoML Time Series
- Train with Date, Sales, and covariates (promo/seasonal signals).
- Compare models with rolling-origin validation.
- Deploy best model and refresh forecast monthly.

### Facebook Prophet (open-source)
- Train with `ds` (date), `y` (demand), and promo regressor.
- Add custom seasonality and holidays.
- Export monthly forecast + confidence interval into spreadsheet.

---

## 7) Rollout Plan (1–3 months)

1. **Weeks 1–2**: Data cleaning and schema alignment.
2. **Weeks 3–4**: Pilot forecast workbook with planners.
3. **Weeks 5–8**: Tune assumptions and policy thresholds.
4. **Weeks 9–12**: Optional AI model integration and governance.

---

## 8) Expected Outcomes

- Better demand visibility for planners.
- Lower stockout and overstock risk.
- Waste reduction, especially for perishable or short shelf-life items.

