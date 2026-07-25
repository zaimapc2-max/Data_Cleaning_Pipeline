# Data Cleaning Pipeline: Top-Grossing Concert Tours

A data cleaning project that takes a raw, messy dataset of the world's top-grossing
concert tours and transforms it into an analysis-ready dataset — cleaned, standardized,
and exported to both CSV and Excel.

## Overview

This project simulates a common freelance request: a client hands over a raw export
with missing data, inconsistent formatting, and wrong data types, and needs it turned
into something usable. The full process — from initial inspection to final export —
is documented in `dcp_notebook.ipynb`.

**Dataset:** Top 20 highest-grossing concert tours, including rank, artist, tour name,
revenue figures, number of shows, and year.

**Source:** [Dirty Dataset to Practice Data Cleaning (Kaggle)](https://www.kaggle.com/datasets/amruthayenikonda/dirty-dataset-to-practice-data-cleaning)

## Damage Report (Initial State)

| | |
|---|---|
| Rows | 20 |
| Columns | 11 |
| Data types | 2 numeric, 9 string (revenue columns stored as text due to currency symbols and commas) |
| Duplicate rows | 0 |
| Missing values | Present in **Peak** and **All Time Peak** only |

**Initial observations:**
- Taylor Swift appears multiple times in the dataset
- Revenue columns need cleaning before they can be used numerically
- Number of shows varies widely across tours
- Ranking information (Peak / All Time Peak) is unavailable for a large share of tours

## Cleaning Steps

### 1. Duplicate Rows
Checked with `.duplicated()` — no duplicate rows were found, so no rows were dropped
at this stage.

### 2. Missing Values
`Peak` and `All Time Peak` were the only columns with missing data:

- **Peak** — 11 missing values (55%)
- **All Time Peak** — 14 missing values (70%)

**Decision: both columns were dropped.**

> Given the small dataset size (20 rows), imputing values for columns this sparse
> would mean fabricating more data than actually exists, rather than genuinely
> cleaning it. Dropping the affected rows instead would shrink the dataset to as
> few as 9 usable rows — too small to analyze meaningfully. Dropping both columns
> was the more defensible choice.

### 3. Inconsistent Formatting
Text columns (`Artist`, `Tour title`) were stripped of leading/trailing whitespace
and standardized to title case, so entries like `"taylor swift "` and `"TAYLOR SWIFT"`
are treated as the same value.

### 4. Data Types
Revenue-related columns (`Actual gross`, `Adjusted gross (in 2022 dollars)`,
`Average gross`, `Ref.`) were stored as text due to currency symbols and commas
(e.g., `"$780,000,000"`). These were stripped of non-numeric characters and
converted to proper numeric types so they can be used in calculations and analysis.

### 5. Final Check
Re-ran `.shape`, `.columns`, `.info()`, and `.describe()` to confirm the dataset
was structurally sound after cleaning — correct types, no unexpected columns,
and shape reflecting the two dropped columns.

## Results

| | Before | After |
|---|---|---|
| Rows | 20 | 20 |
| Columns | 11 | 9 |
| Missing values | 25 (across 2 columns) | 0 |
| Revenue columns | Text (with symbols) | Numeric |

## Files

```
├── data/
│   └── raw_data.csv              # original, unmodified dataset
├── notebook/
│   └── dcp_notebook.ipynb        # full cleaning process, step by step
├── output/
│   ├── clean_data.csv            # cleaned dataset (CSV)
│   └── cleaned_dataset.xlsx      # cleaned dataset (Excel)
└── README.md
```

## Tools Used
Python, Pandas, NumPy, Jupyter Notebook

## Key Takeaway
Not every column can — or should — be salvaged. Part of cleaning a dataset
responsibly is recognizing when a column is too sparse to impute honestly, and
documenting that decision rather than forcing a fix that would introduce more
noise than value.
