# Pandas Quick Reference (Beginner Friendly)

## What is Pandas?

**Pandas** is a Python library used for working with **structured data (tables)**.

It is commonly used for:

* Data cleaning
* Data transformation
* Data exploration
* Data analysis

Think of Pandas like **Excel inside Python**, but much more powerful.

Typical workflow:

Load data → Inspect data → Clean data → Transform data → Analyze data

---

# Importing Pandas

Pandas is usually imported with the alias `pd`.

```python
import pandas as pd
```

---

# Core Pandas Objects

Pandas primarily uses two data structures.

---

## 1. Series

A **Series** is a **one-dimensional labeled array**.

Think of it like **one column in a spreadsheet**.

Example:

```python
s = pd.Series([10, 20, 30])
```

Conceptual output:

```
0    10
1    20
2    30
```

Structure:

```
index | value
```

Custom index example:

```python
s = pd.Series([10,20,30], index=["a","b","c"])
```

```
a    10
b    20
c    30
```

---

## 2. DataFrame

A **DataFrame** is the main pandas object.

It represents a **table of rows and columns**.

Comparable to:

* Excel spreadsheet
* SQL table
* CSV file

Example:

```python
data = {
    "name": ["Alice","Bob","Charlie"],
    "age": [25,30,35]
}

df = pd.DataFrame(data)
```

Conceptual output:

```
   name      age
0  Alice     25
1  Bob       30
2  Charlie   35
```

Structure:

```
       columns
       ↓
index  name   age
  ↓
0      Alice  25
1      Bob    30
2      Charlie 35
```

---

# Loading Data

Most projects begin by loading data.

## CSV

```python
df = pd.read_csv("file.csv")
```

Common arguments:

| Argument      | Meaning                  |
| ------------- | ------------------------ |
| `sep=","`     | delimiter                |
| `header=0`    | row used as column names |
| `index_col=0` | column used as index     |

Example:

```python
df = pd.read_csv("data.csv", index_col=0)
```

---

## Excel

```python
df = pd.read_excel("file.xlsx")
```

---

## JSON

```python
df = pd.read_json("file.json")
```

---

# Inspecting Data

These functions are used constantly when exploring datasets.

### View first rows

```python
df.head()
```

Default: first **5 rows**

```python
df.head(10)
```

---

### View last rows

```python
df.tail()
```

---

### Dataset info

```python
df.info()
```

Displays:

* column names
* data types
* non-null counts
* memory usage

---

### Summary statistics

```python
df.describe()
```

Shows statistics for numeric columns:

* count
* mean
* std
* min
* quartiles
* max

---

### Column names

```python
df.columns
```

---

### Index

```python
df.index
```

---

# Selecting Data

Selecting rows and columns is a core pandas skill.

---

## Selecting Columns

Single column:

```python
df["age"]
```

Returns a **Series**.

Multiple columns:

```python
df[["name","age"]]
```

Returns a **DataFrame**.

---

## Selecting Rows

By index position:

```python
df.iloc[0]
```

Multiple rows:

```python
df.iloc[0:3]
```

Row and column:

```python
df.iloc[0,1]
```

---

## Label-Based Selection

Use `.loc`

```python
df.loc[0]
```

Example:

```python
df.loc[0,"name"]
```

---

# Filtering Data

Filtering uses **boolean conditions**.

Example:

```python
df[df["age"] > 30]
```

Multiple conditions:

```python
df[(df["age"] > 25) & (df["age"] < 40)]
```

Important: wrap conditions in parentheses.

---

# Sorting Data

Sort by column:

```python
df.sort_values("age")
```

Descending:

```python
df.sort_values("age", ascending=False)
```

---

# Adding Columns

Create new columns:

```python
df["age_plus_one"] = df["age"] + 1
```

---

# Removing Columns

```python
df.drop("age", axis=1)
```

Arguments:

| Argument | Meaning |
| -------- | ------- |
| `axis=1` | column  |
| `axis=0` | row     |

Permanent change:

```python
df.drop("age", axis=1, inplace=True)
```

---

# Handling Missing Data

Missing values appear as:

```
NaN
```

Detect missing values:

```python
df.isna()
```

Count missing values:

```python
df.isna().sum()
```

Remove rows with missing data:

```python
df.dropna()
```

Fill missing values:

```python
df.fillna(0)
```

Example:

```python
df["age"].fillna(df["age"].mean())
```

---

# Grouping Data

Used for aggregations.

Example dataset:

```
department  salary
IT          100
IT          120
HR          90
```

Group by department:

```python
df.groupby("department").mean()
```

Common aggregation functions:

```
mean()
sum()
count()
min()
max()
```

---

# Value Counts

Counts occurrences of unique values.

```python
df["department"].value_counts()
```

---

# Applying Functions

```python
df["age"].apply(lambda x: x + 1)
```

---

# Renaming Columns

```python
df.rename(columns={"age":"Age"})
```

Permanent change:

```python
df.rename(columns={"age":"Age"}, inplace=True)
```

---

# Saving Data

Save as CSV:

```python
df.to_csv("clean_data.csv")
```

Save as Excel:

```python
df.to_excel("clean_data.xlsx")
```

---

# Typical Pandas Workflow

```python
import pandas as pd

df = pd.read_csv("data.csv")

df.head()
df.info()

df = df.dropna()

df["new_col"] = df["col1"] + df["col2"]

result = df.groupby("category").mean()

result.to_csv("result.csv")
```

---

# Pandas Function Cheat Sheet

| Function         | Purpose                |
| ---------------- | ---------------------- |
| `read_csv()`     | Load CSV file          |
| `head()`         | Preview data           |
| `info()`         | Dataset structure      |
| `describe()`     | Summary statistics     |
| `loc[]`          | Select by label        |
| `iloc[]`         | Select by index        |
| `groupby()`      | Aggregate data         |
| `sort_values()`  | Sort rows              |
| `drop()`         | Remove rows/columns    |
| `fillna()`       | Replace missing values |
| `value_counts()` | Count values           |

---

# Key Concepts

1. **Series = single column**
2. **DataFrame = table**
3. **Index = row labels**
4. **Columns = variables**
5. **Vectorized operations are fast**

Example:

```python
df["age"] + 1
```

This applies the operation to the **entire column at once**.
