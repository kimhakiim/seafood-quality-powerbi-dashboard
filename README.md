# 🐟 Seafood Manufacturing QC/QA Dashboard

**Power BI Portfolio Project**
Quality Control & Quality Assurance Analytics for Seafood Manufacturing

---

## 📌 Project Overview

This dashboard simulates a Quality Control & Quality Assurance (QC/QA) monitoring system for a seafood manufacturing company. It was built as a portfolio project to demonstrate domain-specific data analytics skills in the fisheries and food processing industry.

The dashboard covers three core monitoring areas:

- **Production** — overall output, pass rate, and batch performance
- **Reject** — reject volume, reject type breakdown, supplier and product analysis
- **Temperature** — cold room monitoring, critical and warning readings

---

## 🗂️ Dataset Summary

| Table               | Type      | Rows   | Description                                                  |
| ------------------- | --------- | ------ | ------------------------------------------------------------ |
| `Fact_Production`   | Fact      | 10,000 | Batch-level production records                               |
| `Fact_RejectDetail` | Fact      | 25,066 | Per-inspection reject records                                |
| `Fact_Temperature`  | Fact      | 17,568 | Hourly cold room temperature readings                        |
| `Dim_Product`       | Dimension | 7      | Product catalog (Salmon Fillet, Tuna Steak, Crab Meat, etc.) |
| `Dim_Supplier`      | Dimension | 6      | Supplier master data                                         |
| `Dim_Date`          | Dimension | 365    | Date table for time intelligence                             |
| `Dim_Shift`         | Dimension | 4      | Shift master data                                            |

**Data period:** Full year 2024 (simulated data)

---

## 📊 Dashboard Pages

### 1. Production Page

Monitors overall production performance against annual targets.

**Key Metrics:**

- Total Production: 47.10M kg (94.2% of 50M/year target)
- Total Reject: 2.35M kg (+0.35M over target)
- Total Pass: 44.75M kg
- Pass Rate: 95.01% (target: 96%)
- Total Batch: 10,000 batches | Avg: 4,710.4 kg/batch

**Visuals:**

- Monthly Production & Reject Rate (combo chart: bar + line dual axis)
- Top 5 Production by Supplier
- Top 5 Production by Product

---

### 2. Reject Page

Drills down into reject patterns by type, supplier, and product.

**Key Metrics:**

- Total Reject: 2.35M kg (+0.35M over target)
- Reject Rate: 4.99% (target: 4%)
- Top Reject Type: Microbial (13.04% of total rejects)
- Reject Avg: 235.05 kg/batch (target: 200 kg/batch)
- Top Inspector: Nur Aisyah

**Visuals:**

- Monthly Reject Rate (line chart)
- Top 5 Reject Type (horizontal bar chart)
- Top 5 Reject by Supplier
- Top 5 Reject by Product

---

### 3. Temperature Page

Monitors cold room temperature compliance across 4 cold rooms (CR-01 to CR-04).

**Key Metrics:**

- Avg Temperature: 2.51°C (within safe range: -2°C to 4°C)
- Total Critical Readings: 4,589 (26.12% of total readings)
- Total Warning Readings: 5,681 (32.34% of total readings)
- Total Cold Room: 4 (CR-01, CR-02, CR-03, CR-04)
- Most Critical Room: CR-03 (1,164 critical readings)

**Visuals:**

- Avg Temp (°C) by Month (line chart)
- Status Distribution — Normal/Warning/Critical (donut chart)
- Critical & Warning by Cold Room (grouped bar chart)
- Critical & Warning by Month (dual line chart)

---

## 🔍 Key Insights

> Full insight log with recommended actions and target departments available in [`Insight_Log.xlsx`](files/Insight_Log.xlsx)

**Impact level criteria:**

- 🔴 **High** — Direct food safety or health risk
- 🟡 **Medium** — Product quality & regulatory compliance risk
- 🟢 **Low** — Cosmetic or presentation risk only

**Summary of key findings:**

| Insight ID | Page        | Finding                                                    | Impact    | Target Dept                 |
| ---------- | ----------- | ---------------------------------------------------------- | --------- | --------------------------- |
| INS-001    | Reject      | Microbial is leading reject type (13.04%)                  | 🔴 High   | QC, Production              |
| INS-002    | Reject      | September reject rate peaked at 5.11%                      | 🟡 Medium | QC, Procurement, Production |
| INS-003    | Reject      | Temperature Abuse ranked 7th but food safety risk is High  | 🔴 High   | QC, Warehouse, Maintenance  |
| INS-004    | Reject      | Ocean Harvest Ltd has highest supplier reject rate (4.99%) | 🟡 Medium | Procurement, QC             |
| INS-005    | Reject      | Crab Meat has highest product reject rate (5.07%)          | 🟡 Medium | Production, QC              |
| INS-006    | Temperature | CR-03 has highest critical readings (1,164 — 26.77%)       | 🔴 High   | Maintenance, QC, Warehouse  |
| INS-007    | Temperature | December recorded highest monthly critical rate (28.09%)   | 🔴 High   | Maintenance, Production     |
| INS-008    | Temperature | Combined non-normal readings exceed 58% of total           | 🔴 High   | Maintenance, QC, Management |
| INS-009    | Production  | Pass rate 95.01% — 0.99% below 96% target                  | 🟡 Medium | Production, QC, Management  |
| INS-010    | Production  | Reject volume +0.35M kg above annual target                | 🟡 Medium | QC, Production, Management  |

---

## ⚠️ Data Quality Issue Log

> Data quality issues identified during ETL process in Power Query. Full log available in [`Issue_Log.xlsx`](files/Insight_Log.xlsx)

| Issue ID | Table             | Column                 | Issue                                               | Rows Affected | Magnitude | Solvable | Resolution                                             |
| -------- | ----------------- | ---------------------- | --------------------------------------------------- | ------------- | --------- | -------- | ------------------------------------------------------ |
| ISS-001  | Fact_Production   | ReceivedDate           | Inconsistent date format (ISO, US, local styles)    | —             | —         | Yes      | Standardized via Power Query custom parsing            |
| ISS-002  | Fact_Production   | Quantity_kg            | Inconsistent numeric format (text suffix "kg")      | —             | —         | Yes      | Removed suffix, converted to decimal                   |
| ISS-003  | Fact_Production   | Reject_kg              | Missing measure values (blank reject quantity)      | 4             | 0.04%     | No       | Retained as raw issue pending stakeholder confirmation |
| ISS-004  | Fact_Production   | Pass_kg                | Inconsistent numeric format (decimal separator)     | 2,511         | 25.1%     | Yes      | Replaced decimal separator, converted to decimal       |
| ISS-005  | Fact_Production   | ProcessingTime_minutes | Missing measure values (blank processing time)      | 3             | 0.03%     | No       | Retained as raw issue pending stakeholder confirmation |
| ISS-006  | Fact_RejectDetail | InspectionDate         | Inconsistent date format (multiple formats)         | —             | —         | Yes      | Standardized via Power Query locale fallback           |
| ISS-007  | Fact_RejectDetail | RejectType             | Missing categorical values (blank reject type)      | 148           | 1.0%      | No       | Retained as raw issue for stakeholder review           |
| ISS-008  | Fact_RejectDetail | Inspector              | Missing descriptive values (blank inspector name)   | 296           | 2.0%      | No       | Retained as raw issue for stakeholder review           |
| ISS-009  | Fact_Temperature  | ReadingDate            | Inconsistent date format (multiple formats)         | —             | —         | Yes      | Standardized via Power Query locale fallback           |
| ISS-010  | Fact_Temperature  | Temperature_C          | Missing measure values (blank temperature readings) | 87            | 1.0%      | No       | Retained as raw issue pending confirmation             |

---

## 🛠️ Technical Details

**Tools**
• Excel
• Power Query
• Power BI Desktop
• Figma
• GitHub

**Data Model:** Star schema — 3 Fact tables, 4 Dimension tables

**Key Technical Highlights:**

- Star schema data model (3 Fact + 4 Dimension tables)
- Dynamic target calculation that scales with filter context
- Conditional color formatting for KPI performance indicators
- Cross-filter interaction management for accurate KPI display
- Built with reusable Power Query transformation steps

**Design Decisions:**

- Color system: Blue (#4FC3F7) = production metrics, Orange (#FF8C00) = reject metrics
- Pie chart replaced with horizontal bar chart for reject type to avoid misleading % when Top N filter is applied
- Cross-filter interaction disabled between Status Distribution donut chart and KPI cards to prevent misleading context when drilling into status categories

---

## 👤 About

Built by **[Abdurrachman Hakim]** as part of a career pivot into data analytics, leveraging domain expertise in seafood/fisheries and a background in data annotation.

[LinkedIn](https://linkedin.com/in/kimhakiim)
[Threads] (https://www.threads.com/@kimhakiim)
[Github] (https://github.com/kimhakiim)

---

_This is a portfolio project using simulated data. All company names, supplier names, and inspector names are fictional._
