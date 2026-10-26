# Exno:1
Data Cleaning Process

# AIM
To read the given data and perform data cleaning and save the cleaned data to a file.

# Explanation
Data cleaning is the process of preparing data for analysis by removing or modifying data that is incorrect ,incompleted , irrelevant , duplicated or improperly formatted. Data cleaning is not simply about erasing data ,but rather finding a way to maximize datasets accuracy without necessarily deleting the information.

# Algorithm
STEP 1: Read the given Data

STEP 2: Get the information about the data

STEP 3: Remove the null values from the data

STEP 4: Save the Clean data to the file

STEP 5: Remove outliers using IQR

STEP 6: Use zscore of to remove outliers

# Coding and Output
```
import pandas as pd
import numpy as np
from scipy import stats
import matplotlib.pyplot as plt
```
```
df = pd.read_csv("Data_set.csv")

print(df.head())
print(df.info())
print(df.isnull().sum())

for col in df.select_dtypes(include="number").columns:
    df[col] = df[col].fillna(df[col].median())

for col in df.select_dtypes(include=["object", "str"]).columns:
    if df[col].isnull().any() and not df[col].mode().empty:
        df[col] = df[col].fillna(df[col].mode()[0])

print("\nAfter cleaning:\n", df.isnull().sum())
df.to_csv("cleaned_Data_set.csv", index=False)
print("Saved successfully!")
```
<img width="596" height="788" alt="image" src="https://github.com/user-attachments/assets/a4869392-4c37-4830-a1b7-59329ba56fdd" />

```
df = pd.read_csv("SAMPLEIDS.csv")

print(df.head())
print("Missing values:\n", df.isnull().sum())
print("Duplicate rows:", df.duplicated().sum())

df = df.drop_duplicates()

# Inspect categorical inconsistencies
for col in df.select_dtypes(include=["object", "str"]).columns:
    print(f"\n{col}:", df[col].value_counts(dropna=False))

# Standardize text
for col in df.select_dtypes(include=["object", "str"]).columns:
    df[col] = df[col].str.strip().str.upper()

# Fill missing values
for col in df.select_dtypes(include="number").columns:
    df[col] = df[col].fillna(df[col].median())

for col in df.select_dtypes(include=["object", "str"]).columns:
    if df[col].isnull().any() and not df[col].mode().empty:
        df[col] = df[col].fillna(df[col].mode()[0])

print("\nRemaining missing values:\n", df.isnull().sum())
df.to_csv("cleaned_SAMPLEIDS.csv", index=False)
print("Saved successfully!")
```
<img width="597" height="786" alt="image" src="https://github.com/user-attachments/assets/f1762e28-72e6-4361-9999-4151878a866b" />
<img width="595" height="481" alt="image" src="https://github.com/user-attachments/assets/8228d5d3-26c9-4a88-9e7a-ed14202a7506" />


```
df = pd.read_csv("heights.csv")
print(df.head())
print(df.describe())

col = "height"  # Change if your column name differs

Q1 = df[col].quantile(0.25)
Q3 = df[col].quantile(0.75)
IQR = Q3 - Q1

lower = Q1 - 1.5 * IQR
upper = Q3 + 1.5 * IQR

outliers = df[(df[col] < lower) | (df[col] > upper)]
print("Unusual measurements:\n", outliers)

plt.boxplot(df[col].dropna())
plt.title("Height Outlier Detection")
plt.show()

df["is_outlier"] = (df[col] < lower) | (df[col] > upper)
df.to_csv("cleaned_heights.csv", index=False)
print("Saved successfully!")
```
<img width="595" height="483" alt="image" src="https://github.com/user-attachments/assets/3d857f35-38a7-4552-a601-9c20830cbfc4" />

```
df = pd.read_csv("iris.csv")
print(df.head())
print(df.info())

for col in df.select_dtypes(include="number").columns:
    Q1 = df[col].quantile(0.25)
    Q3 = df[col].quantile(0.75)
    IQR = Q3 - Q1

    outliers_iqr = df[(df[col] < Q1 - 1.5 * IQR) |
                      (df[col] > Q3 + 1.5 * IQR)]
    print(f"\nIQR outliers in {col}:\n", outliers_iqr[[col]])

    z = np.abs(stats.zscore(df[col], nan_policy="omit"))
    print(f"Z-score outliers in {col}:\n", df[z > 3][[col]])

df.to_csv("cleaned_iris.csv", index=False)
print("Saved successfully!")
```
<img width="600" height="750" alt="image" src="https://github.com/user-attachments/assets/2f9db559-faca-43c1-b7eb-a84cc0f7cf06" />

```
df = pd.read_csv("Loan_data.csv")

print(df.head())
print(df.info())
print("Missing values:\n", df.isnull().sum())
print("Duplicates:", df.duplicated().sum())

df = df.drop_duplicates()

# Fill numerical missing values
for col in df.select_dtypes(include="number").columns:
    df[col] = df[col].fillna(df[col].median())

# Fill categorical missing values
for col in df.select_dtypes(include=["object", "str"]).columns:
    if df[col].isnull().any() and not df[col].mode().empty:
        df[col] = df[col].fillna(df[col].mode()[0])

# IQR and Z-score on ApplicantIncome
col = "ApplicantIncome"

Q1 = df[col].quantile(0.25)
Q3 = df[col].quantile(0.75)
IQR = Q3 - Q1

outliers_iqr = df[(df[col] < Q1 - 1.5 * IQR) |
                  (df[col] > Q3 + 1.5 * IQR)]
print("\nIQR outliers:\n", outliers_iqr[[col]])

z = np.abs(stats.zscore(df[col], nan_policy="omit"))
print("\nZ-score outliers:\n", df[z > 3][[col]])

df.to_csv("cleaned_Loan_data.csv", index=False)
print("Saved successfully!")
```
<img width="600" height="783" alt="image" src="https://github.com/user-attachments/assets/dca3a5ea-7414-4169-870e-c78998863eaa" />
<img width="603" height="496" alt="image" src="https://github.com/user-attachments/assets/9046ac48-2e14-41a1-8410-ff0d9ef892aa" />



# Result

Successfully performed data cleaning, handled missing values and duplicates, and detected outliers using IQR and Z-score method
