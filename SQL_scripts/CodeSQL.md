****CodeSQL****  
/*  
=========================================================  
RBS Portfolio Analysis - SQL Validation Layer  
Description: Data Integrity, Risk Logic, and Churn Validation  
=========================================================  
*/  
  
-- 1. DATA INTEGRITY CHECK  
-- Identifying missing critical risk fields in the Loan and Customer tables  
SELECT   
    COUNT(*) AS Total_Rows,  
    SUM(CASE WHEN Credit_Score IS NULL THEN 1 ELSE 0 END) AS Missing_Scores,  
    SUM(CASE WHEN LTV_Ratio IS NULL THEN 1 ELSE 0 END) AS Missing_LTV  
FROM Loan_R l  
JOIN Customer_R c ON l.Customer_ID = c.Customer_ID;  
  
-- 2. DATA CLEANING  
-- Standardizing Region Names (Fixing 'NW' vs 'North West' inconsistencies)  
UPDATE Customer_R  
SET Region = 'North West'  
WHERE Region IN ('NW', 'N. West', 'Manchester Area');  
  
-- 3. VALIDATING DATE RANGES  
-- Ensuring the dataset covers the full strategic period (2023-2025)  
SELECT MIN(Date), MAX(Date)   
FROM Monthly_Activity_R;  
  
-- 4. BUSINESS QUESTION: CAPITAL FLIGHT ANALYSIS  
-- Identifying customers transferring funds to Challenger Banks (Monzo/Revolut)  
SELECT   
    c.Segment,  
    m.Primary_Bank,  
    COUNT(DISTINCT c.Customer_ID) AS Customer_Count,  
    SUM(m.Estimated_Spend) AS Total_Money_Out  
FROM Customer_R c  
JOIN Monthly_Activity_R m ON c.Customer_ID = m.Customer_ID  
WHERE m.Primary_Bank IN ('Monzo', 'Revolut')   
AND m.Date >= '2025-01-01'  
GROUP BY c.Segment, m.Primary_Bank  
ORDER BY Total_Money_Out DESC;  
  
-- 5. BUSINESS QUESTION: "DOUBLE TROUBLE" SEGMENT  
-- Finding Mortgage customers in the North West with High LTV and Missed Payments  
SELECT   
    c.Customer_ID,  
    l.LTV_Ratio,  
    SUM(m.Missed_Pay) AS Total_Missed_Payments_Last_12M  
FROM Customer_R c  
JOIN Loan_R l ON c.Customer_ID = l.Customer_ID  
JOIN Monthly_Activity_R m ON c.Customer_ID = m.Customer_ID  
WHERE c.Region = 'North West'  
AND l.Product_Type = 'Mortgage'  
AND l.LTV_Ratio > 0.90  
AND m.Date >= DATEADD(year, -1, GETDATE())  
GROUP BY c.Customer_ID, l.LTV_Ratio  
HAVING SUM(m.Missed_Pay) >= 2;   
  
-- 6. LOGIC VALIDATION FOR PORTFOLIO WATERFALL  
-- Verifying Write-Off logic (Loans with 3+ missed payments)  
SELECT   
    l.Product_Type,  
    SUM(l.Loan_Balance) AS Write_Off_Exposure  
FROM Loan_R l  
JOIN Monthly_Activity_R m ON l.Customer_ID = m.Customer_ID  
WHERE m.Missed_Pay >= 3  
GROUP BY l.Product_Type;  
