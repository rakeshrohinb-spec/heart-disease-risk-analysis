# Heart Disease Risk Factor Analysis

I worked through the classic UCI Cleveland Heart Disease dataset (303 patients, 14 clinical variables, with disease severity as the target from 0 = no disease up to 4 = most severe) to see what actually separates healthy patients from at-risk ones. Cleaned and explored it in Python, then built it out into an interactive Power BI dashboard.

**Tools used:** Python (pandas, seaborn, matplotlib), Power BI (Power Query, DAX)

![Dashboard preview](dashboard_screenshot.png)

## Cleaning the data

Two columns, `ca` and `thal`, had missing values hiding as the string `"?"` instead of actual nulls — a pretty common thing to run into with real-world data. Fixed it with `heart.replace('?', np.nan)` followed by `dropna()`, which took the dataset from 303 down to 297 rows.

Also had to explicitly convert both columns to numeric with `pd.to_numeric()`, since pandas treats a column as text once it's seen even one non-numeric value in it. Double-checked afterward that there were zero nulls left and all the dtypes were correct before moving into analysis.

## Checking outliers

Ran the IQR method on cholesterol, resting blood pressure, ST depression (`oldpeak`), and max heart rate (`thalach`). Instead of just dropping anything flagged as an outlier, I checked each one against disease severity first:

- **Cholesterol:** 5 outliers, spread across all severity classes — no real pattern, so kept.
- **Resting blood pressure:** 9 outliers, no clear connection to severity either — kept.
- **ST depression (oldpeak):** 5 outliers, but 4 of the 5 were in the severe or very-severe groups. That's not noise, that's signal — kept.
- **Max heart rate (thalach):** just 1 outlier, and it pointed the same direction as the rest of the disease pattern — kept.

## What I found

**There's a real gender gap.** Male patients had more than double the heart disease rate of female patients — 56% vs. 26%.

**The asymptomatic paradox.** The patients who reported no typical chest pain at all actually had the *highest* disease rate of any chest pain category, at 73% — higher than patients who had classic "typical angina" symptoms. Counterintuitive, but it lines up with what's known clinically: heart disease often doesn't announce itself the way people expect.

**Oldpeak and max heart rate turned out to be the real leading indicators.** ST depression rises and max heart rate falls in a near-monotonic pattern as severity increases — a clean, consistent trend across every stage. Cholesterol, despite being the variable everyone assumes matters most, barely showed any relationship to severity at all.

## Inside the dashboard

- KPI cards for total patients (297), % with disease (46.13%), and average age (54.54)
- Disease severity by gender — bar chart, confirms the gender gap
- Disease severity by chest pain type — bar chart, shows the asymptomatic paradox
- Heart rate & oldpeak trends by severity — dual-axis line chart, sorted in proper clinical severity order rather than alphabetically

## Files here

- `heart-disease-risk-analysis.ipynb` — the full Python cleaning + EDA notebook
- `Hear_Disease_Cleaned.csv` — cleaned dataset
- `Heart-Disease-Risk-Analysis - Dashboard.pbix` — the Power BI dashboard
- `dashboard_screenshot.png` — preview image

## Why this project

This was mostly about practicing the parts of data work that don't show up in a tutorial — catching missing data that's disguised rather than obvious, deciding case-by-case whether an outlier is noise or a real signal instead of just deleting anything unusual, and pulling out a genuinely counterintuitive finding (the asymptomatic paradox) rather than just confirming what everyone already assumes.
