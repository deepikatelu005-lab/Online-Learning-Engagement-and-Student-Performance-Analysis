# Online Learning Engagement and Student Performance Analysis

An interactive data analytics solution built with **Microsoft Power BI**, **Power Query**, **DAX**, and **SQL** to evaluate how digital behavior, prior education, and demographics influence academic outcomes and student retention.
Project Overview:
Online learning platforms record massive volumes of student interaction data, yet institutions often struggle to turn these logs into proactive student support. This project builds a complete business intelligence framework to track 32,593 raw student records across 14 data attributes. The final data pipeline delivers a multi-page interactive dashboard optimizing core student support metrics and identifying at-risk cohorts early.

Domain:** Education Analytics / E-Learning
* **Analysis Population:** 22,422 students (post-cleaning and filtration)
* **Tools Used:** Microsoft Power BI Desktop, Power Query, DAX, SQL Server

---

## 📈 Key Outcomes & KPIs
The final dashboard tracks five core Key Performance Indicators (KPIs) with the following baseline results:

| KPI | Value | Description |
| :--- | :--- | :--- |
| **Total Students** | 22,422 | Unique enrolled student population analyzed |
| **Pass Rate** | 57.64% | Overall completion rate across all cohorts |
| **Dropout Rate** | 20.00% | Percentage of students flagged as dropped out or withdrawn |
| **Average Score** | 73.00 | Average assessment score achieved out of 100 |
| **Average Clicks** | 1.73K | Mean Virtual Learning Environment (VLE) interaction logs per student |

---

## 🎯 Objectives
* **Behavioral Analysis:** Map the direct correlation between online platform activity (VLE clickstream logs) and student test scores.
* **Demographic Breakdown:** Highlight performance gaps by geographical region, prior educational background, gender, and study workload.
* **Early Retention Signals:** Segment dropout paths to isolate and highlight at-risk learners for early interventions.
* **Data Rectification:** Engineer an enterprise-grade data model by resolving high volumes of missing logs, structural errors, and duplicate data rows.

---

## 🛠️ Data Pipeline & Preprocessing
Data cleaning and ETL transformations were handled within **Power Query** to enforce data integrity before data modeling:

### 1. Data Cleaning & Imputation
* **Engagement & Activity Logs:** Missing values inside `total clicks` were imputed with `0`, and empty `engagement_level` records mapped to `"None"`.
* **Academic Scores:** Empty `avg_score` fields were filled using course-level student median metrics, and corresponding `performance_level` missing values mapped to `"Unassessed"`.
* **Socioeconomic Data:** Missing entries within the Index of Multiple Deprivation (`sideband`) were cataloged under a new distinct `"Unknown"` category.
* **Deduplication:** Dropped **3,808 duplicate student IDs** (`id_student`) across course modules to enforce distinct student entities.

### 2. Schema Transformations & Filtering
* **Workload Constraint:** Excluded test records where `studied_credits` fell below 30 credits.
* **Data Type Rectification:** `id_student` cast to Text format to block automated numeric aggregations. `pass_flag` and `dropout_flag` converted to strict binary Booleans (`1/0`).
* **Column Splitting & Merging:** Split categorical string ranges inside `sideband` into numerical boundaries (`imd_min_pct` and `imd_max_pct`). Consolidated `gender` and `highest_education` into a new structural dimensional cross-key called `Demographic_Segment`.

---

## 🗄️ Data Modeling
The preprocessed tables are structured into an optimized relational **Star Schema** to enable high-performance DAX calculations and cross-visual slicing:

* **Fact Table:** `Fact_StudentPerformance`
* **Dimension Tables:** `Dim_Student`, `Dim_Demographics`, and `Dim_Region`
* **Relationships:** \(1:N\) (one-to-many) single-direction relational paths propagating filters from dimensions down to facts.

### Calculated Columns & Measures Created
1. `click_per_credit` (Calculated Column): `total clicks / studied_credits`
2. `Custom risk score` (Calculated Column): Specialized logic mapping risk boundaries
3. Core DAX Measures: Scalable metrics tracking `Total Students`, `Average Score`, `Average Clicks`, `Pass Rate`, and `Dropout Rate`.

---

## 💡 Key Business Insights
* **Platform Engagement Drives Success:** High-engagement students averaged a score of ~78 compared to ~67 for low-engagement profiles. Over 67% of high-engagement profiles safely completed their modules with a significant sub-cohort achieving Distinction.
* **Prior Education and At-Risk Risks:** Students entering with "No Formal Qualifications" exhibited the lowest completion rates (42% Pass Rate) and the highest attrition risks (25% Dropout Rate). 
* **High Concentration of Risk:** Roughly **47.7% of the total student body** maps into either High Risk or Very High Risk flags, emphasizing a major opening for targeted academic tracking dashboards.
* **Regional Uniformity:** Enrolment counts are led heavily by Scotland (~2.5K), whereas regions like Northern Ireland and North Region host lower densities (~0.7K). Despite distribution variances, average performance remains balanced between a tight band of 71 to 75 across all boundaries.

---

## 🚀 Future Scope
* **Predictive Frameworks:** Integrate machine learning classification files to compute live dropout probabilities before the conclusion of academic tracks.
* **Early Alert Integration:** Hook conditional telemetry triggers to text/email alerts targeting disengaged or down-trending click streams.
* **Real-Time Data Refresh:** Establish automated scheduled gateways bridging live LMS databases directly to the Power BI Cloud Service.
