
import pandas as pd
import matplotlib.pyplot as plt

# Load dataset
df = pd.read_csv("data.csv")

print("First 5 rows:")
print(df.head())

# Check missing values
print("\nMissing Values:")
print(df.isnull().sum())

# Fill missing values
for column in df.select_dtypes(include='number').columns:
    df[column].fillna(df[column].mean(), inplace=True)

for column in df.select_dtypes(include='object').columns:
    df[column].fillna(df[column].mode()[0], inplace=True)

# Remove duplicate rows
df.drop_duplicates(inplace=True)

# Remove outliers using IQR method
numeric_columns = df.select_dtypes(include='number').columns

for col in numeric_columns:
    Q1 = df[col].quantile(0.25)
    Q3 = df[col].quantile(0.75)
    IQR = Q3 - Q1

    lower = Q1 - 1.5 * IQR
    upper = Q3 + 1.5 * IQR

    df = df[(df[col] >= lower) & (df[col] <= upper)]

print("\nCleaned Data:")
print(df.head())

# Save cleaned dataset
df.to_csv("cleaned_data.csv", index=False)

# Visualization 1: Histogram
for col in numeric_columns:
    plt.figure(figsize=(6,4))
    plt.hist(df[col], bins=10)
    plt.title(f"{col} Distribution")
    plt.xlabel(col)
    plt.ylabel("Frequency")
    plt.show()

# Visualization 2: Bar chart (if categorical column exists)
categorical = df.select_dtypes(include='object').columns

if len(categorical) > 0:
    df[categorical[0]].value_counts().plot(kind='bar', figsize=(6,4))
    plt.title(f"{categorical[0]} Count")
    plt.xlabel(categorical[0])
    plt.ylabel("Count")
    plt.show()

print("\nProject Completed Successfully!")

output:
<img width="600" height="400" alt="Image" src="https://github.com/user-attachments/assets/b2a3b060-1074-4099-a75b-4dbdce58927a" />
