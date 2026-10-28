# 🐼 Pandas & Data Preprocessing — Interview Questions & Answers

A comprehensive technical interview preparation guide covering Pandas DataFrames, missing value imputation, duplicate handling, multi-condition filtering, groupby aggregations, schema transformations, and production machine learning data preprocessing pipelines.

---

## 📋 Table of Contents

1. [What Is Pandas and Why Is Data Preprocessing 80% of an AI/ML Engineer's Work?](#q1-what-is-pandas-and-why-is-data-preprocessing-80-of-an-aiml-engineers-work)
2. [How Do You Detect and Quantify Missing Values (isnull, isna, sum) in a DataFrame?](#q2-how-do-you-detect-and-quantify-missing-values-isnull-isna-sum-in-a-dataframe)
3. [What Is the Difference Between Dropping Missing Values (dropna) and Imputing Them (fillna)?](#q3-what-is-the-difference-between-dropping-missing-values-dropna-and-imputing-them-fillna)
4. [How Do You Detect and Remove Duplicate Records Using drop_duplicates()?](#q4-how-do-you-detect-and-remove-duplicate-records-using-drop_duplicates)
5. [How Do You Rename Columns and Modify Schema Definitions in Pandas?](#q5-how-do-you-rename-columns-and-modify-schema-definitions-in-pandas)
6. [How Do You Filter DataFrames Using Boolean Indexing and Query Conditions?](#q6-how-do-you-filter-dataframes-using-boolean-indexing-and-query-conditions)
7. [How Does sort_values() Work for Single and Multi-Column Sorting?](#q7-how-does-sort_values-work-for-single-and-multi-column-sorting)
8. [How Does groupby() Work and How Do You Execute Multiple Aggregations?](#q8-how-does-groupby-work-and-how-do-you-execute-multiple-aggregations)
9. [How Do You Engineer New Feature Columns and Export Cleaned Data to CSV?](#q9-how-do-you-engineer-new-feature-columns-and-export-cleaned-data-to-csv)
10. [What Is the Difference Between loc and iloc in Pandas?](#q10-what-is-the-difference-between-loc-and-iloc-in-pandas)
11. [What Is the Difference Between a Pandas Series and a DataFrame?](#q11-what-is-the-difference-between-a-pandas-series-and-a-dataframe)
12. [What Does the Complete Production Machine Learning Preprocessing Pipeline Look Like?](#q12-what-does-the-complete-production-machine-learning-preprocessing-pipeline-look-like)

---

### Q1: What Is Pandas and Why Is Data Preprocessing 80% of an AI/ML Engineer's Work?

> **Interview-Ready Answer:**  
> Pandas is Python's core data analysis and manipulation library built on top of NumPy, providing labeled tabular data structures (`Series` and `DataFrame`); data preprocessing consumes ~80% of practical machine learning effort because production data is noisy, incomplete, and unstructured, and algorithmic models strictly depend on clean numeric inputs to avoid the "garbage-in, garbage-out" failure mode.

#### The Data Quality Reality in Production AI

```text
Raw Industry Data (Dirty)                 Engineered Training Data (Clean)
┌─────────────────────────────────┐       ┌─────────────────────────────────┐
│ Missing values (NaN, null, "")  │       │ Imputed medians / categoricals  │
│ Duplicate records               │  ──►  │ Deduplicated entity keys        │
│ Inconsistent casing / units     │ (80%) │ Scaled float32 feature tensors  │
│ Outliers / corrupted sensors    │       │ Filtered, clipped distributions │
└─────────────────────────────────┘       └─────────────────────────────────┘
```

#### Why Clean Data Directly Determines Model Accuracy:
- **Missing Data:** Algorithms like Linear Regression and Support Vector Machines cannot process `NaN` values and will raise fatal exceptions during `.fit()`.
- **Outliers:** Skew gradient steps and disproportionately distort quadratic loss functions like Mean Squared Error.
- **Data Leakage:** Unsanitized training sets that incorporate testing distribution metadata cause false high validation scores that collapse in production.

---

### Q2: How Do You Detect and Quantify Missing Values (isnull, isna, sum) in a DataFrame?

> **Interview-Ready Answer:**  
> Missing values are detected using `.isnull()` or `.isna()` (which return an element-wise boolean mask where `NaN` or `None` map to `True`); total missing counts per feature column are quantified by chaining `.sum()`, and missing percentages are determined by dividing by `len(df)`.

#### Code Demonstration

```python
import pandas as pd
import numpy as np

# Sample realistic dataset
data = {
    "Name": ["Krishna", "Rahul", "Aman", None, "Priya"],
    "Age": [25, 30, np.nan, 22, 28],
    "Salary": [50000.0, 60000.0, 55000.0, np.nan, 70000.0],
    "City": ["Indore", "Delhi", "Indore", "Mumbai", None]
}
df = pd.DataFrame(data)

# 1. Element-wise boolean mask
mask = df.isnull()

# 2. Count of missing values per column
missing_counts = df.isnull().sum()

# 3. Percentage of missing values per column (Critical for feature pruning)
missing_pct = (df.isnull().sum() / len(df)) * 100

print("Missing Counts:\n", missing_counts)
print("\nMissing Percentages:\n", missing_pct)
```

#### Output Analysis:
```text
Missing Counts:
Name      1
Age       1
Salary    1
City      1
dtype: int64
```
> **Industry Rule of Thumb:** If a column has $> 40\%$–$50\%$ missing values and cannot be reliably imputed, it is often discarded from the feature set.

---

### Q3: What Is the Difference Between Dropping Missing Values (dropna) and Imputing Them (fillna)?

> **Interview-Ready Answer:**  
> `dropna()` removes rows or columns containing missing values, which risks severe information loss on small datasets, whereas `fillna()` replaces missing entries with statistical estimates (mean for Gaussian distributions, median for skewed data, mode for categoricals) to preserve sample size and statistical power.

#### Imputation Strategies

```python
import pandas as pd

# Strategy 1: Drop rows with ANY missing value
df_dropped = df.dropna()
# Drops 4 out of 5 rows, leaving only 1 row! (Severe data loss)

# Strategy 2: Drop only if all columns are NaN
df_dropped_all = df.dropna(how='all')

# Strategy 3: Drop columns exceeding missing threshold
df_dropped_cols = df.dropna(axis=1, thresh=4)

# Strategy 4: Statistical Imputation (Recommended)
# Age has no extreme outliers -> fill with mean
df["Age"] = df["Age"].fillna(df["Age"].mean())

# Salary might be skewed -> fill with median
df["Salary"] = df["Salary"].fillna(df["Salary"].median())

# Categorical text -> fill with mode or constant "Unknown"
df["City"] = df["City"].fillna("Unknown")
df["Name"] = df["Name"].fillna("Anonymous")

print("Cleaned DataFrame:\n", df)
```

#### Comparison Matrix

| Strategy | Advantages | Disadvantages | Best Used When |
| :--- | :--- | :--- | :--- |
| **`dropna()`** | Fast, guarantees 100% clean data with no artificial noise | Discards valid information in other columns; reduces dataset size | Huge datasets ($10^6$ rows) where missing count is $< 1\%$ |
| **Mean Imputation** | Preserves sample count; maintains column mean | Lowers variance; vulnerable to severe skew from extreme outliers | Normally distributed numeric variables |
| **Median Imputation**| Robust against extreme outliers and skewed distributions | Alters correlation between features | Skewed numeric data (e.g., salaries, house prices) |
| **Mode / Constant** | Preserves categorical structure | Can introduce artificial mode bias if missingness is high | Low-cardinality nominal features |

---

### Q4: How Do You Detect and Remove Duplicate Records Using drop_duplicates()?

> **Interview-Ready Answer:**  
> Duplicates are identified with `.duplicated()` and eliminated using `.drop_duplicates()`; the method accepts the `subset` parameter to match on specific unique business keys and the `keep` parameter (`'first'`, `'last'`, or `False`) to define which duplicate instances to preserve.

#### Code Demonstration

```python
import pandas as pd

records = {
    "emp_id": [101, 102, 103, 101, 104],
    "name": ["Alice", "Bob", "Charlie", "Alice", "David"],
    "salary": [70000, 80000, 75000, 70000, 90000]
}
emp_df = pd.DataFrame(records)

# 1. Detect duplicates across all columns
dup_mask = emp_df.duplicated()  # Returns boolean Series

# 2. Drop duplicates (keeping first occurrence)
clean_df = emp_df.drop_duplicates()

# 3. Deduplicate based on a unique entity ID subset
clean_subset = emp_df.drop_duplicates(subset=["emp_id"], keep="first")

print("Deduplicated Count:", len(clean_subset))  # 4 rows
```

#### Why Removing Duplicates Is Critical for ML:
Duplicates inflate evaluation metrics by allowing identical samples to exist simultaneously in both training and test sets, causing artificial test performance that fails in production.

---

### Q5: How Do You Rename Columns and Modify Schema Definitions in Pandas?

> **Interview-Ready Answer:**  
> Columns are renamed using `df.rename(columns={'old_name': 'new_name'})` with dictionary mapping for selective changes, or by directly reassigning `df.columns` for wholesale schema updates; column types are cast using `.astype()` to ensure optimal memory allocation.

#### Code Demonstration

```python
import pandas as pd

df = pd.DataFrame({
    "Monthly Salary": [50000, 60000],
    "emp age": ["25", "30"]
})

# 1. Safe Selective Rename (Returns a new DataFrame)
df = df.rename(columns={
    "Monthly Salary": "monthly_salary",
    "emp age": "age"
})

# 2. Type Casting for Model Compliance
df["age"] = df["age"].astype("int32")
df["monthly_salary"] = df["monthly_salary"].astype("float32")

# 3. Standardizing all column names (Best Practice: lowercase, snake_case)
df.columns = df.columns.str.strip().str.lower().str.replace(" ", "_")

print(df.dtypes)
```

---

### Q6: How Do You Filter DataFrames Using Boolean Indexing and Query Conditions?

> **Interview-Ready Answer:**  
> DataFrames are filtered using boolean indexing by applying comparison masks inside bracket notation (`df[condition]`); compound conditions require bitwise operators (`&` for AND, `|` for OR, `~` for NOT) with each condition wrapped in parentheses to enforce operator precedence.

#### Code Demonstration

```python
import pandas as pd

df = pd.DataFrame({
    "name": ["Krishna", "Rahul", "Aman", "Priya"],
    "salary": [50000, 60000, 55000, 70000],
    "city": ["Indore", "Delhi", "Indore", "Mumbai"]
})

# 1. Single Condition
high_salary = df[df["salary"] > 55000]

# 2. Multiple Conditions: AND (&)
indore_and_high_salary = df[(df["city"] == "Indore") & (df["salary"] >= 50000)]

# 3. Multiple Conditions: OR (|)
metro_cities = df[(df["city"] == "Delhi") | (df["city"] == "Mumbai")]

# 4. Using .isin() for multi-value membership
filtered_isin = df[df["city"].isin(["Delhi", "Mumbai"])]

# 5. Using df.query() syntax
query_result = df.query('salary > 55000 and city == "Mumbai"')

print("Query Result:\n", query_result)
```

> ⚠️ **Common Bug:** Using Python keywords `and` / `or` instead of `&` / `|` inside DataFrame brackets raises `ValueError: The truth value of a Series is ambiguous`.

---

### Q7: How Does sort_values() Work for Single and Multi-Column Sorting?

> **Interview-Ready Answer:**  
> `sort_values(by=..., ascending=...)` reorders DataFrame rows based on column values; multi-column sorting is executed by passing lists of column names to `by` and corresponding booleans to `ascending` to establish hierarchical sorting order.

#### Code Demonstration

```python
import pandas as pd

df = pd.DataFrame({
    "city": ["Indore", "Delhi", "Indore", "Mumbai", "Delhi"],
    "salary": [50000, 60000, 55000, 70000, 45000]
})

# 1. Single column descending sort
descending_salary = df.sort_values(by="salary", ascending=False)

# 2. Multi-column hierarchical sort: City A-Z, Salary High-Low
hierarchical_sort = df.sort_values(
    by=["city", "salary"],
    ascending=[True, False]
)

print(hierarchical_sort)
```

---

### Q8: How Does groupby() Work and How Do You Execute Multiple Aggregations?

> **Interview-Ready Answer:**  
> `groupby()` implements the Split-Apply-Combine pattern: it splits data into groups based on key columns, applies aggregate functions (`mean`, `sum`, `count`, `std`) independently to each group, and combines the results into a summarized DataFrame.

#### Split-Apply-Combine Pattern

```text
Input DataFrame ──► SPLIT by 'city' ──► APPLY 'mean(salary)' ──► COMBINE
Indore: 50K, 55K     Indore group        52.5K                     Indore: 52.5K
Delhi:  60K, 45K     Delhi group         52.5K                     Delhi:  52.5K
Mumbai: 70K          Mumbai group        70.0K                     Mumbai: 70.0K
```

#### Code Demonstration

```python
import pandas as pd

df = pd.DataFrame({
    "city": ["Indore", "Delhi", "Indore", "Mumbai", "Delhi"],
    "salary": [50000, 60000, 55000, 70000, 45000],
    "bonus": [5000, 6000, 5500, 7000, 4500]
})

# 1. Simple Single Aggregation
avg_salary_by_city = df.groupby("city")["salary"].mean()

# 2. Multi-Metric Aggregation with .agg()
city_summary = df.groupby("city").agg({
    "salary": ["mean", "max", "count"],
    "bonus": ["sum"]
})

print("City Summary:\n", city_summary)
```

---

### Q9: How Do You Engineer New Feature Columns and Export Cleaned Data to CSV?

> **Interview-Ready Answer:**  
> New feature columns are engineered using vectorized arithmetic, conditional transformations (`np.where`), or `.apply()` functions; cleaned datasets are serialized to disk using `.to_csv('filename.csv', index=False)` to prevent writing extraneous integer index columns.

#### Code Demonstration

```python
import pandas as pd
import numpy as np

df = pd.DataFrame({
    "salary": [50000, 60000, 80000, 40000],
    "experience": [2, 4, 8, 1]
})

# 1. Vectorized Arithmetic: 10% annual bonus column
df["bonus"] = df["salary"] * 0.10

# 2. Vectorized Feature Engineering: Ratio feature
df["salary_per_year_exp"] = df["salary"] / df["experience"]

# 3. Conditional Feature Creation using np.where
df["is_senior"] = np.where(df["experience"] >= 5, 1, 0)

# 4. Safe CSV Export (index=False avoids Unnamed: 0 column on reload)
df.to_csv("cleaned_features.csv", index=False)

print(df.head())
```

---

### Q10: What Is the Difference Between loc and iloc in Pandas?

> **Interview-Ready Answer:**  
> `.loc` selects data by **label** (explicit row index labels and column names) and uses inclusive stop bounds `[start:stop]`, whereas `.iloc` selects data strictly by **integer position** (zero-indexed row and column offsets) and uses exclusive stop bounds `[start:stop]`.

#### Comparison Table

| Attribute | `df.loc[]` | `df.iloc[]` |
| :--- | :--- | :--- |
| **Indexing Basis** | Label-based (strings, dates, named indices) | Integer-based position ($0, 1, \dots, N-1$) |
| **Stop Slice Bound** | **Inclusive** (`'A':'C'` includes `'C'`) | **Exclusive** (`0:3` includes indices $0, 1, 2$) |
| **Column Selection** | Column names: `df.loc[:, 'salary':'bonus']` | Column integer offsets: `df.iloc[:, 0:2]` |
| **Boolean Masking** | Fully supported (`df.loc[df['salary'] > 50000]`) | Requires boolean arrays converted to positions |

```python
# loc: Label-based (rows 0 to 2 inclusive, column 'salary')
val_loc = df.loc[0:2, "salary"]

# iloc: Position-based (rows 0 and 1 only, column 0 only)
val_iloc = df.iloc[0:2, 0]
```

---

### Q11: What Is the Difference Between a Pandas Series and a DataFrame?

> **Interview-Ready Answer:**  
> A `Series` is a one-dimensional labeled array capable of holding any data type with an associated index, while a `DataFrame` is a two-dimensional tabular data structure composed of an ordered collection of columns, where each individual column is a `Series` sharing a common index.

#### Structural Comparison

| Feature | `pd.Series` | `pd.DataFrame` |
| :--- | :--- | :--- |
| **Dimensionality** | 1D array (`shape = (N,)`) | 2D table (`shape = (M, N)`) |
| **Constituents** | Values + Index | Collection of named Series sharing index |
| **Access Syntax** | `series[index]` | `df[column_name][row_index]` or `df.loc[r, c]` |
| **ML Role** | Typically represents the target label $y$ | Typically represents the feature matrix $X$ |

---

### Q12: What Does the Complete Production Machine Learning Preprocessing Pipeline Look Like?

> **Interview-Ready Answer:**  
> A production ML preprocessing pipeline systematically transforms raw heterogeneous data into a normalized numeric feature matrix through five sequential phases: Data Ingestion & Deduplication, Missing Value Imputation, Categorical Encoding (One-Hot / Target), Feature Scaling (Standard / MinMax), and Feature Matrix Export to NumPy arrays.

#### End-to-End Code Template

```python
import pandas as pd
import numpy as np
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler, OneHotEncoder
from sklearn.compose import ColumnTransformer

# 1. Ingestion
raw_data = {
    'age': [25, 30, np.nan, 35, 40],
    'department': ['Engineering', 'Sales', 'Engineering', 'HR', 'Sales'],
    'salary': [70000, 65000, 80000, 60000, 95000],
    'promoted': [0, 1, 1, 0, 1]
}
df = pd.DataFrame(raw_data)

# 2. Imputation
df['age'] = df['age'].fillna(df['age'].median())

# 3. Separation of Features (X) and Target (y)
X = df[['age', 'department', 'salary']]
y = df['promoted'].to_numpy()

# 4. Train-Test Split (BEFORE feature scaling to prevent data leakage!)
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)

# 5. Preprocessor: Standardize numerics, One-Hot encode categoricals
numeric_features = ['age', 'salary']
categorical_features = ['department']

preprocessor = ColumnTransformer(
    transformers=[
        ('num', StandardScaler(), numeric_features),
        ('cat', OneHotEncoder(drop='first'), categorical_features)
    ]
)

# 6. Fit strictly on training set, transform both
X_train_processed = preprocessor.fit_transform(X_train)
X_test_processed = preprocessor.transform(X_test)

print("Processed Training Matrix Shape:", X_train_processed.shape)
```