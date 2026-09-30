# Building the dashboard in Power BI Desktop

Power BI is free from the Microsoft Store. `.pbix` files can't be generated from code, so build it with these steps (about 30 minutes).

## 1. Load data
Home > Get data > Text/CSV > `data/fred_clean.csv` > Transform Data. Confirm `Date` is type **Date** and the rest are **Decimal number**, then Close & Apply.

## 2. Measures (Modeling > New measure)

```DAX
Current Mortgage Rate =
VAR d = MAX(fred_clean[Date])
RETURN CALCULATE(AVERAGE(fred_clean[MORTGAGE30US]), FILTER(ALL(fred_clean), fred_clean[Date] = d))

Mortgage Rate 1Y Ago =
VAR d = EDATE(MAX(fred_clean[Date]), -12)
RETURN CALCULATE(AVERAGE(fred_clean[MORTGAGE30US]), FILTER(ALL(fred_clean), fred_clean[Date] = d))

Mortgage YoY Change = [Current Mortgage Rate] - [Mortgage Rate 1Y Ago]

Current Unemployment =
VAR d = MAX(fred_clean[Date])
RETURN CALCULATE(AVERAGE(fred_clean[UNRATE]), FILTER(ALL(fred_clean), fred_clean[Date] = d))
```

`MAX(fred_clean[Date])` is the last month inside the date slicer, so the cards update when you change the slicer range.

## 3. Visuals

| Visual | Setup |
|---|---|
| 3 KPI cards | `Current Mortgage Rate`, `Mortgage YoY Change`, `Current Unemployment` |
| Line chart: mortgage vs fed funds | X: Date; Y: MORTGAGE30US, FEDFUNDS |
| Line and column chart: delinquency vs unemployment | X: Date; columns: UNRATE; line: DRSFRMACBS (secondary axis) |
| Line chart: mortgage spread | X: Date; Y: Mortgage_Spread |
| Date slicer | Date, style "Between" |

Layout tips: KPI cards across the top, slicer beside them, two charts in the middle, spread chart at the bottom. Add a title and a "Source: FRED" text box.

## 4. Save and screenshot
Save as `fred-economic-dashboard.pbix` in the repo root. Set the slicer to 2007 to 2010 (or 2020 to 2023) and screenshot the page as `dashboard_screenshot.png`.
