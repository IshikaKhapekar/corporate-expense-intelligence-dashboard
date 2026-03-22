# 🧳 Travel Expense Analytics Dashboard
### FP&A Reporting Project | Data Cleaning + Power BI Dashboard


---

## 📌 Project Overview

This project simulates the Financial Planning & Analysis (FP&A) reporting function 
of a global consulting firm's finance team. It covers the complete data analytics 
workflow — from raw data ingestion and cleaning to an executive-level Power BI dashboard 
tracking travel reimbursements, budget variances, and compliance across departments 
and regions.

**Business Problem:**
A consulting firm with 124 employees across 6 departments and 5 global regions needed 
a centralized system to track travel expenses, identify budget overruns, flag anomalous 
transactions, and report to senior management on a monthly basis.

**Solution Built:**
An end-to-end FP&A reporting system with automated variance flagging, 
multi-dimensional analysis, and a 2-page interactive Power BI dashboard.

---

## 📊 Dataset

| Detail | Info |
|---|---|
| Source | Kaggle — Employee Travel Reimbursements |
| Link | [Dataset Link](https://www.kaggle.com/datasets/legrande/employee-travel-im-bursements) |
| Raw Rows | 3,974 |
| Employees | 124 |
| Unique Trips | 1,001 |
| Date Range | January 2023 — December 2024 |
| Regions | APAC, EMEA, Americas, South Asia, Middle East |
| Departments | HR, Finance, Sales, Operations, IT, Marketing |

**Original Columns:**
LineID, BusinessTripID, LineItemID, EmployeeID, StartCity, StartState, 
EndCity, EndState, ReportedDistance, ValidatedDistance

**Added Columns:**
Department, FullName, Region, ExpenseCategory, TripDate, Month, Quarter,
ReimbursementAmount, BudgetAmount, Variance, Variance%, ApprovalStatus,
AnnualSpendFlag, DistanceMismatch, ZeroAmountFlag, DataQuality

---

## 🧹 Data Cleaning Process

Complete audit trail of all cleaning steps performed.

### Cleaning Summary

| Check | Total Rows | Issues Found | Issues Fixed | Impact % |
|---|---|---|---|---|
| Duplicates | 3,974 | 0 | 0 | 0.00% |
| Missing Values | 3,974 | 5 | 0 | 0.13% |
| Distance Mismatch | 3,974 | 73 | 0 | 1.84% |
| Zero Amount | 3,974 | 5 | 0 | 0.13% |
| Over Limit Employees | 3,974 | 144 | 0 | 3.62% |
| Flagged Transactions | 3,974 | 707 | 0 | 17.80% |
| CLEAN Rows | 3,974 | 3,896 | — | 98.04% |
| REVIEW Rows | 3,974 | 78 | — | 1.96% |

### Step 1 — Duplicate Detection
Checked on LineID (unique identifier) and BusinessTripID + LineItemID combination.
Result: 0 duplicates found.

<img width="1872" height="639" alt="Screenshot 2026-03-23 023009" src="https://github.com/user-attachments/assets/a6d0a524-3643-444c-a399-6c6e4776f020" />

### Step 2 — Missing Values Treatment
Checked EmployeeID, ReimbursementAmount, TripDate, Department columns for blanks.
Result: 5 rows with zero amount — flagged in ZeroAmountFlag column.

### Step 3 — Distance Mismatch Validation
Formula used:
```excel
=IF(ABS(ReportedDistance - ValidatedDistance) > 10, "Review", "OK")
```
Result: 73 trips flagged where reported distance differed from validated 
distance by more than 10 miles — potential reimbursement fraud risk.

<img width="251" height="647" alt="Screenshot 2026-03-23 023201" src="https://github.com/user-attachments/assets/360c0179-64f3-4e26-827e-a34fc161aeb4" />

### Step 4 — Variance Flagging
Formula used:
```excel
=IF(Variance% > 0.15, "Flagged", "Approved")
```
Result: 707 transactions (17.8%) flagged for exceeding 15% budget variance.

### Step 5 — Annual Budget Limit Check
Formula used:
```excel
=IF(SUMIF(EmployeeID column, current ID, ReimbursementAmount column) > 
TravelBudgetLimit, "Over Limit", "Within Limit")
```
Result: 144 employees (3.6%) exceeded their annual travel budget limit.

<img width="251" height="647" alt="Screenshot 2026-03-23 023201" src="https://github.com/user-attachments/assets/8d607a45-8cb4-4e42-8130-600d94ee3846" />

### Step 6 — Data Quality Master Flag
Single column summarizing all issues:
```excel
=IF(OR(EmployeeID="", Amount=0, Distance=0, DistanceMismatch="Review"),
"REVIEW", "CLEAN")
```
Result: 3,896 rows (98%) marked CLEAN. 78 rows flagged for REVIEW.

---

## 📈 Power BI Dashboard

### Page 1 — Expense Dashboard
Executive overview of all travel expenses across departments, 
regions and time periods.

<img width="1048" height="590" alt="Screenshot 2026-03-23 022140" src="https://github.com/user-attachments/assets/93873fb7-2e6a-4563-a961-98ee33275182" />

**Visuals included:**
- 5 KPI Cards: Total Actual Spend, Total Budget, Total Variance, Overall Variance%, Total Trips
- Line Chart: Monthly Expense vs Budget Trend (2023–2024)
- Clustered Bar Chart: Department Budget vs Actuals
- Donut Chart: Expense split by Category (Flight, Cab, Train, Car Rental, Hotel)
- Matrix Table: Department Spend by Quarter with conditional formatting
- Slicers: Quarter, Department, Region

---

### Page 2 — Anomaly & Flagged Report
Drill-down view of all flagged and anomalous transactions 
for finance team investigation.

<img width="1052" height="587" alt="Screenshot 2026-03-23 022209" src="https://github.com/user-attachments/assets/6cc10289-0874-4edf-8ee2-3257c384dcd3" />

**Visuals included:**
- 3 KPI Cards: Compliance Rate (82.09%), Over Limit Employees (114), Flagged Trips (707)
- Bar Chart: Flagged Trips by Department
- Bar Chart: Variance by Region (Red = Over Budget, Green = Under Budget)
- Detailed Table: All flagged transactions with FullName, Department, Region, 
  ExpenseCategory, Actual Spend, Budget, Variance%, ApprovalStatus
- Slicers: ApprovalStatus, AnnualSpendFlag, ExpenseCategory

---

## 💡 Key Findings

1. **IT department** recorded the highest travel spend at $12,337 — 
   exceeding budget by the largest margin across all departments.

2. **707 transactions (17.8%)** exceeded budget variance threshold of 15% — 
   highest concentration in Marketing and Sales departments.

3. **73 trips (1.84%)** show significant distance mismatch between reported 
   and validated distance — flagged for potential reimbursement fraud audit.

4. **South Asia and Americas** regions recorded highest positive variance — 
   recommend regional travel caps for next fiscal year.

5. **144 employees (3.6%)** exceeded annual travel budget limit — 
   immediate manager review and travel freeze recommended.

---

## 📋 CFO Recommendations

1. Introduce pre-approval workflow for trips where estimated cost exceeds $500
2. Negotiate corporate rates with cab and car rental vendors to reduce Category spend
3. Cap flight bookings to economy class for trips under 4 hours duration
4. Implement monthly travel budget review meetings with department heads
5. Launch audit of 73 distance-mismatch trips for potential fraud investigation

---

## 🛠️ Tools Used

| Tool | Purpose |
|---|---|
| Microsoft Excel | Data cleaning, formula building |
| Power BI Desktop | Dashboard design, DAX measures, interactive visuals |
| GitHub | Version control and project documentation |
| Kaggle | Dataset source |

---

## 👤 Author

Ishika Khapekar
Aspiring Data Analyst 
linkedin: https://www.linkedin.com/in/ishika-khapekar-a068182ab

---

*This project was built as part of my data analytics portfolio targeting 
FP&A and financial reporting roles in consulting and professional services firms.*
```

---

## 🎯 Skills Demonstrated

| Skill | Details |
|---|---|
| Data Cleaning | Duplicate detection, missing value treatment, distance validation |
| Excel Analysis | VLOOKUP, SUMIF, COUNTIF, CHOOSE, conditional formatting |
| DAX Measures | SUM, DIVIDE, COUNTROWS, FILTER, DISTINCTCOUNT |
| Power BI | Relationships, measures, slicers, conditional formatting, matrix |
| FP&A Concepts | Budget vs actuals, variance analysis, compliance rate, AR aging |
| Business Communication | CFO executive summary, audit trail, anomaly reporting |

