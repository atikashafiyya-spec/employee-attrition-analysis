# Employee Attrition Analysis
This analysis focuses on identifying workforce factors associated with employee attrition to support employee retention strategies and organizational decision making.

## Dataset Overview

The dataset contains 1,470 employee records with 35 variables related to employee demographics, job roles, compensation, and workplace satisfaction.
This analysis focuses on identifying factors that may influence employee attrition.

### Key Variables
- **OverTime** — workload indicator
- **MonthlyIncome** — compensation factor
- **JobSatisfaction** — employee satisfaction level
- **WorkLifeBalance** — work-life balance evaluation
- **JobInvolvement** — employee engagement level
- **Attrition** — target variable indicating employee turnover

### Supporting Variables
Additional variables were used to provide contextual analysis, including:
- Age
- Department
- Job Role

## Data Analyst Workflow
### 1. Data Cleaning & Validation
Performed data cleaning and validation using SQLite, including:
  - Checking Missing Value
  - Checking Duplicate Employee IDs
  - Detecting Data Anomalies
Key SQL queries used during the cleaning process:

<details>
<summary><b>View SQL Data Cleaning Queries</b></summary>

```SQL
-- 1. Missing Value Check
SELECT COUNT(*) AS null_count 
FROM Employee
WHERE EmployeeNumber IS NULL 
   OR Attrition IS NULL 
   OR Age IS NULL;

-- 2. Duplicate Employee IDs Check
SELECT 
    EmployeeNumber, 
    COUNT(*) AS duplicate_count
FROM Employee
GROUP BY EmployeeNumber
HAVING COUNT(*) > 1;

-- 3. Detecting Data Anomalies
SELECT COUNT(*) AS anomaly_count
FROM Employee
WHERE Age <= 0 
   OR MonthlyIncome <= 0 
   OR TotalWorkingYears < 0;
```
</details>

### 2. SQL Exploratory Data Analysis (EDA)
Several SQL queries were used to explore employee attrition patterns and generate business insights.

<details> <summary><b>View SQL EDA Queries</b></summary>

```SQL
-- 1. Overall Attrition Rate
SELECT 
    ROUND(SUM(CASE WHEN Attrition = 'Yes' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) AS Attrition_Rate
FROM Employee;

-- 2. Attrition Rate by Overtime
SELECT 
    OverTime,
    ROUND(SUM(CASE WHEN Attrition = 'Yes' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) AS Attrition_Rate
FROM Employee
GROUP BY OverTime;

-- 3. Attrition Rate by Work-Life Balance
SELECT 
    WorkLifeBalance,
    ROUND(SUM(CASE WHEN Attrition = 'Yes' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) AS Attrition_Rate
FROM Employee
GROUP BY WorkLifeBalance
ORDER BY WorkLifeBalance;

-- 4. Attrition Rate by Job Satisfaction
SELECT 
    JobSatisfaction,
    ROUND(SUM(CASE WHEN Attrition = 'Yes' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) AS Attrition_Rate
FROM Employee
GROUP BY JobSatisfaction
ORDER BY JobSatisfaction;

-- 5. Attrition Rate by Job Involvement
SELECT 
    JobInvolvement,
    ROUND(SUM(CASE WHEN Attrition = 'Yes' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) 
    AS Attrition_Rate
FROM Employee
GROUP BY JobInvolvement
ORDER BY JobInvolvement;

-- 6. Attrition Rate by Age Group
SELECT 
    CASE 
        WHEN Age BETWEEN 18 AND 25 THEN '18-25'
        WHEN Age BETWEEN 26 AND 35 THEN '26-35'
        WHEN Age BETWEEN 36 AND 45 THEN '36-45'
        WHEN Age BETWEEN 46 AND 55 THEN '46-55'
        ELSE '55+' 
    END AS AgeGroup,
    ROUND(SUM(CASE WHEN Attrition = 'Yes' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) AS Attrition_Rate
FROM Employee
GROUP BY 1
ORDER BY 1;

-- 7. Attrition Rate by Income Gorup
SELECT 
    CASE 
        WHEN MonthlyIncome < 2000 THEN '< 2K'
        WHEN MonthlyIncome BETWEEN 2000 AND 5000 THEN '2K-5K'
        WHEN MonthlyIncome BETWEEN 5001 AND 10000 THEN '5K-10K'
        WHEN MonthlyIncome BETWEEN 10001 AND 15000 THEN '10K-15K'
        ELSE '15K+' 
    END AS GroupIncome,
    ROUND(SUM(CASE WHEN Attrition = 'Yes' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) 
    AS Attrition_Rate
FROM Employee
GROUP BY 1
ORDER BY MIN(MonthlyIncome);

-- 8. Job Satisfaction vs Attrition by Job Role
SELECT 
    JobRole,
    ROUND(SUM(CASE WHEN JobSatisfaction = 1 AND Attrition = 'Yes' THEN 1 ELSE 0 END) * 100.0 / NULLIF(SUM(CASE WHEN JobSatisfaction = 1 THEN 1 ELSE 0 END), 0), 2) AS "1",
    ROUND(SUM(CASE WHEN JobSatisfaction = 2 AND Attrition = 'Yes' THEN 1 ELSE 0 END) * 100.0 / NULLIF(SUM(CASE WHEN JobSatisfaction = 2 THEN 1 ELSE 0 END), 0), 2) AS "2",
    ROUND(SUM(CASE WHEN JobSatisfaction = 3 AND Attrition = 'Yes' THEN 1 ELSE 0 END) * 100.0 / NULLIF(SUM(CASE WHEN JobSatisfaction = 3 THEN 1 ELSE 0 END), 0), 2) AS "3",
    ROUND(SUM(CASE WHEN JobSatisfaction = 4 AND Attrition = 'Yes' THEN 1 ELSE 0 END) * 100.0 / NULLIF(SUM(CASE WHEN JobSatisfaction = 4 THEN 1 ELSE 0 END), 0), 2) AS "4",
    ROUND(SUM(CASE WHEN Attrition = 'Yes' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) AS "Total"
FROM Employee
GROUP BY JobRole
ORDER BY Total DESC;
```
</details>

### 3. Power BI Dashboard
The dashboard visualizes employee attrition patterns across several key factors, including:

  - Overtime
  - Job Satisfaction
  - Work-Life Balance
  - Job Involvement
  - Monthly Income
  - Age Group
  - Job Role

## Dashboard Preview
<img width="1428" height="806" alt="image" src="https://github.com/user-attachments/assets/0de130c2-432b-4ed0-8f58-64be8e2aa62c" />

## Data Source
[IBM HR Analytics Employee Attrition Dataset - Kaggle](https://www.kaggle.com/datasets/patelprashant/employee-attrition/data)

## Important Links
- **Interactive Dashboard:** [Download Power BI File (.pbix)](./AtikaShafiyya_AttritionDashboard.pbix)
- **Data Cleansing Documentation:** [Notion Log](https://www.notion.so/Employee-Attrition-Analysis-using-SQL-36c3ba008b1c803a8e6fefd6e9f28409?source=copy_link)


## Tools & Technologies
  - SQL (SQLite)
  - Power BI
  - Data Cleaning & Validation
  - Exploratory Data Analysis (EDA)
  - Data Visualization

## Key Insight
  - Overtime Is the Main Driver of Attrition Rates: Employees who work overtime have an attrition rate of 30.53%, which is three times higher than those who do not work overtime. This indicates a problem with workload (burnout).
  - Retention Among Low-Income Employees (<$2K): Employee turnover is extremely high among those earning less than $2K, exceeding 50%. This suggests that the company’s compensation package is not competitive enough to retain employees.
  - Young Employee Vulnerability (Gen-Z/Early Career): The 18–25 age group accounts for the highest attrition rate at 35,77%. This trend decreases with age, indicating the need for adaptation in onboarding processes and employee engagement for new hires.
  - Linear Relationship Between Satisfaction & Retention: A decrease in scores for Job Satisfaction, Work-Life Balance, and Job Involvement to the lowest level (Score 1) consistently drives a 25% to 35% increase in attrition.
  - High-Attrition Departments: Sales Representative is the most unstable position with a total attrition rate of 39.76%, reaching as high as 58.33% when satisfaction is at its lowest. This is followed by Laboratory Technician (23.94%) and Human Resources (23.08%).

## Business Recommendations
### Immediate Actions
  - Implementation of Overtime & HR Alert System: Set a maximum weekly overtime limit. Create an automated alert system on the HR dashboard: if a team is found to have worked overtime for two consecutive weeks, managers must restructure their tasks.
  - Compensation Review: Conduct salary comparison specifically for roles with earnings under $2K, particularly for Sales Representative and Laboratory Technician positions, to reduce turnover rates exceeding 40%.

### Long-Term Retention Strategy
  - Mentorship Program: To address the 35% attrition rate among employees aged 18–25, implement a mentoring program and provide clear career paths from day one to build a sense of belonging.
  - Implement one-on-one sessions: Provide training for supervisors to conduct regular one-on-one sessions with employees that show signs of declining Job Involvement or Job Satisfaction (scores 1–2).
  - Restructuring the Sales Role’s Workload: Consider that even Sales Representatives with high satisfaction (score 4) still have high attrition (30.43%), suggesting the issue is likely systemic. It is necessary to review whether the role has unrealistic sales targets, a commission system, or administrative tasks that consume too much of their time.
