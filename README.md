# Medical-Visit

## Data Cleaning and Preprocessing: Medical Visit Dataset

This script outlines the cleaning and preprocessing steps applied to the raw `Medical visit.csv` dataset using **Pandas** and **NumPy**. The goal is to ensure data quality, consistency, and correct data types before analysis or modeling.

### Setup

This script requires the following Python libraries:

```bash
pip install pandas numpy
```

### 📋 Cleaning Steps Overview

The script addresses several data quality issues:

#### 1\. Data Integrity Checks

| Issue | Action Taken |
| :--- | :--- |
| **Missing Values** | Checked using `df.isnull().sum()`. (Result: None found.) |
| **Duplicates** | Counted and identified fully duplicate rows. |

-----

#### 2\. Data Type & Column Standardization

| Column | Original Type | Action Taken |
| :--- | :--- | :--- |
| `ScheduledDay` | object | Converted to **datetime**. |
| `AppointmentDay` | object | Converted to **datetime** and then truncated to **date only**. |
| `PatientId` | float | Converted to **`int64`**. |
| `AppointmentID` | int64 | Converted to **`string`**. |

**Column Renaming:** Renamed columns for clarity and consistency:

  * `Hipertension` $\rightarrow$ **`Hypertension`**
  * `Handcap` $\rightarrow$ **`Handicap`**
  * `No-show` $\rightarrow$ **`NoShow`**

-----

#### 3\. Categorical & Text Cleaning

  * **Gender:** Converted to **uppercase** (`F`, `M`) and stripped whitespace.
  * **Neighbourhood / NoShow:** Converted to **lowercase** and stripped whitespace for uniform categories.

-----

#### 4\. Outlier Filtering

  * **Age:** Rows with clearly erroneous ages (e.g., **Age $< 0$** or **Age $> 100$**) were filtered and removed to maintain a sensible range.

### Python Script

Save the following code as a Python file (e.g., `data_cleaning.py`) and place it alongside your `Medical visit.csv` file.

```python
import pandas as pd
import numpy as np

# --- 1. SETUP: Load Data ---
df = pd.read_csv('Medical visit.csv')

# --- 2. Duplicates and Missing Values ---
# Check and remove duplicates (currently commented out but provided)
# df.drop_duplicates(inplace=True)

# --- 3. Data Type Conversion & Column Cleanup ---

# Convert date columns to datetime
df['ScheduledDay'] = pd.to_datetime(df['ScheduledDay'], errors='coerce')
df['AppointmentDay'] = pd.to_datetime(df['AppointmentDay'], errors='coerce')
# Remove time component from AppointmentDay
df['AppointmentDay'] = df['AppointmentDay'].dt.date

# Convert ID columns
df['PatientId'] = df['PatientId'].astype(np.int64)
df['AppointmentID'] = df['AppointmentID'].astype(str)

# Rename columns
df.rename(columns={
    'Hipertension': 'Hypertension',
    'Handcap': 'Handicap',
    'No-show': 'NoShow'
}, inplace=True)

# --- 4. Cleaning Text and Categorical Data ---

# Standardize text fields (lowercase and strip whitespace)
df['Neighbourhood'] = df['Neighbourhood'].str.strip().str.lower()
df['NoShow'] = df['NoShow'].str.strip().str.lower()

# Standardize Gender (uppercase and strip whitespace)
df['Gender'] = df['Gender'].str.strip().str.lower().str.upper()

# --- 5. Outlier/Range Filtering ---

# Filter rows where 'Age' is outside a sensible range (0-100)
df = df[(df['Age'] >= 0) & (df['Age'] <= 100)]

# --- 6. Verification ---
print("\n--- Final Cleaned Data Check ---")
df.info()
print("\nFirst 5 rows after cleaning:")
print(df.head())
```
