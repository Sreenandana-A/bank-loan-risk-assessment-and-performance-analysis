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

