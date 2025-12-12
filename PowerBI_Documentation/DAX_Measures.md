Below are the exact DAX formulas used in the Power BI model, referenced to the specific tables (`Loan_R`, `Customer_R`, `Monthly_Activity_R`).  
  
## 1. Credit Risk Quadrant (Scatter Plot)  
**Purpose:** Dynamically colors the Scatter Plot to isolate high-risk customers based on LTV > 90% and Credit Score < 650.  
```dax  
Scatter Color =   
IF(  
    SELECTEDVALUE('Loan_R'[LTV_Ratio]) > 0.90 && SELECTEDVALUE('Customer_R'[Credit_Score]) < 650,   
    "#E66C37", -- Critical Risk (Orange)  
    "#003366"  -- Safe (Navy Blue)  
)  
**2. Portfolio Risk Metrics**  
****Purpose: Calculates the percentage of the total loan book exposed to high-risk factors.****  
****codeDax****  
Portfolio at Risk % =   
VAR RiskyVal = CALCULATE(SUM('Loan_R'[Loan_Balance]), 'Loan_R'[LTV_Ratio] > 0.90)  
VAR TotalVal = SUM('Loan_R'[Loan_Balance])  
RETURN DIVIDE(RiskyVal, TotalVal, 0)  
  
  
**3. Churn & Digital Migration**  
****Purpose: Calculates the churn rate and total capital flight, isolating competitors (Monzo/Revolut) using KEEPFILTERS.****  
**Competitor Market Share:**  
****codeDax****  
Competitor Usage % =   
VAR CompetitorTxns = CALCULATE(COUNTROWS('Monthly_Activity_R'), 'Monthly_Activity_R'[Primary_Bank] <> "RBS")  
VAR TotalTxns = COUNTROWS('Monthly_Activity_R')  
RETURN DIVIDE(CompetitorTxns, TotalTxns, 0)  
**Capital Flight (Money Out):**  
****codeDax****  
Total Money Out =   
CALCULATE(  
    SUM('Monthly_Activity_R'[Estimated_Spend]),  
    KEEPFILTERS('Monthly_Activity_R'[Primary_Bank] <> "RBS")  
)  
  
  
**4.Retention Rate (The Funnel):**  
****codeDax****  
RETENTION RATE =   
VAR Loyalists = CALCULATE(DISTINCTCOUNT('Monthly_Activity_R'[Customer_ID]), 'Monthly_Activity_R'[Primary_Bank] = "RBS", YEAR('Monthly_Activity_R'[Date]) = 2025)  
VAR TotalBase = COUNTROWS('Customer_R')  
RETURN DIVIDE(Loyalists, TotalBase, 0)  
  
  
**5. Portfolio Waterfall Logic (The Financial Walk)**  
****Purpose: Reconciles the movement of the loan book. "Repayments" are calculated as the residual value (Open + New - WriteOff - Close).****  
**Base Measures:**  
****codeDax****  
closing Balance = SUM('Loan_R'[Loan_Balance])  
  
opening Balance = CALCULATE(SUM('Loan_R'[Loan_Balance]), SAMEPERIODLASTYEAR('Monthly_Activity_R'[Date]))  
  
New Loans = CALCULATE(SUM('Loan_R'[Loan_Balance]), DATESBETWEEN('Monthly_Activity_R'[Date], MIN('Monthly_Activity_R'[Date]), MAX('Monthly_Activity_R'[Date])))  
  
Write Offs = CALCULATE(SUM('Loan_R'[Loan_Balance]), 'Monthly_Activity_R'[Missed_Pay] >= 3) * -1  
  
Repayments = ([opening Balance] + [New Loans] - ABS([Write Offs])) - [closing Balance]  
**Switch Statement (For the Visual):**  
****codeDax****  
Waterfall Values =   
VAR CurrentCategory = SELECTEDVALUE('waterfall_categories'[category])  
  
RETURN SWITCH(CurrentCategory,  
    "opening balance", [opening Balance],  
    "(+) new loans", [New Loans],  
    "(-) repayments", [Repayments] * -1,  
    "(-) write off", [Write Offs],  
    "closing balance", [closing Balance],  
    BLANK()  
)  
