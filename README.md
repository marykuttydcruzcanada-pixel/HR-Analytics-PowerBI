HR Analytics Dashboard | Power BI & DAX
![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-0078D4?style=for-the-badge&logo=microsoft&logoColor=white)
![Power Query](https://img.shields.io/badge/Power%20Query-217346?style=for-the-badge&logo=microsoft&logoColor=white)
---
Project Summary
An end-to-end HR analytics dashboard built in Power BI, analyzing 2,500+ employee records to surface attrition risks, engagement gaps, and satisfaction trends — giving HR leadership the visibility to act before they lose people, not after.
---
Business Questions Answered
What is the overall attrition rate, and which departments are losing the most people?
How do engagement and satisfaction scores vary across teams — and where are they falling below company average?
What are the monthly termination trends, and are things getting better or worse over time?
---
Tools & Techniques
Area	Details
Visualization	Power BI Desktop
Data Modeling	Star schema — Fact table + dimension tables
Time Intelligence	Custom Calendar table with DAX
Data Cleaning	Power Query (M language)
Measures	DAX KPIs — attrition rate, engagement score, satisfaction score
---
Data Model
Built on star schema principles:
Fact Table: Employee records (headcount, terminations, survey scores)
Dimension Tables: Department, Date (custom Calendar), Job Role, Location
Relationships: All dimensions connected to fact table via single-direction relationships
A custom Calendar table was created using DAX to enable time-intelligence functions like `DATEADD`, `TOTALYTD`, and monthly termination tracking.
```dax
Calendar =
ADDCOLUMNS(
  CALENDAR(MIN(Employees[HireDate]), MAX(Employees[TerminationDate])),
  "Year", YEAR([Date]),
  "Month", MONTH([Date]),
  "Month Name", FORMAT([Date], "MMMM"),
  "Quarter", "Q" & QUARTER([Date])
)
```
---
Key DAX Measures
```dax
-- Attrition Rate
Attrition Rate =
DIVIDE(
  CALCULATE(COUNTROWS(Employees), Employees[Status] = "Terminated"),
  COUNTROWS(Employees)
)

-- Engagement Score (Average)
Avg Engagement Score = AVERAGE(Employees[EngagementScore])

-- MoM Termination Change
MoM Termination Change % =
VAR Current = CALCULATE([Total Terminations], DATESMTD(Calendar[Date]))
VAR Previous = CALCULATE([Total Terminations], DATEADD(Calendar[Date], -1, MONTH))
RETURN DIVIDE(Current - Previous, Previous)
```
---
Dashboard Pages
Page 1 — Executive Overview
KPI cards: Engagement (58%), Satisfaction (62%), Attrition Rate
Trend line: Monthly terminations over time
Bar chart: Headcount and attrition by department
Page 2 — Engagement & Satisfaction Analysis
Department-level heatmap of engagement vs. satisfaction scores
Flagged departments performing 5–10% below company average
Scatter plot: Engagement vs. tenure
Page 3 — Attrition Deep Dive
Breakdown by department, job role, and tenure band
Monthly termination trend with year-over-year comparison
Top contributing factors to attrition
---
Key Findings
Overall attrition rate reveals departments with the highest turnover risk
Engagement at 58% and satisfaction at 62% are below healthy benchmarks (typically 70%+), flagging organization-wide retention risk
3 departments identified performing 5–10% below company average on both metrics — providing HR with clear, targeted intervention priorities
Monthly termination tracking via time-intelligence reveals whether attrition is trending up or stabilizing
---
Screenshots
> *(Add your dashboard screenshots here)*
> Recommended: Overview page, Department breakdown, Attrition trend page
---
How to Run This Project
Download the `.pbix` file from this repository
Open with Power BI Desktop (free download at powerbi.microsoft.com)
The dataset is embedded — no external connection needed
Use the slicers on each page to explore by department, year, or job role
---
What I Learned
Designing a star schema from a flat HR dataset — separating facts from dimensions
Writing time-intelligence DAX using a custom Calendar table
Translating raw survey scores into actionable KPI narratives for a non-technical audience
Using conditional formatting in Power BI to flag underperforming departments visually
---
Built by Marykutty Dcruz | LinkedIn | Richmond Hill, ON
