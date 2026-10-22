# FitnessHub Subscription Analytics

Interactive Power BI analysis of subscription retention, revenue, and customer acquisition cost recovery.

## Executive summary

### Customer cohort analysis

The highest cancellation risk occurs **after approximately three months**, at the transition from the third to the fourth payment month. Across mature cohorts, retention falls from approximately **78% at month index 2 to 50% at month index 3**, a decline of about **28 percentage points**. About half of the original customers remain by the fourth payment month. FitnessHub should strengthen onboarding, personalized engagement, and renewal reminders before this transition.

This finding comes directly from the retention heatmap. Month 0 is the initial subscription payment; cancellation is inferred from payment cessation because the source does not include explicit cancellation dates. Blank heatmap cells represent future months that have not yet been observed.

### Revenue forecast

FitnessHub is forecast to generate **2,682,598 in subscription revenue from February 2026 through January 2027**, averaging approximately **223,550 per month**. This total sums the 12 point forecasts exported from the native Power BI forecast visual. The January 2026 historical anchor is excluded.

Monthly revenue is projected to rise from **178,248 in February 2026 to 259,124 in January 2027**. The outlook is predominantly upward, with modest seasonal variation: forecast revenue dips from **256,385 in November to 252,420 in December**, then increases in January. The forecast therefore indicates growth with a smaller seasonal component rather than large recurring swings. Only 25 historical months are available, so leadership should treat the seasonal pattern cautiously.

The **95% prediction band widens** over the forecast horizon, indicating greater uncertainty further ahead. These monthly bounds are not a confidence interval for the annual total. Leadership should use the forecast for planning while monitoring actual revenue, retention, and acquisition spending.

The visual uses **dim_date[StartOfMonth]** on a **continuous** X-axis and **Subscription Revenue** on the Y-axis. Forecast settings: **12 months**, **seasonality 12**, **95% confidence interval**, **ignore last 0**.

### CAC vs. LTV

| Acquisition cohort | Customers | Total CAC | CAC recovery |
|---|---:|---:|---|
| 2024 | 2,816 | 501,536.80 | Month index **7**, the **eighth payment month** |
| 2025 | 6,484 | 1,162,459.11 | **Not reached within available observations** |

The chart compares cumulative subscription revenue with a separate **Y-axis reference line driven by the Total CAC measure**. Total CAC is not a regular Y-axis series. The single-select cohort-year filter changes both the revenue curve and CAC benchmark.

Later tenure points for 2025 contain only earlier signups; they do not represent complete follow-up for every customer. The absence of observed payback is not a prediction that the cohort will never recover CAC. Revenue-based recovery also excludes service delivery costs and profit margins.

### Business conclusion

The forecast supports continued subscription revenue growth, but early churn and unrecovered acquisition spending for the 2025 cohort create sustainability concerns. Prioritize retention before the fourth payment month and review cohort CAC recovery alongside revenue growth. Expanding acquisition alone does not demonstrate profitable growth.

## Data and model

- Source: `data/Fitness_Subscriptions_Dataset.xlsx`, containing `customers` and `transactions`.
- Coverage: January 2024–January 2026; 9,300 customers and 64,979 transactions.
- Subscription-only analysis: 51,982 transactions where Transaction_Type is Subscription.
- Cohort Month: first subscription payment month; signup dates are not supplied separately.
- Relationships: customers[Customer_ID] **1 → many** transactions[Customer_ID]; dim_date[Date] **1 → many** transactions[Transaction_Date]. Tenure is a disconnected month-index table.
- dim_date uses ADDCOLUMNS and CALENDAR; StartOfMonth stores each date’s month start.
- CAC is counted once per customer. Cohort LTV is cumulative subscription revenue across original cohort members.
- Currency is not identified in the source, so values are reported in dataset currency units.

## Forecast values from Power BI

| Month | Subscription revenue forecast |
|---|---:|
| 2026-02 | 178,248 |
| 2026-03 | 186,133 |
| 2026-04 | 194,569 |
| 2026-05 | 202,828 |
| 2026-06 | 215,479 |
| 2026-07 | 222,543 |
| 2026-08 | 230,046 |
| 2026-09 | 238,054 |
| 2026-10 | 246,769 |
| 2026-11 | 256,385 |
| 2026-12 | 252,420 |
| 2027-01 | 259,124 |
| **12-month total** | **2,682,598** |

The original visual export, including monthly upper/lower bounds and the historical anchor, is in `analysis/power_bi_forecast_export.csv`. The 12 future months alone are in `analysis/forecast_12_months.csv`. Exported values are reported at the whole-unit precision provided by Power BI.

## Files and report use

Open `report.pbix` in Power BI Desktop and select Customer Cohort, Revenue Forecast, or CAC vs. LTV. Embedded data supports viewing the saved report; refreshing requires the Excel source path to point to its location on the viewer’s computer.

`screenshots/cohort_analysis.png`, `screenshots/revenue_forecast.png`, and `screenshots/cac_vs_ltv.png` are actual screenshots of the revised report. `screenshots/data_model.png` is a logical relationship diagram; it should be replaced with an actual Power BI Model view screenshot for the required screenshot submission.
