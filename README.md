# Mortgage & Economic Indicators Dashboard

How do interest rates, inflation, and unemployment relate to mortgage rates and mortgage delinquencies? This project combines six Federal Reserve (FRED) series into one monthly dataset and visualizes it in Power BI.

## Data (FRED, fred.stlouisfed.org)

| Series | Description | Native frequency |
|---|---|---|
| MORTGAGE30US | 30-year fixed mortgage rate | Weekly |
| FEDFUNDS | Federal funds rate | Monthly |
| DGS10 | 10-year Treasury yield | Daily |
| UNRATE | Unemployment rate | Monthly |
| CPIAUCSL | Consumer price index | Monthly |
| DRSFRMACBS | Delinquency rate, single-family mortgages | Quarterly |

## Cleaning (`cleaning.ipynb`)

- Loaded 6 raw series (range 1947 to Sept 2026), converted FRED's `.` placeholders to nulls (720 missing daily Treasury values, mostly holidays).
- Converted daily and weekly series to **monthly averages**; carried each quarterly delinquency value across its 3 months.
- Calculated **inflation as year-over-year % change in CPI** and the **mortgage spread** (30-yr mortgage rate minus 10-yr Treasury).
- Dropped October 2025: BLS did not publish UNRATE/CPI for that month, so I left it out rather than fabricate a value.
- Delinquency data is published with a lag, so the latest 4 months are blank.
- **Result: `data/fred_clean.csv`, 427 rows x 9 columns, Jan 1991 to Aug 2026.**

## Dashboard

Built in Power BI Desktop (see [POWERBI_GUIDE.md](POWERBI_GUIDE.md)). The slicer here is set to 2007 to 2023.

![Dashboard screenshot](dashboard_screenshot.png)

## Insights

1. **Mortgage rates moved before the Fed did.** The Fed's first 2022 hike was in April (fed funds 0.08% to 0.20% in Q1), but the 30-year rate had already climbed from 3.10% in Dec 2021 to 5.0%+ by May 2022, when fed funds was only 0.77%. By Dec 2022 mortgage rates were 6.36% (+3.3 pts in 12 months) with fed funds at 4.10%. Mortgage rates track the 10-year Treasury and Fed expectations, not the policy rate itself.
2. **Delinquencies follow unemployment, except when policy intervenes.** Across 1991 to 2026, unemployment and mortgage delinquency have a 0.70 correlation (strongest when unemployment leads by ~3 months, 0.71). In the 2008 crisis unemployment peaked at 10.0% (Oct 2009) and delinquencies peaked at 11.48% (Jan 2010). In 2020, unemployment spiked to 14.8% but delinquency only edged from 2.35% to 2.84%, because forbearance and stimulus broke the link. For a lender, unemployment alone is a poor risk signal without policy context.
3. **The mortgage spread widened sharply after 2022.** From 1991 to 2019 the average spread over the 10-year Treasury was 1.66 pts. It hit 2.97 pts in June 2023, above the 2008 peak (2.87), and averaged 2.84 for 2023. It has since eased to 1.98 pts (Aug 2026), so a large part of borrowers' high rates in 2023 was risk/volatility pricing, not just Treasury yields.

## Current snapshot (Aug 2026)

30-yr mortgage rate 6.67% (vs 6.59% a year earlier), unemployment 4.1%, inflation 3.35% YoY, fed funds 3.63%.

## Repo layout

```
data/            raw FRED CSVs + fred_clean.csv
cleaning.ipynb  cleaning and merging notebook
POWERBI_GUIDE.md dashboard build steps and DAX
fred-economic-dashboard.pbix  Power BI dashboard
dashboard_screenshot.png  dashboard screenshot
```
