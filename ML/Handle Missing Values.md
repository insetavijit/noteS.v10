| **Action**                         | **Description / What to Do**                                                    | **Tools / Methods**                                      |
| ---------------------------------- | ------------------------------------------------------------------------------- | -------------------------------------------------------- |
| Identify Missing Data              | Locate NA/Null/Blank entries and measure their percentage                       | `df.isnull().sum()`, `missingno`, `df.info()`            |
| Analyze Impact                     | Understand which features matter and whether missingness is random or patterned | EDA, distribution checks, domain understanding           |
| Decide Strategy                    | Choose imputation or removal based on feature type & importance                 | Drop / Mean / Median / Mode / Custom / Predictive        |
| Impute Numerical Values            | Replace missing numeric fields using central tendency or models                 | Mean, Median, KNN, Regression                            |
| Impute Categorical Values          | Fill missing categories logically to maintain data integrity                    | Mode, “Unknown”, label encoding                          |
| Group-Based Imputation             | Impute values within logical groups for accuracy                                | Median by property type, city, category                  |
| Add Indicator Column               | Track which values were missing for interpretability                            | `df['col_missing'] = df['col'].isnull()`                 |
| Validate After Imputation          | Re-check dataset integrity and distributions                                    | `df.isnull().sum()` again, histograms                    |
| Automate with Pipelines (optional) | Build reproducible preprocessing workflows                                      | `sklearn.pipeline`, `SimpleImputer`, `ColumnTransformer` |

### **1. [[Handle Missing Values]]**

Identify incomplete records where certain numerical or categorical fields contain blanks, nulls, or undefined entries. Missing values can distort statistical calculations, reduce model training quality, and diminish predictive accuracy if ignored. The goal is to assess which fields are missing, determine their significance, and fill them using techniques appropriate to the data type and distribution.

For **numerical features**, choose common imputation methods such as **mean** or **median** depending on whether the data distribution is uniform or skewed. For highly skewed datasets, the **median** is usually more robust. For **categorical values**, the **mode** (most frequent value) or a special placeholder such as `"Unknown"` can prevent data loss and ensure model stability.

For example, if the **“bathrooms”** column contains blank entries, replacing them with the **median number of bathrooms** across **similar property types** ensures that missing values do not introduce bias or distort learning patterns in a real-estate prediction model.

---

#### **What this step achieves**

- Prevents accidental bias caused by missing samples
    
- Maintains dataset completeness without discarding useful rows
    
- Improves model performance and stability
    
- Preserves data distribution and logical patterns
    

---

#### **Common Techniques**

|Type of Feature|Imputation Technique|Description / Usage|
|---|---|---|
|Numerical|Mean / Median|Replaces missing numeric values based on distribution|
|Numerical|KNN / Regression Imputation|Predict missing values using feature relationships|
|Categorical|Mode (Most Frequent)|Fills blanks using the most common category|
|Categorical|“Unknown” Category|Adds a separate cluster for missing values|
|Both|Grouped Imputation|Fills missing values based on subgroups (e.g., property type or location)|

---

#### **Mini Code Example**

```python
import pandas as pd

df = pd.read_csv("housing.csv")

# Impute numerical values
df['bathrooms'] = df['bathrooms'].fillna(df['bathrooms'].median())

# Impute categorical values
df['property_type'] = df['property_type'].fillna(df['property_type'].mode()[0])
```

---

#### **Best Practice**

Always validate after imputation using:

```python
df.isnull().sum()
```

---

Say **"expand row 2"** to move to the next item:

### `2. Fix Incorrect or Inconsistent Data` 🚀