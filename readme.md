# 📊 Reducing 30-Day Readmissions in Diabetic Inpatients  
*A Data Analytics Project using SQL & Tableau*

---

## 🏥 Project Overview
Hospital readmissions within 30 days are a major challenge in healthcare.  
Using the **[Diabetes 130-US Hospitals dataset](https://www.kaggle.com/datasets/)** (1999–2008), I analyzed patient encounters to uncover drivers of readmissions and created interactive Tableau dashboards for hospital decision-makers.

---

## 🎯 Objectives
- Load and manage healthcare data in **PostgreSQL**.  
- Write **SQL queries** to answer key clinical and operational questions.  
- Build an **interactive Tableau dashboard** for KPIs and insights.  
- Derive actionable recommendations to reduce readmissions.  

---

## 🗂️ Dataset
- **Source:** Kaggle — Diabetes 130-US Hospitals for years 1999–2008  
- **Rows:** ~100,000 encounters  
- **Columns:** 50 (demographics, diagnoses, lab results, medications, outcomes)  
- **Key variables:**  
  - `encounter_id`, `patient_nbr`, `age`, `gender`, `race`  
  - `admission_type_id`, `discharge_disposition_id`, `time_in_hospital`  
  - `diag_1`, `diag_2`, `diag_3` (ICD-9 codes)  
  - `num_medications`, `insulin`, `metformin`, …  
  - `readmitted` (<30, >30, No)  

---

## 🔍 SQL Analysis
Some guiding questions I answered with SQL:  

1. What % of patients are readmitted within 30 days?  
2. Which diagnoses are most associated with readmissions?  
3. How does **length of stay (LOS)** affect readmission risk?  
4. Do medication adjustments (e.g., insulin up/down) influence outcomes?  
5. Are certain **age groups** or **payer types** more at risk?  
6. Which hospitals or admission types drive the highest readmissions?  

**Sample Query:**

```sql
-- Readmission rate by length of stay bucket
SELECT 
    CASE 
        WHEN time_in_hospital BETWEEN 1 AND 3 THEN '1–3 days'
        WHEN time_in_hospital BETWEEN 4 AND 7 THEN '4–7 days'
        WHEN time_in_hospital BETWEEN 8 AND 14 THEN '8–14 days'
        ELSE '15+ days'
    END AS los_bucket,
    COUNT(*) AS encounters,
    AVG(CASE WHEN readmitted = '<30' THEN 1 ELSE 0 END) AS readmit_rate
FROM diabetic_data
GROUP BY 1
ORDER BY 1;

