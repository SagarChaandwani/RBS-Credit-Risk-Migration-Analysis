 RBS-Credit-Risk-Migration-Analysis

> *Disclaimer: This is a personal portfolio project using a synthetic dataset. It is a simulation of real-world banking challenges and is not affiliated with the NatWest Group.*

# 🏦 RBS Strategic Portfolio Monitor
### Credit Risk Exposure & Digital Channel Migration (2023-2025)

---

### 📋 Project Specification
| **Domain** | **Retail Banking / Risk / Strategy** |
| :--- | :--- |
| **Tech Stack** | `SQL Server` `Power BI` `DAX` `Star Schema` |
| **Focus** | Non-Performing Loans (NPL) & Competitor Analysis |
| **Status** | ✅ Complete (v1.0) |

---

> **🚀 Executive Brief:** A strategic deep-dive into the "Perfect Storm" facing UK Retail Banking. This analysis quantifies the correlation between macroeconomic inflation, rising mortgage default rates, and the "Silent Migration" of capital to fintech competitors.

---

## 📖 Client Background & Context

The Royal Bank of Scotland (RBS) is a historic British financial institution and a key subsidiary of the NatWest Group. As a legacy bank transitioning into the digital era, it manages vast datasets across mortgages, credit cards, and current accounts.

### 📉 Market Context: The "Perfect Storm"
RBS is navigating a critical transition period. Post-pandemic inflation has squeezed household disposable income, leading to higher default rates on mortgages. Simultaneously, digital-first "Challenger Banks" (**Monzo, Revolut**) are aggressively acquiring market share from legacy banks, specifically targeting the profitable "Young Professional" demographic.

### ⚠️ The Business Problem
RBS leadership identified two critical financial bleeds that required immediate data intelligence:

1.  **Asset Quality (Credit Risk):** A rising concentration of **Non-Performing Loans (NPLs)** in the North West region, driven by variable-rate exposure.
2.  **Share of Wallet (Digital Churn):** A "Silent Migration" where customers retain RBS mortgages (high friction to switch) but move their primary daily transaction activity to competitors (low friction).



## 📑 Table of Contents
1. [Executive Summary & Impact](#-executive-summary--quantified-impact)
2. [Key Business Questions Solved](#-key-business-questions-solved)
3. [Data Structure (Star Schema)](#-data-structure--modelling)
4. [Dashboard Deep Dive](#-dashboard-deep-dive)
5. [Strategic Recommendations](#-strategic-recommendations)
6. [Assumptions & Future Scope](#-assumptions--future-scope)
7. [Technical Implementation](#-technical-implementation--expertise)
8. [Data Dictionary](#-data-dictionary)


---

## 🏆 Executive Summary & Quantified Impact
This longitudinal analysis of **£199M** in portfolio exposure across **1,999 customers** reveals a dual-threat scenario peaking in 2025.

*   **Risk Mitigation:** Enabled the Retail Risk team to reduce exposure by targeting the **28.6% "High-Risk" portfolio segment** (defined as LTV > 90% and Credit Score < 650).
*   **Revenue Recovery:** Identified a **22.3% Digital Churn Rate** and provided data-backed recommendations to recover **£15M** in lost annual transaction volume.
*   **Regional Hotspot:** Pinpointed the **North West** region as the epicenter for **45%** of new defaults.

---

## 🎯 Key Business Questions Solved
*   **Where is our credit risk concentrated?**
    *   Identified the **North West** region as the epicenter for **45%** of new defaults, driven primarily by Mortgage holders.
*   **Why are customers leaving for challenger banks?**
    *   Pinpointed the **"Young Professional"** and **"Family"** segments as the primary source of capital migrating to **Monzo** and **Revolut**.
*   **What is the 'Sticky Product' vs. 'Daily Spend' correlation?**
    *   Quantified that only **11%** of customers remain true "RBS Loyalists," showing high churn in primary transaction activity despite retaining large mortgage balances.

---

## 🗂 Data Structure & Modelling
To enable high-performance filtering over 2.5 million transaction rows, I designed a **Star Schema** data model in Power BI.

*   **`Monthly_Activity_R` (Fact Table):** The core transactional table containing monthly spend, missed payment flags, and primary banking app usage.
*   **`Customer_R` (Dimension):** Static demographics including Age, Region, Segment, and Credit Score.
*   **`Loan_R` (Dimension):** Loan specifics including LTV Ratio, Product Type (Mortgage, Credit Card), and Origination Date.
*   **`waterfall_categories` (Helper):** A disconnected table used to structure the financial "Walk" chart.

---

## 🖼️ Dashboard Deep Dive

### 1. Executive Home: The Strategic Pulse
A high-level health check of the £199M portfolio designed for the C-Suite.
![Executive Dashboard](Screenshots/1.%20RBS%20Strategic%20Monitor.png)
*   **KPI Ticker:** Immediate visibility on Total Exposure, Active Customers, and the alarming 28.6% Risk Rate.
*   **Primary Bank Market Share (Ribbon Chart):** Visualizes the loss of market dominance. The **Blue Band (RBS)** narrows significantly from 2023 to 2025, while **Orange (Monzo)** expands, indicating a loss of "Share of Wallet."
*   **Missed Payment Trend (Line Chart):** A time-series analysis showing the volatility of financial stress. The upward trend in 2024-2025 correlates with the inflation crisis.

### 2. Credit Risk Audit: The "Forensic" View
Deep-dive into High-LTV mortgages using a Quadrant Analysis.
![Risk Dashboard](Screenshots/2.%20Credit%20Risk%20Audit.png)
*   **Risk Quadrant (Scatter Plot):** A custom quadrant analysis using DAX.
    *   **Navy Dots:** Safe customers.
    *   **Orange Dots (The "Danger Zone"):** Customers with **LTV > 90%** AND **Credit Score < 650**. These are the priority targets for intervention.
*   **Portfolio Value Walk (Waterfall):** Reconciles the loan book movement. It visually proves that despite new business (Green), the portfolio growth is being stifled by stalled repayments (Red) and write-offs.

### 3. Digital Migration
Tracking the flow of capital from RBS to Competitors.
![Migration Dashboard](Screenshots/3.%20Digital%20Migration%20and%20Competitors.png)
*   **Capital Flight Analysis (Decomposition Tree):** A Root Cause Analysis of the £818K leaving RBS.
    *   **Insight:** The majority flows to **Monzo**, driven specifically by the **"Family"** and **"Young Professional"** segments.
*   **The Retention Funnel:** A stark look at loyalty. Out of 1999 customers, only **220 (11%)** remain "RBS Loyalists" (using RBS for primary banking) in 2025.

---

## 💡 Strategic Recommendations

### 1. Risk Mitigation: The "Mortgage Holiday"
*   **Finding:** 28.6% of the portfolio is in the "High Risk" quadrant (Orange Zone).
*   **Recommendation:** Launch a **"Mortgage Holiday" Scheme** specifically for North West customers with LTV > 90%.
*   **Impact:** Offer a 3-month interest-only period to alleviate short-term cash flow pressure, preventing full default.

### 2. Product Strategy: Closing the Feature Gap
*   **Finding:** Young Professionals are moving money to Monzo for "Savings Pots" and "Instant Notifications."
*   **Recommendation:** Fast-track the **"RBS Spaces" (Savings Pots)** feature sprint.
*   **Impact:** Strategy targeted to close the feature gap driving migration, aiming to recover the **£15M annual transaction loss**.

---
## 🧠 Assumptions & Future Scope

### Assumptions made during analysis:
1.  **Risk Definition:** "High Risk" is strictly defined as LTV > 90% AND Credit Score < 650. In a live environment, this would be adjusted based on the bank's Risk Appetite Framework.
2.  **Churn Definition:** A customer is considered "Churned" if their `Primary_Bank` changes from RBS to a competitor for 3 consecutive months.
3.  **Waterfall Calculation:** Due to archived transaction logs, Repayments were calculated as the residual value of `(Open + New - WriteOffs) - Closing Balance`.

### Future Scope (Next Steps):
*   **Predictive Modelling:** Implement a Machine Learning model to predict *which* specific customers will churn in the next 30 days based on transaction behavior.
*   **Real-time Alerts:** Set up Power BI Data Alerts to notify Relationship Managers immediately when a High-Net-Worth client misses a payment.


---
## 🛠 Technical Implementation & Expertise

### Advanced DAX
*   **Risk Quadrant Logic:** Dynamic coloring of scatter points based on selected LTV and Credit Score thresholds.
*   **Waterfall Logic:** Implemented a complex `SWITCH` measure to map disconnected table categories (Opening, New, Repayments, Closing) into a financial walk.
*   **Churn Analysis:** Used `KEEPFILTERS` to calculate outbound money flow while excluding internal transfers.

### SQL Validation
SQL was used to validate data integrity (checking for NULL LTVs), standardize region names, and bucket risk tiers before importing into Power BI.

---

## 📚 Data Dictionary
*A reference guide to the core tables used in the Star Schema.*

| Table | Column | Description |
| :--- | :--- | :--- |
| **Loan_R** | `LTV_Ratio` | Loan-to-Value ratio. Calculated as `Loan Amount / Collateral Value`. Used for Risk Tiering. |
| **Loan_R** | `Product_Type` | Category of lending product (Mortgage, Personal Loan, Credit Card). |
| **Customer_R** | `Credit_Score` | FICO-equivalent score (300-850). Scores < 650 are flagged as sub-prime. |
| **Monthly_Activity_R** | `Missed_Pay` | Binary Flag (1 = Missed Payment, 0 = Paid). Used to calculate volatility. |
| **Monthly_Activity_R** | `Estimated_Spend` | Monthly transaction volume derived from customer segment profiles. |
| **Monthly_Activity_R** | `Primary_Bank` | The bank app opened most frequently by the customer in a given month. |

---
