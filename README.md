# SQL Data Cleaning Project: Tech Layoffs Dataset

## 📋 Project Overview
This project focuses on cleaning a messy dataset containing information about tech company layoffs. The goal of this project is to transform raw, inconsistent data into a structured, usable format that is ready for Exploratory Data Analysis (EDA) and visualization. 

All data cleaning processes were performed using **SQL** (MySQL), demonstrating best practices in handling real-world data issues.

## 🛠️ Tools & Techniques Used
- **SQL Dialect:** MySQL
- **Key SQL Skills Applied:** 
  - Common Table Expressions (CTEs)
  - Window Functions (`ROW_NUMBER()`, `PARTITION BY`)
  - Self Joins
  - String Manipulation (`TRIM`, `TRAILING`, `LIKE`)
  - Date Formatting (`STR_TO_DATE`)
  - Data Definition Language (DDL) & Data Manipulation Language (DML)

## 🧹 The Data Cleaning Process

The data cleaning phase was broken down into a systematic 4-step process. Before modifying the data, a **staging table** (`layoffs_staging`) was created to ensure the original raw data was preserved.

### 1. Removing Duplicates
- **Challenge:** The dataset contained multiple identical rows that could skew future analysis.
- **Solution:** Utilized a CTE and the `ROW_NUMBER()` window function partitioned by key columns (company, location, industry, total_laid_off, percentage_laid_off, date, stage, country, funds_raised_millions) to identify duplicates. 
- Created a secondary staging table (`layoffs_staging2`) to safely delete rows where the row number was greater than 1.

### 2. Standardizing the Data
- **Whitespace Removal:** Used `TRIM()` to remove accidental spaces from company names.
- **Industry Categorization:** Standardized naming conventions (e.g., grouped all variations of 'Crypto Currency' and 'Crypt%' into a single 'Crypto' category).
- **Geographical Data:** Cleaned up trailing punctuation in the `country` column (e.g., changing 'United States.' to 'United States').
- **Date Formatting:** Converted the `date` column from a text string format (`%m/%d/%Y`) into a proper SQL `DATE` data type to allow for future time-series analysis.

### 3. Handling Null & Missing Values
- **Challenge:** Several rows had missing `industry` values even though the company existed in other rows with the industry filled in.
- **Solution:** Performed a **Self Join** on the table to match companies with identical names and populated the missing `industry` fields using data from the populated rows.

### 4. Removing Unnecessary Data
- Deleted records where both `total_laid_off` and `percentage_laid_off` were `NULL`, as these rows provide no meaningful value for analyzing layoff impacts.
- Dropped the temporary `row_num` column created during the deduplication phase to keep the final dataset clean and optimized.

## 💡 Conclusion
By following these steps, the raw dataset was successfully cleansed of duplicates, standardized for consistency, populated with missing critical values, and stripped of useless rows. The final `layoffs_staging2` table is now highly reliable and ready for comprehensive data analysis.