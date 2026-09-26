# Data Analyst Interview Prep — 100 Questions + Healthcare/Comp Bio/AI Additions

---

## 🧠 Data Analyst Role Basics

**1. What does a data analyst do in a company?**
Collects, cleans, and analyzes data to answer business questions and support decisions. Builds reports/dashboards, runs ad-hoc analyses, and communicates findings to stakeholders so they can act on evidence rather than intuition.

**2. Difference between data analyst, data scientist, and BI analyst?**
- **Data analyst**: answers specific business questions, mostly SQL/Excel/BI tools, descriptive/diagnostic focus.
- **Data scientist**: builds predictive models, uses ML/statistics, works more in Python/R, often productionizes models.
- **BI analyst**: focuses on building/maintaining dashboards and reporting infrastructure for ongoing monitoring.
Overlap is heavy; titles vary a lot by company.

**3. Typical workflow of a data analyst (requirement to insight)?**
Clarify the business question → identify data sources → extract/query data → clean and validate → explore (EDA) → analyze → visualize → interpret and write recommendations → present to stakeholders → iterate based on feedback.

**4. Main goals of data analysis?**
- **Descriptive** – what happened (reports, dashboards)
- **Diagnostic** – why it happened (root-cause, drill-downs)
- **Predictive** – what will happen (forecasting, modeling)
- **Prescriptive** – what should we do (recommendations, optimization)

**5. What is a KPI and why is it important?**
A Key Performance Indicator is a measurable value tied directly to a strategic goal (e.g., 30-day readmission rate, patient wait time). KPIs matter because they focus attention and let teams track progress against objectives, not just activity.

**6. Difference between metrics and KPIs?**
All KPIs are metrics, but not all metrics are KPIs. A metric is any measurable number (e.g., number of lab tests run); a KPI is a metric explicitly chosen because it tracks a strategic goal (e.g., diagnostic turnaround time as a quality-of-care KPI).

**7. Dashboard vs report?**
A report is typically static, point-in-time, and narrative (PDF/slide deck for a decision or meeting). A dashboard is a live, interactive, ongoing monitoring tool that updates as new data arrives and lets users filter/drill down themselves.

**8. What is exploratory data analysis (EDA)?**
The process of summarizing a dataset's main characteristics — distributions, missingness, outliers, relationships between variables — usually via visuals and summary stats, before formal modeling or reporting, to catch data issues and generate hypotheses.

**9. Raw data vs processed data?**
Raw data is unaltered, as collected (e.g., a raw EHR extract with duplicate encounters, free text, inconsistent units). Processed data has been cleaned, transformed, validated, and structured for analysis (deduplicated, standardized units, coded fields).

**10. How do you prioritize which analysis to work on first?**
Weigh business impact (revenue/clinical outcome/decision at stake), urgency/deadline, effort required, and how many stakeholders depend on it. I use a quick impact-vs-effort framing and confirm priorities with the requester rather than assuming.

---

## 📊 SQL & Databases

**11. What is SQL and why is it critical for a data analyst?**
Structured Query Language is the standard language for querying and manipulating relational databases. It's critical because most enterprise/clinical data (EHRs, claims, lab systems) lives in relational databases, and SQL lets analysts pull exactly the data needed at scale without exporting whole tables.

**12. How do SELECT, WHERE, ORDER BY, LIMIT work?**
`SELECT` picks columns; `WHERE` filters rows before aggregation based on a condition; `ORDER BY` sorts the result set (ASC/DESC); `LIMIT` caps the number of rows returned. Example: `SELECT patient_id, age FROM patients WHERE age > 65 ORDER BY age DESC LIMIT 10;`

**13. How do you join two tables (INNER, LEFT, RIGHT, FULL)?**
- **INNER JOIN**: only matching rows in both tables.
- **LEFT JOIN**: all rows from the left table, matched rows from the right (NULLs if no match).
- **RIGHT JOIN**: mirror of LEFT.
- **FULL OUTER JOIN**: all rows from both, matched where possible.
Example: join `encounters` to `patients` on `patient_id` to attach demographics to each visit.

**14. How do GROUP BY and aggregates work?**
`GROUP BY` collapses rows sharing a value into one row per group, and aggregate functions (`SUM`, `AVG`, `COUNT`, `MAX`, `MIN`) compute over each group. E.g., `SELECT department, AVG(length_of_stay) FROM encounters GROUP BY department;`

**15. How do you write subqueries and CTEs?**
A subquery is a query nested inside another (in `WHERE`, `FROM`, or `SELECT`). A CTE (`WITH x AS (...)`) is a named, temporary result set that makes complex queries readable and reusable, especially for multi-step logic (e.g., first compute per-patient visit counts, then filter on that in the outer query).

**16. How do you calculate running totals or rolling averages with window functions?**
Window functions compute across a set of rows related to the current row without collapsing them. `SUM(cost) OVER (PARTITION BY patient_id ORDER BY visit_date)` gives a running total per patient; `AVG(value) OVER (ORDER BY date ROWS BETWEEN 6 PRECEDING AND CURRENT ROW)` gives a 7-row rolling average.

**17. How do you clean and filter data directly in SQL?**
Use `WHERE`/`HAVING` to exclude bad rows, `TRIM`/`UPPER`/`REPLACE` to standardize text, `CASE WHEN` to recode values, `CAST`/`CONVERT` to fix types, and `COALESCE` to fill nulls with defaults.

**18. How do you handle duplicates and NULLs in SQL?**
Duplicates: `DISTINCT`, or `ROW_NUMBER() OVER (PARTITION BY key ORDER BY ...)` and keep row_number = 1. NULLs: `IS NULL`/`IS NOT NULL` for filtering, `COALESCE(col, default)` to substitute, and understanding that NULL never equals NULL in comparisons.

**19. How do you optimize a slow query?**
Check the execution plan (`EXPLAIN`), add indexes on filtered/joined columns, avoid `SELECT *`, filter early, avoid functions on indexed columns in `WHERE`, reduce unnecessary joins/subqueries, and consider materialized views or pre-aggregated tables for repeated heavy queries.

**20. How do you design a simple schema for a business domain?**
Identify entities (e.g., `patients`, `encounters`, `providers`, `diagnoses`), define primary keys, add foreign keys to link related tables, normalize to reduce redundancy (e.g., store diagnosis codes in a reference table, not repeated text), and consider indexing on commonly joined/filtered columns.

---

## 🧮 Excel & Spreadsheets

**21. How do you use Excel for quick data cleaning/analysis?**
Text-to-columns for splitting fields, Find & Replace for standardizing values, Remove Duplicates, Data Validation to catch bad entries, filters/sorts for quick inspection, and formulas (`TRIM`, `SUBSTITUTE`) to clean text before deeper analysis.

**22. SUMIF, COUNTIF, VLOOKUP/XLOOKUP?**
`SUMIF(range, criteria, sum_range)` sums values matching a condition; `COUNTIF(range, criteria)` counts matches; `VLOOKUP(value, table, col_index, FALSE)` looks up a value in the first column and returns a value from another column; `XLOOKUP` is the modern replacement — more flexible (can look left, has cleaner not-found handling).

**23. Remove duplicates and standardize text in Excel?**
Data → Remove Duplicates for exact row duplicates; `TRIM`, `PROPER`/`UPPER`/`LOWER`, and `SUBSTITUTE` to standardize casing/whitespace; Flash Fill for pattern-based cleanup.

**24. PivotTables for summarizing data?**
PivotTables let you drag fields into Rows/Columns/Values/Filters to quickly aggregate (sum, average, count) large datasets without formulas — e.g., average length of stay by department and month.

**25. Building simple dashboards in Excel (charts + slicers)?**
Build PivotTables/PivotCharts from clean source data, add Slicers/Timelines for interactive filtering, arrange visuals on one sheet, and use consistent formatting/labels so a non-technical viewer can self-serve.

**26. Conditional formatting for insights?**
Highlights values meeting rules (e.g., red for readmission rates above threshold, color scales for trend visibility) so outliers and patterns are visually obvious without reading every cell.

**27. Exporting/sharing formatted reports?**
Export to CSV/PDF for sharing outside Excel, use defined print areas and headers for polished PDFs, and protect/lock cells before distributing so formulas aren't accidentally broken.

**28. Handling large datasets: Excel vs a database?**
Excel struggles past roughly a million rows and gets slow/unstable well before that with formulas. Databases handle millions/billions of rows efficiently via indexing and set-based operations — large or recurring analyses belong in SQL/a database, with Excel used for final presentation layers.

**29. Avoiding common Excel pitfalls?**
Avoid hard-coded numbers (use cell references/formulas), always label axes/units, avoid manual copy-paste of formulas across large ranges (use structured tables), and version-control important workbooks to avoid silent overwrites.

**30. Documenting Excel analyses?**
Use a dedicated "Notes/Methodology" tab describing data sources, filters applied, assumptions, and last-refreshed date, and use named ranges/clear headers instead of unlabeled formulas so someone else can audit the logic.

---

## 📈 Data Visualization & BI Tools

**31. Purpose of data visualization?**
To make patterns, trends, and outliers immediately perceivable — visuals let stakeholders grasp information far faster and more accurately than tables of numbers, and support faster, better decisions.

**32. When to use bar, line, pie, histogram?**
- **Bar**: comparing categories (e.g., cases by hospital).
- **Line**: trends over time (e.g., monthly infection rate).
- **Pie**: parts of a whole with few categories (use sparingly — bars are usually clearer).
- **Histogram**: distribution of a continuous variable (e.g., age distribution of patients).

**33. Best practices for labeling, colors, readability?**
Always label axes and units, use a clear title stating the takeaway, limit colors and use them meaningfully (not decoratively), avoid 3D effects/chartjunk, and ensure sufficient contrast/colorblind-friendly palettes.

**34. Designing a dashboard for a non-technical stakeholder?**
Lead with the key metric/answer, minimize jargon, use large clear visuals over dense tables, provide simple filters, and add brief annotations explaining what "good" vs "bad" looks like.

**35. Report vs self-service dashboard?**
A report answers a specific question once/periodically and is authored by the analyst. A self-service dashboard lets users explore data themselves via filters, reducing repeated ad-hoc requests to the analytics team.

**36. Power BI / Tableau / Looker / Data Studio?**
All connect to data sources and let you build interactive visuals/dashboards. Tableau is strong for exploratory visual analytics, Power BI integrates tightly with Microsoft/Excel and has strong DAX modeling, Looker uses LookML for governed semantic modeling, Data Studio (Looker Studio) is free/lightweight for simpler reporting.

**37. Filtering and slicing data in a BI tool?**
Apply filters at the data-source, dashboard, or visual level; use parameters/slicers for user-driven filtering (e.g., date range, department); cascading filters let one selection narrow others.

**38. Measures and dimensions in BI tools?**
Dimensions are categorical/descriptive fields used to slice data (e.g., department, gender, diagnosis code). Measures are numeric values that get aggregated (e.g., cost, length of stay, count of encounters).

**39. Sharing dashboards and controlling access?**
Publish to a shared workspace/server, set row-level security or role-based permissions (critical for PHI — ensure users only see data they're authorized for), and use scheduled refresh/subscriptions for stakeholders.

**40. Telling a "data story" with charts/annotations?**
Structure visuals in a logical narrative arc (context → finding → implication), annotate key inflection points directly on the chart, and keep only the visuals that support the specific conclusion — cut anything that doesn't move the story forward.

---

## 📊 Descriptive Statistics & EDA

**41. Mean, median, mode?**
Mean = arithmetic average; median = middle value when sorted (robust to outliers); mode = most frequent value. In skewed healthcare data (e.g., hospital charges), median is often more representative than mean.

**42. Standard deviation and variance?**
Variance is the average squared deviation from the mean; standard deviation is its square root, expressed in the same units as the data — both measure spread/variability around the mean.

**43. Quartiles and IQR?**
Quartiles split sorted data into four equal parts (Q1=25th percentile, Q2=median, Q3=75th percentile). IQR = Q3−Q1, representing the spread of the middle 50% of data, used for detecting outliers.

**44. Detecting outliers and what to do with them?**
Detect via IQR rule (below Q1−1.5×IQR or above Q3+1.5×IQR), z-scores (|z|>3), or boxplots/scatterplots. Then investigate cause: data entry error → correct/remove; genuine extreme case (e.g., an ICU outlier) → keep but consider robust statistics or flag separately.

**45. Distributions and how to inspect them?**
A distribution shows how values are spread across a range. Inspect with histograms (shape), boxplots (spread/outliers), or density plots — checking for normality, skew, and multimodality informs which statistical methods are appropriate.

**46. Skewness and kurtosis?**
Skewness measures asymmetry (right-skew = long tail toward high values, common in cost/length-of-stay data). Kurtosis measures "tailedness" — how much data is in the tails vs the center compared to a normal distribution.

**47. Growth rate, percentage change, CAGR?**
% change = (new−old)/old × 100. Growth rate is the same concept over a period. CAGR (Compound Annual Growth Rate) = (End/Start)^(1/years) − 1, showing the smoothed annual growth rate over multiple periods.

**48. Cohort-style metrics (retention by signup month)?**
Group users/patients by a starting event (signup month, first diagnosis month), then track a metric (e.g., % still active, % readmitted) at fixed intervals afterward (month 1, 2, 3...) to compare cohorts on equal footing.

**49. Summarizing categorical vs numerical data?**
Categorical: frequency counts, proportions, mode, bar charts. Numerical: mean/median, std dev, quartiles, histograms/boxplots. Cross-tabulations and grouped summary stats combine both.

**50. Structuring an EDA notebook/report?**
1) Data overview (shape, types, missingness) 2) Univariate analysis 3) Bivariate/multivariate relationships 4) Outlier/data quality checks 5) Key findings and hypotheses 6) Next steps — keep it narrative, not just code output.

---

## 🛠️ Python (or R) for Data Analysis

**51. Why Python instead of (or with) Excel?**
Python handles much larger datasets, is fully reproducible/scriptable (no manual clicking), integrates with statistics/ML libraries, and can automate repetitive pipelines — Excel remains useful for quick, small, ad-hoc looks and stakeholder-facing outputs.

**52. Loading data from CSV/SQL into pandas?**
`pd.read_csv('file.csv')` for CSV; for SQL, use a connection (e.g., `sqlalchemy.create_engine`) with `pd.read_sql(query, engine)` to pull a query result directly into a DataFrame.

**53. Inspecting rows, shape, dtypes, missing values?**
`df.head()`/`df.tail()`, `df.shape`, `df.dtypes` or `df.info()`, and `df.isnull().sum()` to count missing values per column — always a first step before analysis.

**54. Cleaning missing values?**
`df.dropna()` removes rows/columns with missing data; `df.fillna(value)` fills with a constant, mean/median, or forward/backward fill; interpolation (`df.interpolate()`) estimates values based on trend — the right choice depends on why data is missing and how much.

**55. Filtering, sorting, grouping with pandas?**
Filter: `df[df['age'] > 65]`. Sort: `df.sort_values('date')`. Group: `df.groupby('department')['cost'].mean()` aggregates by category.

**56. Aggregates and pivots with groupby/pivot_table?**
`df.groupby(['dept','month']).agg({'cost':'sum','los':'mean'})` computes multiple aggregates by group; `pd.pivot_table(df, index='dept', columns='month', values='cost', aggfunc='sum')` reshapes into a wide summary table, similar to an Excel PivotTable.

*(Note: your original list skips 57–59 — commonly these cover merging/joining DataFrames, string/date operations, and basic visualization with matplotlib/seaborn. I've added them below in the supplementary section.)*

---

## 📈 Business Metrics & Analytical SQL (60–69)

**60. Month-on-month or week-on-week growth?**
Use a window function to get the prior period's value, then compute % change: `(current - LAG(current) OVER (ORDER BY month)) / LAG(current) OVER (ORDER BY month)`.

**61. Retention/churn query?**
Define an active period (e.g., monthly). For each cohort month, count users active in month 0, then check what fraction of that same group is active in month N: `retained = COUNT(DISTINCT user_id WHERE active in month N) / COUNT(DISTINCT user_id in cohort)`.

**62. LTV (lifetime value) conceptually?**
Estimated total value (revenue, or in healthcare, utilization/cost) a patient/customer generates over their entire relationship — often approximated as average revenue per period × average expected lifespan (or retention-adjusted).

**63. Funnel analysis query (sign-up → activation → purchase)?**
Use conditional aggregation or self-joins to count distinct users reaching each stage: `SUM(CASE WHEN stage='signup' THEN 1 END)`, `SUM(CASE WHEN stage='activated' THEN 1 END)`, etc., then compute conversion rate between consecutive stages.

**64. Time-based aggregations (daily/weekly/monthly)?**
Use date truncation functions (`DATE_TRUNC('month', date)` or `DATEPART`) to bucket rows before grouping and aggregating — allows rolling up transaction-level data to any time granularity.

**65. Comparing cohorts (e.g., users by acquisition month)?**
Assign each user to a cohort based on first activity date, then compare a metric (retention, spend, readmission) across cohorts at equivalent time offsets to see if newer cohorts perform better/worse.

**66. Lead-time, cycle-time, process metrics?**
Lead time = total time from request to completion; cycle time = time actively spent working on it. Computed as `DATEDIFF(end_date, start_date)` between relevant timestamp columns, then averaged/segmented by category.

**67. A/B test-style analysis in SQL?**
Aggregate the outcome metric by test group (`GROUP BY variant`), compute conversion rate/mean per group, and pull the counts needed for a significance test (chi-square or t-test) done in Python/R or a stats tool — SQL usually can't compute p-values natively.

**68. Approximate RFM segmentation in SQL?**
Compute Recency (days since last visit/purchase), Frequency (count of visits), Monetary (total spend/cost) per customer/patient, then bucket each into quantiles (e.g., `NTILE(5)`) and combine scores to segment (e.g., high-value frequent vs at-risk).

**69. Documenting and versioning SQL queries?**
Store queries in a Git repo with clear file names/comments explaining purpose and assumptions, use consistent formatting/style, and note the business context and last-validated date at the top of each query.

---

## 🧠 Behavioral & Business-Sense Questions

**70. Walk me through a real-world analysis end-to-end.**
Use the STAR framework: Situation (business question), Task (your role), Action (data sourced, methods, tools), Result (what changed because of it, with a number if possible). Pick an example with a measurable outcome.

**71. Presenting insights to a non-technical audience.**
Emphasize leading with the "so what," using plain language over jargon, visuals over tables, and checking understanding through questions — describe a specific time you adjusted your framing for the audience.

**72. A time your analysis changed a decision or strategy.**
Pick a concrete example where a specific finding directly influenced a resource, policy, or process change, and quantify the impact if possible (e.g., "led to reallocating staff to reduce ER wait times by X%").

**73. Finding and fixing a data quality issue.**
Describe how you noticed the anomaly (e.g., duplicate patient IDs, impossible values), how you investigated root cause, the fix you implemented or recommended, and how you prevented recurrence (validation rule, alert).

**74. Translating a vague business question into concrete analysis.**
Ask clarifying questions to understand the decision being made, define specific measurable metrics, scope the time frame/population, and confirm the plan with the stakeholder before diving in.

**75. Handling conflicting priorities from stakeholders.**
Clarify each request's urgency/impact, communicate transparently about capacity and trade-offs, and (when possible) escalate to a manager or use a shared prioritization framework rather than silently picking one.

**76. Collaborating with product, marketing, engineering teams.**
Establish shared definitions of metrics up front, communicate data limitations clearly, and build relationships so you're looped in early on decisions rather than only pulling numbers after the fact.

**77. Validating your analysis before sharing it.**
Cross-check totals against a second source, sanity-check numbers against known benchmarks, review edge cases/outliers, and where possible have a peer review the logic before it reaches stakeholders.

**78. Explaining technical/statistical concepts simply.**
Use analogies grounded in the audience's world, avoid jargon, lead with the practical implication before the mechanism, and check for understanding rather than assuming it.

**79. Staying updated with data trends/tools.**
Following relevant newsletters/communities, doing small side projects with new tools, and — in healthcare specifically — tracking changes in coding standards (ICD-10 updates), interoperability standards (FHIR), and regulatory changes (HIPAA, CMS rules).

---

## 📊 Case-Study / Scenario Questions

**80. Design an analysis to track feature adoption.**
Define "adoption" (first use vs sustained use), instrument event tracking, build a funnel from awareness → first use → repeat use, segment by user type, and track adoption rate trend over time post-launch.

**81. Evaluate marketing campaign performance.**
Define success metrics (conversion, cost per acquisition, ROI), compare against a control/baseline period or holdout group, segment by channel, and account for confounders (seasonality, concurrent campaigns).

**82. Churn/retention dashboard for SaaS.**
Include: cohort retention curves, churn rate trend, churn by segment/plan type, leading indicators of churn (usage decline), and revenue impact — with filters for time period and customer segment.

**83. Sales-performance report for a regional team.**
Show revenue/quota attainment by rep and region, trend vs prior period, pipeline health, and top drivers of variance — with drill-down from region → team → individual.

**84. Customer segmentation (high-value vs low-value).**
Use RFM or similar features (spend, frequency, engagement) and either rule-based thresholds or clustering (k-means) to group customers/patients, then validate segments make business sense and are actionable.

**85. Analyzing a sudden drop in traffic/orders.**
Check for technical issues first (tracking broken, outage), then segment the drop by channel/geography/device to isolate where it's concentrated, check for external factors (holidays, competitor actions, policy changes), and compare against historical patterns.

**86. Analyzing a pricing change or discount test.**
Compare a treatment group (new price) to a control (old price) if possible, measure impact on volume and total revenue/margin, check for cannibalization or delayed effects, and ensure sample sizes support statistical confidence.

**87. Analyzing support ticket volume and trends.**
Break down volume by category/severity over time, identify spikes and correlate with releases/incidents, track resolution time trends, and flag recurring root causes for process improvement.

**88. Designing a simple A/B test and success metrics.**
Define a single primary metric tied to the hypothesis, randomize assignment, calculate required sample size/power beforehand, run for a predetermined duration, and avoid peeking/stopping early on significance.

**89. Explaining results and next steps to a manager.**
Lead with the headline finding and business implication, support with 2-3 key visuals/numbers, be explicit about confidence/limitations, and end with a clear, actionable recommendation.

---

## 🧠 Tooling, Process & Best Practices

**90. Tools you use most often?**
Typically SQL for extraction, Python/pandas for analysis, a BI tool (Tableau/Power BI/Looker) for dashboards, Excel for quick ad-hoc work and stakeholder deliverables, and Git for version control.

**91. Versioning code and SQL (Git, folder structure)?**
Store scripts/queries in a Git repo, use meaningful commit messages, organize by project/domain, and use branches for exploratory work vs finalized production queries.

**92. Documenting queries, dashboards, assumptions?**
Maintain a data dictionary, comment complex logic inline, keep a changelog for dashboards, and note key assumptions (e.g., "excludes cancelled encounters") directly in the deliverable.

**93. Handling data privacy and PII (or PHI)?**
Apply the minimum-necessary principle, de-identify/aggregate data where possible, use role-based access controls, never export sensitive data outside approved systems, and follow relevant regulations (HIPAA in healthcare, GDPR elsewhere).

**94. Managing permissions and dashboard access?**
Use role-based or row-level security within the BI tool, restrict PHI-containing views to authorized roles only, and periodically audit who has access.

**95. Automating repetitive reports?**
Use scheduled SQL jobs/stored procedures, BI tool scheduled refresh/email subscriptions, or Python scripts run via a scheduler (cron/Airflow) to eliminate manual, repeated pulls.

**96. Ad-hoc vs recurring analyses?**
Recurring analyses should be built into automated dashboards/pipelines to save time; ad-hoc requests get a lighter-weight, faster turnaround approach, but if a pattern of similar ad-hoc requests emerges, it's a signal to build a self-service tool instead.

**97. Getting feedback on dashboards and improving them?**
Solicit direct user feedback, watch how people actually use it (which filters get used), track a "still needed?" review cadence, and iterate based on what confuses or gets ignored.

**98. Top 5 productivity habits as a data analyst?**
Templatizing recurring queries/reports, writing reusable SQL/Python functions, maintaining a personal data dictionary/glossary, always validating outputs before sharing, and blocking focus time for deep analysis vs reactive requests.

**99. Skills you want to improve in the next 6-12 months?**
Give a genuine, specific answer tied to the role — for a healthcare/comp bio/AI-adjacent role, good answers include deepening SQL performance tuning, learning more bioinformatics-specific tools, or strengthening statistical rigor for causal inference.

---

## 🧬 SUPPLEMENTARY: Healthcare Data Analyst / Computational Biology / AI Questions

### Healthcare Data & Compliance

**H1. What is PHI and how do you handle it?**
Protected Health Information — any individually identifiable health data (name, DOB, MRN, diagnoses, etc.) linked to a patient. Handle it under HIPAA's minimum-necessary standard: de-identify when possible, restrict access, never move it outside approved secure systems, and log access.

**H2. What are the 18 HIPAA identifiers, and what's the difference between the Safe Harbor and Expert Determination de-identification methods?**
Safe Harbor requires removing all 18 specific identifiers (names, dates more granular than year, geographic subdivisions smaller than state, MRNs, etc.). Expert Determination instead has a qualified statistician certify the re-identification risk is very small, allowing more nuanced retention of useful fields.

**H3. What is HL7/FHIR and why does it matter for healthcare data analysis?**
HL7 is a family of standards for exchanging clinical data; FHIR (Fast Healthcare Interoperability Resources) is the modern, API-based standard built on structured "resources" (Patient, Observation, Encounter). Understanding FHIR matters because increasingly EHR data is exposed via FHIR APIs rather than flat database exports.

**H4. What's the difference between ICD-10, CPT, and SNOMED CT codes?**
ICD-10 codes diagnoses (why a patient was seen); CPT codes procedures/services performed (billing); SNOMED CT is a much more granular clinical terminology used for detailed clinical documentation and interoperability, often mapped to ICD-10 for billing.

**H5. How would you clean and structure raw EHR data for analysis?**
Handle multiple encounters per patient, reconcile units (e.g., lab values in different scales), map free-text/coded fields to standard vocabularies, deduplicate patient records (identity resolution across systems), and carefully handle missingness that may be clinically meaningful (e.g., a test not ordered vs not resulting).

**H6. How do you calculate a readmission rate, and what pitfalls exist?**
Numerator: patients readmitted within a defined window (commonly 30 days) after discharge; denominator: total eligible discharges. Pitfalls: excluding planned readmissions, handling transfers vs new admissions correctly, and risk-adjusting for patient severity so you're not just penalizing hospitals that treat sicker patients.

**H7. What's the difference between claims data and clinical (EHR) data, and when would you use each?**
Claims data (billing records) is broad, standardized, and good for population-level utilization/cost analysis but lacks clinical detail and lags in time. EHR data is clinically rich (labs, vitals, notes) and near real-time but harder to standardize across systems and often incomplete outside the health system that generated it.

**H8. How do you handle missing lab or vital sign data in a clinical dataset?**
Distinguish "missing at random" from "missing because not clinically indicated" — the latter is informative, not noise (e.g., a test not ordered because the patient was healthy). Approaches include multiple imputation, indicator flags for missingness, or excluding it explicitly rather than blindly imputing means.

**H9. How would you measure quality of care using data?**
Use established quality measures where possible (HEDIS, CMS star ratings, or condition-specific measures like HbA1c control for diabetics), risk-adjust for patient population differences, and pair outcome measures with process measures (e.g., % of eligible patients screened) for a fuller picture.

**H10. What's a common source of bias in healthcare data, and how do you address it?**
Selection bias from who gets tested/treated (e.g., disease severity correlating with who seeks care), historical bias baked into records (documentation differences across demographic groups), and missing-not-at-random data. Address via careful cohort definition, sensitivity analysis, and being explicit about population limitations in any report.

### Computational Biology

**H11. What is a GWAS (genome-wide association study) and what statistical challenges does it involve?**
A GWAS tests millions of genetic variants (SNPs) for association with a trait/disease across many individuals. Key challenges: multiple-testing correction at massive scale (Bonferroni or FDR across millions of tests), population stratification confounding results, and needing very large sample sizes for adequate power given small individual effect sizes.

**H12. What is sequence alignment, and what's the difference between global and local alignment?**
Sequence alignment arranges DNA/RNA/protein sequences to identify regions of similarity, which can indicate functional, structural, or evolutionary relationships. Global alignment (e.g., Needleman-Wunsch) aligns entire sequences end-to-end; local alignment (e.g., Smith-Waterman, or BLAST heuristically) finds the best-matching subregions, useful when sequences share only partial similarity.

**H13. What is differential gene expression analysis?**
Comparing RNA expression levels (e.g., from RNA-seq) between conditions (e.g., tumor vs normal tissue) to find genes that are significantly up- or down-regulated, typically using tools like DESeq2 or edgeR that model count data and correct for multiple testing.

**H14. What is a p-value adjustment / multiple testing correction, and why is it critical in genomics?**
When testing thousands to millions of hypotheses simultaneously (e.g., one per gene or SNP), the chance of false positives by pure chance is very high. Bonferroni correction is conservative (divides alpha by number of tests); False Discovery Rate (Benjamini-Hochberg) is more commonly used in genomics as it controls the expected proportion of false positives among significant results while retaining more power.

**H15. What bioinformatics tools/languages are commonly used in computational biology?**
R/Bioconductor (DESeq2, limma) and Python (Biopython, scikit-bio, pandas) for analysis; command-line tools like BLAST, BWA, SAMtools, GATK for sequence alignment and variant calling; and workflow managers like Nextflow/Snakemake for reproducible pipelines.

**H16. What's the difference between genotype and phenotype data, and how do you link them analytically?**
Genotype is the genetic sequence/variant data; phenotype is the observable trait or clinical outcome. Linking them (as in GWAS or PheWAS) requires careful matching of samples, controlling for confounders like ancestry/population structure, and often uses regression models adjusting for covariates.

**H17. What is batch effect, and how do you detect/correct for it?**
Systematic, non-biological variation introduced by technical factors (different sequencing runs, reagent lots, processing dates) that can masquerade as real biological signal. Detect via PCA/clustering that separates samples by batch rather than by condition; correct using tools like ComBat or by including batch as a covariate in the statistical model.

### AI / Machine Learning in Healthcare

**H18. How do you evaluate a classification model when classes are imbalanced (e.g., predicting a rare disease)?**
Accuracy is misleading with imbalance. Use precision, recall, F1-score, and especially the precision-recall curve (more informative than ROC-AUC under heavy imbalance); consider the real-world cost of false negatives vs false positives, which in healthcare often favors optimizing recall/sensitivity for serious conditions.

**H19. Why does model interpretability matter more in healthcare/clinical AI than in many other domains?**
Clinicians and regulators need to trust and validate model reasoning before acting on it, especially for high-stakes decisions; a "black box" prediction is hard to challenge or audit, and interpretability supports clinical adoption, regulatory approval (e.g., FDA), and identifying when a model is relying on spurious/biased signals.

**H20. What is algorithmic bias in healthcare AI, and can you give an example?**
Systematic errors that disadvantage particular groups — a well-known real example is a widely-used risk-prediction algorithm that used healthcare cost as a proxy for health need, which under-predicted illness severity in Black patients because less money was historically spent on their care for the same level of need, not because they were healthier.

**H21. How would you validate a clinical prediction model before deployment?**
Use a held-out test set and ideally external validation on a different population/site, check calibration (not just discrimination/AUC), evaluate performance across subgroups for fairness, and pilot in a shadow/silent mode alongside clinical workflow before full deployment.

**H22. What's the difference between sensitivity/specificity and precision/recall, and why do both matter in a diagnostic AI context?**
Sensitivity (recall) = true positive rate among actual positives; specificity = true negative rate among actual negatives; precision = true positive rate among predicted positives. In diagnostics, high sensitivity avoids missing real cases (critical for screening), while high specificity/precision avoids overdiagnosis and unnecessary follow-up — the right balance depends on the clinical cost of each error type.

**H23. What are common data leakage pitfalls in healthcare ML models?**
Using future information not available at prediction time (e.g., a lab result drawn after the outcome event), including the target variable indirectly (e.g., a "discharge disposition" field that encodes the outcome), or splitting train/test by row instead of by patient (causing the same patient to appear in both sets).

**H24. How do you handle protected attributes (race, gender, age) when building a healthcare AI model?**
Decide deliberately whether to include them based on the use case — sometimes needed for legitimate clinical risk adjustment, sometimes a source of proxy bias. Regardless, audit model performance and error rates across these subgroups even if the attribute isn't a direct model input, since proxies (zip code, insurance type) can encode the same bias.

**H25. What's your experience (or approach) with regulatory/compliance considerations for AI in healthcare (FDA, SaMD)?**
Understand that AI/ML tools used for diagnosis or treatment decisions may be classified as Software as a Medical Device (SaMD) and require FDA clearance; document data provenance, model versioning, and performance monitoring plans, since regulators increasingly expect ongoing post-deployment monitoring for model drift, not just a one-time validation.

---

## 💡 Quick Tips for the Interview Itself
- Have 3–4 STAR stories ready that you can adapt to different behavioral questions (a data quality catch, an insight that changed a decision, a stakeholder conflict, a technical challenge).
- For healthcare-specific roles, be ready to speak fluently about **HIPAA/PHI**, **claims vs clinical data**, and at least one **quality measure** (readmissions, HEDIS) even if your prior experience isn't healthcare — read up beforehand.
- For comp-bio/AI-adjacent roles, be honest about your depth (don't overclaim genomics expertise you don't have) but show you understand the statistical rigor (multiple testing, batch effects, leakage) that translates from general data analysis.
- Always tie technical answers back to **impact**: what decision changed, what outcome improved, what risk was avoided.
