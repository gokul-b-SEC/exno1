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
import seaborn as sns
```
```
# Step 1: Read the Data
df = pd.read_csv('Data_set.csv')
df

# Step 2: Get basic information
df.info()
df.describe()

# Step 3: Check null values
df_null = df.isnull()
df_null

df_null_sum = df.isnull().sum()
df_null_sum

# Fill Null Values with Constant Value 0
df_nafill_0 = df.fillna(0)
df_nafill_0

# Fill Null Values with ffill Method
df_ffill = df.ffill()
df_ffill

# Fill Null Values with bfill Method
df_bfill = df.bfill()
df_bfill

# Fill Null Values with Mean
df_mean = df.copy()

for col in df_mean.select_dtypes(include=np.number).columns:
    df_mean[col] = df_mean[col].fillna(df_mean[col].mean())

df_mean

# Drop Null Values
df_dropna = df.dropna()
df_dropna

# Step 4: Save Clean Data
clean_data = df_mean
clean_data.to_csv('clean_data.csv', index=False)
```
<img width="593" height="217" alt="image" src="https://github.com/user-attachments/assets/afd51c37-cb6f-4a5a-9ef8-adecb8902ae7" />
```
# Step 1: Read the Data
df = pd.read_csv('SAMPLEIDS.csv')
df

# Step 2: Get basic information
df.info()
df.describe()

# Step 3: Check null values
df.isnull().sum()

# Step 4: Check duplicate values
df.duplicated().sum()

# Display duplicate rows
df[df.duplicated()]

# Remove duplicate values
df = df.drop_duplicates()

# Step 5: Inspect inconsistent values
for col in df.columns:
    print("\nColumn:", col)
    print(df[col].value_counts(dropna=False))
    print(df[col].unique())

# Step 6: Save Clean Data
df.to_csv('clean_SAMPLEIDS.csv', index=False)
```
<img width="597" height="786" alt="image" src="https://github.com/user-attachments/assets/f1762e28-72e6-4361-9999-4151878a866b" />
<img width="595" height="481" alt="image" src="https://github.com/user-attachments/assets/8228d5d3-26c9-4a88-9e7a-ed14202a7506" />


```
# Step 1: Read the Data
df = pd.read_csv('heights.csv')
df

# Step 2: Get basic information
df.info()
df.describe()

# Step 3: Check null values
df.isnull().sum()

# Step 4: Detect Outliers using Boxplot
sns.boxplot(data=df.select_dtypes(include=np.number))

# Step 5: Detect Outliers using Scatterplot
sns.scatterplot(data=df.select_dtypes(include=np.number))

# Step 6: Calculate IQR
for col in df.select_dtypes(include=np.number).columns:
    q1 = df[col].quantile(0.25)
    q3 = df[col].quantile(0.75)
    iqr = q3 - q1

    print("\nColumn:", col)
    print("IQR:", iqr)

    rid = df[(df[col] < (q1 - 1.5 * iqr)) |
             (df[col] > (q3 + 1.5 * iqr))]

    print("Outliers:")
    print(rid[col])

    delid = df[~((df[col] < (q1 - 1.5 * iqr)) |
                 (df[col] > (q3 + 1.5 * iqr)))]

    print("Data after removing outliers:")
    print(delid)
```
<img width="595" height="483" alt="image" src="https://github.com/user-attachments/assets/3d857f35-38a7-4552-a601-9c20830cbfc4" />

```
# Step 1: Read the Data
ir = pd.read_csv('iris.csv')
ir

# Step 2: Get basic information
ir.info()
ir.describe()

# Step 3: Detect Outliers using Boxplot
sns.boxplot(data=ir.select_dtypes(include=np.number))

# Step 4: Detect Outliers using Scatterplot
sns.scatterplot(data=ir.select_dtypes(include=np.number))

# Step 5: Remove Outliers using IQR Method
ir_clean = ir.copy()

for col in ir.select_dtypes(include=np.number).columns:

    q1 = ir_clean[col].quantile(0.25)
    q3 = ir_clean[col].quantile(0.75)
    iqr = q3 - q1

    print("\nColumn:", col)
    print("IQR:", iqr)

    rid = ir_clean[(ir_clean[col] < (q1 - 1.5 * iqr)) |
                   (ir_clean[col] > (q3 + 1.5 * iqr))]

    print("Outliers:")
    print(rid[col])

    ir_clean = ir_clean[(ir_clean[col] >= (q1 - 1.5 * iqr)) &
                        (ir_clean[col] <= (q3 + 1.5 * iqr))]

print("Data after IQR:")
ir_clean

# Step 6: Remove Outliers using Z-score Method
numeric_data = ir.select_dtypes(include=np.number)

z = np.abs(stats.zscore(numeric_data))

threshold = 3

outliers = numeric_data[(z > threshold).any(axis=1)]

print("Outliers:")
print(outliers)

# Remove Outliers
ir_zclean = ir[(z <= threshold).all(axis=1)]

print("Data after Z-score:")
ir_zclean
```
<img width="600" height="750" alt="image" src="https://github.com/user-attachments/assets/2f9db559-faca-43c1-b7eb-a84cc0f7cf06" />

```
# Step 1: Read the Data
df = pd.read_csv('Loan_data.csv')
df

# Step 2: Get basic information
df.info()
df.describe()

# Step 3: Check null values
df.isnull().sum()

# Step 4: Separate Numerical and Categorical Columns
num_cols = df.select_dtypes(include=np.number).columns
cat_cols = df.select_dtypes(include=['object', 'str']).columns

# Step 5: Fill Numerical Null Values with Mean
for col in num_cols:
    df[col] = df[col].fillna(df[col].mean())

# Step 6: Fill Categorical Null Values with Mode
for col in cat_cols:
    df[col] = df[col].fillna(df[col].mode()[0])

# Step 7: Remove Duplicate Values
df = df.drop_duplicates()

# Step 8: Check Null Values Again
df.isnull().sum()

# Step 9: Display Clean Data
df

# Step 10: Save Clean Data
df.to_csv('clean_Loan_data.csv', index=False)
```
<img width="600" height="783" alt="image" src="https://github.com/user-attachments/assets/dca3a5ea-7414-4169-870e-c78998863eaa" />
<img width="603" height="496" alt="image" src="https://github.com/user-attachments/assets/9046ac48-2e14-41a1-8410-ff0d9ef892aa" />



# Result

Successfully performed data cleaning, handled missing values and duplicates, and detected outliers using IQR and Z-score method
