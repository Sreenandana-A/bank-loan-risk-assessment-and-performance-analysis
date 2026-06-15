# 🏦 Bank Loan Risk Assessment and Performance Analysis — SQL & Tableau

Analyzing loan portfolio performance, borrower behavior, profitability, and credit risk through SQL analysis and Tableau visualization.
## 📋 Table of Contents

- [Overview](#-overview)
- [Problem Statement](#-problem-statement)
- [Dataset](#-dataset)
- [Tools and Technologies](#️-tools-and-technologies)
- [Key Questions Answered](#-key-questions-answered)
- [SQL Analysis and Queries](#-sql-analysis-and-queries)
- [Key Insights](#-key-insights)
- [Dashboard](#-dashboard)
- [Results & Conclusion](#-results--conclusion)
- [Future Work](#-future-work)
- [Author & Contact](#-author--contact)

## 🔍 Overview

This project analyzes a bank loan portfolio using SQL and Tableau to evaluate loan performance, borrower behavior, profitability, and credit risk. The analysis identifies high-risk loan segments, measures portfolio performance through KPIs, and uncovers factors associated with loan repayment and defaults.

## ❓ Problem Statement

Banks must balance profitability with credit risk when issuing loans. Understanding repayment behavior, loan defaults, borrower characteristics, and lending patterns is critical for making informed business decisions.

This project aims to identify high-risk loan segments, evaluate portfolio performance, analyze borrower behavior, and uncover factors associated with loan repayment performance and defaults.

## 🗃️ Dataset

The dataset contains historical bank loan information, borrower profiles, loan performance metrics, and repayment details.



| Category | Key Fields |
|----------|------------|
| Identification | id, member_id |
| Geographic Information | address_state |
| Loan Information | loan_amount, term, installment, application_type |
| Credit Information | grade, sub_grade, int_rate |
| Borrower Information | emp_title, emp_length, annual_income, home_ownership, verification_status |
| Financial Information | dti, total_acc |
| Loan Purpose | purpose |
| Loan Status Information | loan_status |
| Date Information | issue_date, last_payment_date, next_payment_date, last_credit_pull_date |
| Repayment Information | total_payment |

## 🛠️ Tools and Technologies

SQL Server | SSMS | Tableau | Data Cleaning | EDA | KPI Analysis | GitHub

## 🎯 Key Questions Answered

1. Which state issued the highest number of loans?
2. Which loan grade received the highest funded amount?
3. What was the most common loan purpose?
4. What percentage of loans were fully paid?
5. Which loan grade recorded the highest number of charged-off loans?
6. Which loan grade had the highest default rate?
7. Which loan grade generated the highest profit?
8. Is borrower income associated with loan repayment performance?
9. Is there a relationship between interest rates and loan defaults?
10. Which loan segments represent the highest lending risk?

## 📝 SQL Analysis and Queries
### 🧹 Data Cleaning

#### Check for NULL Values
```sql
SELECT
    SUM(CASE WHEN id IS NULL THEN 1 ELSE 0 END) AS id_nulls,
    SUM(CASE WHEN address_state IS NULL THEN 1 ELSE 0 END) AS address_state_nulls,
    SUM(CASE WHEN application_type IS NULL THEN 1 ELSE 0 END) AS application_type_nulls,
    SUM(CASE WHEN emp_length IS NULL THEN 1 ELSE 0 END) AS emp_length_nulls,
    SUM(CASE WHEN emp_title IS NULL THEN 1 ELSE 0 END) AS emp_title_nulls,
    SUM(CASE WHEN grade IS NULL THEN 1 ELSE 0 END) AS grade_nulls,
    SUM(CASE WHEN home_ownership IS NULL THEN 1 ELSE 0 END) AS home_ownership_nulls,
    SUM(CASE WHEN issue_date IS NULL THEN 1 ELSE 0 END) AS issue_date_nulls,
    SUM(CASE WHEN last_credit_pull_date IS NULL THEN 1 ELSE 0 END) AS last_credit_pull_date_nulls,
    SUM(CASE WHEN last_payment_date IS NULL THEN 1 ELSE 0 END) AS last_payment_date_nulls,
    SUM(CASE WHEN loan_status IS NULL THEN 1 ELSE 0 END) AS loan_status_nulls,
    SUM(CASE WHEN next_payment_date IS NULL THEN 1 ELSE 0 END) AS next_payment_date_nulls,
    SUM(CASE WHEN member_id IS NULL THEN 1 ELSE 0 END) AS member_id_nulls,
    SUM(CASE WHEN purpose IS NULL THEN 1 ELSE 0 END) AS purpose_nulls,
    SUM(CASE WHEN sub_grade IS NULL THEN 1 ELSE 0 END) AS sub_grade_nulls,
    SUM(CASE WHEN term IS NULL THEN 1 ELSE 0 END) AS term_nulls,
    SUM(CASE WHEN verification_status IS NULL THEN 1 ELSE 0 END) AS verification_status_nulls,
    SUM(CASE WHEN annual_income IS NULL THEN 1 ELSE 0 END) AS annual_income_nulls,
    SUM(CASE WHEN dti IS NULL THEN 1 ELSE 0 END) AS dti_nulls,
    SUM(CASE WHEN installment IS NULL THEN 1 ELSE 0 END) AS installment_nulls,
    SUM(CASE WHEN int_rate IS NULL THEN 1 ELSE 0 END) AS int_rate_nulls,
    SUM(CASE WHEN loan_amount IS NULL THEN 1 ELSE 0 END) AS loan_amount_nulls,
    SUM(CASE WHEN total_acc IS NULL THEN 1 ELSE 0 END) AS total_acc_nulls,
    SUM(CASE WHEN total_payment IS NULL THEN 1 ELSE 0 END) AS total_payment_nulls
FROM bank_loan_data
```
#### Null Handling
```sql
UPDATE bank_loan_data SET emp_title='Unknown' WHERE emp_title IS NULL
```
#### Check for Empty Strings
```sql
SELECT *
FROM bank_loan_data
WHERE address_state = '' OR application_type = '' OR emp_length = '' OR emp_title = ''
   OR grade = '' OR home_ownership = '' OR loan_status = '' OR purpose = ''
   OR sub_grade = ''OR term = '' OR verification_status = ''
```
#### Text Standardization
```sql
UPDATE bank_loan_data SET emp_title= lower(trim(emp_title)) WHERE emp_title IS NOT NULL
```
#### Duplicate Detection
```sql
SELECT
    *,
    COUNT(*) AS duplicate_count
FROM bank_loan_data
GROUP BY
    id,address_state,application_type,emp_length,emp_title,grade,home_ownership,issue_date,
    last_credit_pull_date,last_payment_date,loan_status,next_payment_date,member_id,
    purpose,sub_grade,term,verification_status,annual_income,dti,installment,
    int_rate,loan_amount,total_acc,total_payment
HAVING COUNT(*) > 1
```
#### Data Validation
```sql
SELECT *
FROM bank_loan_data
WHERE annual_income <= 0

SELECT *
FROM bank_loan_data
WHERE loan_amount <= 0

SELECT *
FROM bank_loan_data
WHERE int_rate <= 0

SELECT *
FROM bank_loan_data
WHERE total_payment <= 0
```
### 📊 Exploratory Data Analysis (EDA)

#### Loan Status Distribution
```sql
SELECT loan_status, COUNT(*)
FROM bank_loan_data
GROUP BY loan_status
```
#### Loan Distribution by State
```sql
SELECT address_state, COUNT(*)
FROM bank_loan_data
GROUP BY address_state
```
#### Loan Distribution by Purpose
```sql
SELECT purpose, COUNT(*)
FROM bank_loan_data
GROUP BY purpose
```

#### Loan Distribution by Grade
```sql
SELECT grade, COUNT(*)
FROM bank_loan_data
GROUP BY grade
```

### 📈 KPI Analysis

#### Total Loan Applications
```sql
SELECT COUNT(*) AS Total_Loan
FROM bank_loan_data
```
#### Total Loan Amount
```sql
SELECT SUM(loan_amount) AS Total_Loan_Amount
FROM bank_loan_data
```

#### Total Amount Received
```sql
SELECT SUM(total_payment) AS Total_Amount_Received
FROM bank_loan_data
```

#### Average Loan Amount
```sql
SELECT AVG(loan_amount) AS Avg_Loan_Amount
FROM bank_loan_data
```

#### Average Interest Rate
```sql
SELECT AVG(int_rate) AS Avg_Interest_Rate
FROM bank_loan_data
```

#### Average Annual Income
```sql
SELECT AVG(annual_income) AS Avg_Annual_Income
FROM bank_loan_data
```

#### Good Loan Percentage
```sql
SELECT
ROUND(
    COUNT(CASE WHEN loan_status='Fully Paid' THEN 1 END)
    *100.0/COUNT(*),2
) AS Good_Loan_Percentage
FROM bank_loan_data
```

#### Good Loan Applications
```sql
SELECT COUNT(*) AS Good_Loan_Applications
FROM bank_loan_data
WHERE loan_status='Fully Paid'
```

#### Good Loan Amount Received
```sql
SELECT SUM(total_payment) AS Good_Loan_Amount_Received
FROM bank_loan_data
WHERE loan_status IN ('Fully Paid','Current')
```

#### Bad Loan Percentage
```sql
SELECT
ROUND(
    COUNT(CASE WHEN loan_status='Charged Off' THEN 1 END)
    *100.0/COUNT(*),2
) AS Bad_Loan_Percentage
FROM bank_loan_data
```

#### Bad Loan Applications
```sql
SELECT COUNT(*) AS Bad_Loan_Applications
FROM bank_loan_data
WHERE loan_status='Charged Off'
```

#### Bad Loan Amount Received
```sql
SELECT SUM(total_payment) AS Bad_Loan_Amount_Received
FROM bank_loan_data
WHERE loan_status='Charged Off'
```

### 🔍 Advanced Business Analysis

#### Default Rate by Grade
```sql
SELECT
    grade,
    ROUND(
        COUNT(CASE WHEN loan_status='Charged Off' THEN 1 END)
        *100.0/COUNT(*),2
    ) AS default_rate
FROM bank_loan_data
GROUP BY grade
ORDER BY default_rate DESC
```

#### Profitability Analysis
```sql
SELECT
    grade,
    SUM(total_payment)-SUM(loan_amount) AS profit
FROM bank_loan_data
GROUP BY grade
ORDER BY profit DESC
```

#### Borrower Analysis
```sql
SELECT
    home_ownership,
    loan_status,
    COUNT(*) AS loans
FROM bank_loan_data
GROUP BY home_ownership, loan_status
```

#### Income Analysis
```sql
SELECT
    loan_status,
    AVG(annual_income) AS avg_income
FROM bank_loan_data
GROUP BY loan_status
```

#### Interest Rate Analysis
```sql
SELECT
    loan_status,
    AVG(int_rate) AS avg_interest_rate
FROM bank_loan_data
GROUP BY loan_status
```

#### Top 5 Largest Loans in Each Grade
```sql
SELECT *
FROM (
    SELECT *,
           ROW_NUMBER() OVER (
               PARTITION BY grade
               ORDER BY loan_amount DESC
           ) AS rn
    FROM bank_loan_data
) t
WHERE rn <= 5
```
#### State Ranking by Total Loan Amount
```sql
SELECT
    address_state,
    SUM(loan_amount) AS total_loan_amount,
    RANK() OVER (
        ORDER BY SUM(loan_amount) DESC
    ) AS state_rank
FROM bank_loan_data
GROUP BY address_state
```
#### Monthly Loan Trend Analysis
```sql
SELECT
    YEAR(issue_date) AS year,
    MONTH(issue_date) AS month,
    COUNT(*) AS total_loans,
    SUM(loan_amount) AS total_amount
FROM bank_loan_data
GROUP BY YEAR(issue_date), MONTH(issue_date)
ORDER BY year, month
```
## 💡 Key Insights

- 🏆 California (CA) issued the highest number of loans with 6,894 applications.
- 💰 Grade B received the highest funded amount totaling $130.7M.
- 📌 Debt Consolidation was the most common loan purpose with 18,214 applications.
- ✅ 83.33% of all loans were fully paid.
- ⚠️ Grade B recorded the highest number of charged-off loans with 1,343 defaults.
- 🚨 Grade G had the highest default rate at 31.31%, making it the riskiest loan grade.
- 📈 Grade B generated the highest profit of $10.07M.
- 👥 Charged-off borrowers had the lowest average annual income ($63.5K).
- 📊 Charged-off loans had a higher average interest rate (13.88%) than fully paid loans (11.64%).




## ✅ Results & Conclusion

The analysis revealed significant patterns in loan performance, borrower behavior, profitability, and risk. Grade B loans contributed the highest funding volume and profitability, while Grade G loans exhibited the highest default risk. Income and interest rate analyses suggested strong associations with repayment outcomes. These findings can support data-driven lending strategies and improve risk management decisions.

## 🔮 Future Work

- Build predictive loan default models using Machine Learning.
- Implement borrower risk scoring.
- Develop real-time portfolio monitoring dashboards.
- Integrate automated loan performance reporting.

## 👤 Author & Contact

*Sreenandana A*

- 💼 LinkedIn: https://www.linkedin.com/in/sreenandana-a-
- 📧 Email: sreenandananair03@gmail.com
