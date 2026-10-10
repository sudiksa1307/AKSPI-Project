# AKSPI Project: Heart Disease Analysis

## Question
How do heart disease rates compare between male and female patients in this dataset, and how could a healthcare provider use these differences to guide screening and prevention programs?

- **Number to measure:** Percentage of patients with heart disease.
- **Group to compare:** Sex (`sex` column).

## Dataset
- **Name:** Synthetic Heart Disease Risk Analysis
- **Kaggle link:** https://www.kaggle.com/datasets/srisyra02/synthetic-heart-disease-risk-analysis
- **What one row means:** Each row represents one synthetic patient, including their age, sex, health measurements, and heart disease status.

## Python + AI

### Python
I used pandas to load the CSV dataset, remove duplicate rows and rows with missing values, and standardize column names. I checked the first five rows, column names, and data types to understand the dataset. After cleaning, the table contained 5,000 records.

```python
import pandas as pd

df = pd.read_csv("PASTE_YOUR_RAW_LINK")
df = df.drop_duplicates().dropna()
df.columns = (
    df.columns.str.strip()
    .str.lower()
    .str.replace(" ", "_")
)

df.head()
```

### AI
I used ChatGPT to help refine my research question, understand SQL concepts, and draft queries comparing heart disease percentages by sex. I ran the queries in Google Colab and checked their outputs. The numerical findings below come from the query results.

## SQL

The queries below assume that `num = 0` means no heart disease and `num > 0` means heart disease. This interpretation must be confirmed using the dataset documentation.

**Q1. How many synthetic patient records show heart disease?**

```sql
SELECT COUNT(*) AS heart_disease_cases
FROM data
WHERE num > 0;
```

**Finding:** [count] of the 5,000 synthetic patient records show heart disease.

**Q2. Which sex group has the highest percentage of patients with heart disease?**

```sql
SELECT sex,
       COUNT(*) AS total_patients,
       SUM(CASE WHEN num > 0 THEN 1 ELSE 0 END) AS disease_cases,
       ROUND(
           100.0 * AVG(CASE WHEN num > 0 THEN 1 ELSE 0 END),
           2
       ) AS disease_percentage
FROM data
GROUP BY sex
ORDER BY disease_percentage DESC;
```

**Finding:** Sex group [code] has the highest heart disease percentage at [percentage]%, compared with [percentage]% in the other group.

**Q3. What are the top five ages by heart disease case count within sex group 1?**

```sql
SELECT age,
       COUNT(*) AS disease_cases
FROM data
WHERE sex = 1 AND num > 0
GROUP BY age
ORDER BY disease_cases DESC, age ASC
LIMIT 5;
```

**Finding:** Within sex group 1, the top five ages are [ages], with [counts] heart disease cases respectively.

## Limitations
This dataset contains synthetic patient records. The findings describe patterns in this dataset and do not establish real-world heart disease rates or clinical screening recommendations.

