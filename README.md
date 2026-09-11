# Illinois Enrollment Demographics Dashboard

A Tableau project analyzing longitudinal enrollment patterns across student level, race/ethnicity, and gender using publicly available University of Illinois enrollment statistics.

## 1. Goal and Context

Institutional enrollment data are often published as large tables that are difficult to interpret quickly. The goal of this project is to turn those historical counts into an interactive visual system that helps answer:

> How has the composition of Illinois enrollment changed over time, and which demographic groups gained or lost share or rank?

The project focuses on **trend analysis, composition, and demographic comparison**, not prediction.

## 2. Data

The primary Illinois workbook (`Atlas intern/IL.twb`) connects to historical enrollment data with fields including:

- Fall Term
- Student Level
- Caucasian
- Asian American
- African American
- Hispanic
- Native American
- Hawaiian / Pacific Islander
- Multiracial
- International
- Unknown

The workbook also uses reshaped fields such as `Year`, `Ethnicity`, and `counts` for longitudinal demographic analysis.

## 3. Data Transformation

A wide historical table can be represented as

$$
X_{t,g}=\text{enrollment count for demographic group }g\text{ in year }t.
$$

For visualization, it is often more convenient to reshape this into long form:

$$
(t,g,X_{t,g}),
$$

so that `Year` and `Ethnicity` become dimensions and `counts` becomes the measure.

This structure supports stacked areas, trend lines, percentage-of-total calculations, and rank-order views.

## 4. Core Tableau Calculations

The workbook includes a FIXED level-of-detail expression:

```tableau
{ FIXED [Year]: SUM([counts]) }
```

Mathematically, this is the total enrollment for year $t$:

$$
T_t=\sum_g X_{t,g}.
$$

The workbook then computes demographic share using

```tableau
SUM([counts]) / SUM([Calculation1])
```

which corresponds to

$$
S_{t,g}=\frac{X_{t,g}}{T_t}.
$$

This distinction is important: absolute counts answer **how many students**, while shares answer **how the composition changed**.

## 5. Dashboard Views

The primary workbook contains views including:

- Illinois Undergraduate Enrollment — Absolute Values
- Illinois Undergraduate Enrollment — Percentage
- Illinois Undergraduate Enrollment — Rank Order Chart
- Illinois Undergraduate Enrollment — Stacked Area Chart
- State of Illinois Gender Trend
- Illinois Total Gender Trend

These views address complementary questions rather than repeating the same metric.

## 6. Why Use Both Counts and Shares?

Suppose a group grows from 1,000 to 1,500 students while total enrollment grows from 10,000 to 20,000.

Its absolute count increases by

$$
\frac{1500-1000}{1000}=50\%,
$$

but its enrollment share falls from

$$
\frac{1000}{10000}=10\%
$$

to

$$
\frac{1500}{20000}=7.5\%.
$$

Looking only at counts would suggest growth; looking at share reveals a decline in relative representation. This is why the dashboard uses both measures.

## 7. Rank-Order Analysis

For each year $t$, demographic groups can be ranked by enrollment count or share:

$$
\text{Rank}_{t,g}
=
\operatorname{rank}\left(-X_{t,g}\right).
$$

A lower numerical rank corresponds to a larger group. Tracking rank over time makes it easy to see when groups overtake one another even if all groups are growing in absolute terms.

## 8. Stacked Area Interpretation

A stacked area chart uses the decomposition

$$
T_t=\sum_g X_{t,g}
$$

to show both total scale and group contribution. When normalized to 100%, the same view emphasizes

$$
\sum_g S_{t,g}=1.
$$

The first emphasizes growth in total enrollment; the second emphasizes compositional change.

## 9. Evaluation / Quality Checks

This is a descriptive analytics project, so there is no train/test split or predictive accuracy metric. Evaluation focuses on consistency of the aggregation logic.

For each year:

$$
T_t=\sum_g X_{t,g}
$$

and, when all demographic groups are included,

$$
\sum_g S_{t,g}=1.
$$

Useful checks include:

- year totals reconcile to the sum of demographic counts
- percentages remain in $[0,1]$
- rank order matches the displayed counts or shares
- filters use the same underlying population across dashboard views
- longitudinal views do not mix incompatible definitions across years

## 10. Results and Interpretation

The completed Tableau workbook demonstrates how enrollment can be examined from several perspectives: absolute scale, demographic share, rank order, gender trends, and longitudinal composition.

The repository confirms the exact LOD logic used for annual totals and percentage calculations. It does not contain a verified plain-text export of all final dashboard values, so this README does not invent numerical trend claims that cannot be checked directly from the repository.

The key analytical takeaway is that **institutional growth and demographic representation are different quantities**. A group can increase in headcount while losing share, or remain stable in count while changing rank. Using counts, shares, ranks, and trend views together provides a more complete interpretation.

## 11. Data Source

The analysis uses publicly available University of Illinois enrollment statistics published through institutional research resources, including UIUC Data & Analytics for Institutional Research (DAIR) and historical enrollment reports.

Because the workbook was created from a local downloaded copy, a local source path appears in Tableau metadata. The analysis itself is based on public aggregate institutional data rather than confidential student-level records.

## 12. Repository Structure

```text
.
├── README.md
└── Atlas intern/
    ├── IL.twb
    ├── Fall enrollments by race ethnicity and level new.xls
    └── additional development workbooks / spreadsheets
```

## 13. Tools & Skills

Tableau · LOD Expressions · Demographic Analysis · Longitudinal Analysis · Ranking · Percentage-of-Total Analysis · Institutional Analytics · Data Visualization

## 14. Limitations

- This project is descriptive, not causal.
- Historical demographic categories may change in meaning or reporting practice over time.
- Exact dashboard values are not duplicated in text unless they can be verified from repository artifacts.
- Comparisons across years should be interpreted with attention to changes in institutional definitions and data collection.