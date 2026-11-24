# Obesity Levels ETL Pipeline

## Objective
Build an ETL pipeline that turns the raw UCI obesity survey dataset into a clean, analysis-ready dataset (plus summary reports) for future Data Science projects.

## Data Source
UCI Machine Learning Repository – *Estimation of Obesity Levels Based on Eating Habits and Physical Condition* (CSV, 2,111 records × 17 columns, structured, static dataset). Data from Mexico, Peru and Colombia; ~77 % synthetic (SMOTE), ~23 % from a web survey.

## ETL Workflow
```
Data Source (UCI CSV)
        │
        ▼
     Extract        -> download/read, show records & columns, save raw copy (raw_data/)
        │
        ▼
  Validate Data     -> range checks, remove impossible values
        │
        ▼
    Clean Data      -> duplicates, rename, standardize text, fix data types
        │
        ▼
 Transform Data     -> BMI, categories, labels, sort, rank
        │
        ▼
Generate Reports    -> grouping, aggregation, summary statistics
        │
        ▼
      Load          -> output/ (clean dataset) and reports/ (all reports)
```

## List of Transformations (and why)
| # | Transformation | Why it was necessary |
|---|---|---|
| 1 | Remove duplicate records (24 found) | Duplicates bias statistics and ML models |
| 2 | Rename columns (e.g. `FAVC` → `high_calorie_food`) | Original names are cryptic abbreviations |
| 3 | Standardize text (strip, remove underscores, consistent casing) | Consistent labels for grouping and filtering |
| 4 | Change data types (decimals → integers for age; | Survey answers are whole-number scales; decimals came from synthetic generation |
| 5 | Validation – remove out-of-range values | Guards against impossible ages, heights, weights and scale values |
| 6 | Calculated column: BMI = weight / height² | Standard health metric, useful feature for analysis |
| 7 | Create categories: `age_group`, `bmi_category` (WHO bands) | Enables group-wise analysis |
| 8 | Sort records by BMI (descending) | Makes extremes easy to find |
| 9 | Grouping / aggregation and summary statistics | Powers the category-wise reports |

## Reports Generated (`reports/`)
- `cleaned_data.csv` – complete cleaned dataset
- `summary_statistics.csv` – describe() for numeric columns
- `obesity_level_report.csv` – category-wise report
- `gender_report.csv`, – gender-wise report
- `transport_report.csv` – transport-wise report

The final analysis-ready file is saved as `output/cleaned_data.csv`.

## Challenges Faced
- Cryptic column names and non-integer values in fields that should be whole numbers.
- Dataset is largely synthetic, so findings should not be treated as real-world prevalence.

## Learning Outcomes
- Designing a modular Extract → Transform → Load pipeline.
- Practical data cleaning, validation and feature engineering with pandas.
- Generating reproducible reports and documenting each transformation.

## Future Improvements
- Add logging and unit tests for each step.
- Add charts (BMI distribution, obesity level by transport).
- Command-line arguments and a config file for paths.
- Load results into a database (SQLite/PostgreSQL).
- Schedule the pipeline (cron / Airflow) and add data-quality checks.
