# Overview

I have chosen to undertake this project as my first end-to-end project, aiming to leverage my extensive experience in credit risk analysis. The main goal of the dashboard is to help analyze and identify different factors that influence loan default rates. The project covers the entire process—from data exploration and cleaning to manipulation and preparation—using tools such as Excel, SQL, and Power BI. The dataset, sourced from Kaggle, includes customer profiles, as well as historical and current loan statuses, along with loan details such as amounts, terms, and interest rates.

# Table of Contents

- [Objective](#objective)
- [Data Source](#data-source)
- [Stages](#stages)
- [Development](#development)
  - [Process](#process)
  - [Data Exploration](#data-exploration)
  - [Data Cleaning](#data-cleaning)

# Objective

Provide a clear visualization of key performance metrics, identify emerging risk trends, and recommend data-driven decision-making to optimize loan portfolio management and mitigate potential risks.

# Data Source

The [data](https://www.kaggle.com/datasets/prakashraushan/loan-dataset/data) is sourced from Kaggle (an Excel extract). The dataset consists of the following columns:

| Column_Name | Description |
| --- | --- |
| customer_id | Unique identifier for each customer |
| customer_age | Age of the customer |
| customer_income | Annual income of the custome |
| home_ownership | Home ownership status (e.g., RENT, OWN, MORTGAGE) |
| employment_duration | Duration of employment in months | 
| loan_intent | Purpose of the loan (e.g., PERSONAL, EDUCATION, MEDICAL, VENTURE) |
| loan_grade | Grade assigned to the loan |
| loan_amnt | Loan amount requested |
| loan_int_rate | Interest rate of the loan |
| term_years | Loan term in years |
| historical_default | Indicates if the customer has a history of default (Y/N) |
| cred_hist_length | Length of the customer's credit history in years | 
| Current_loan_status | Current status of the loan (DEFAULT, NO DEFAULT) |

# Stages

- Development
- Testing
- Analysis

# Development

## Process

What was the step-by-step approach to executing this project from start to finish?

1. Get the data
2. Explore the data in Excel
3. Load the data into SQL Server
4. Clean and manipulate the data in SQL
5. Test the data with SQL
6. Load and visualize the data in Power BI
7. Generate the findings based on the insights
8. Write the documentation + commentary
9. Publish the data to GitHub Pages

## Data Exploration

This was the stage where data was scanned for any errors, inconsistencies, blanks, duplicates, and unusual characters. Below were the initial observations on the dataset:

  1.All columns are relevant to the analysis.
  2. Five columns contained missing values: employment_duration, loan_amnt, loan_int_rate, historical_default, and current_loan_status.
  3. The employment_duration and customer_age columns included extreme values that required confirmation or validation.
  4. The loan_amnt column contained an unusual character preceding the British Pound symbol.

## Data Cleaning

The foilowing were the criteria for a clean data:

  - Column Relevance: Retain only the columns relevant to the analysis or application.
  - Data Types: Ensure data types align appropriately with the contents of each column.
  - Duplicate Removal: Eliminate duplicate rows to maintain data accuracy and integrity.
  - Completeness: Ensure no null values exist in any column, except where their presence is valid and contextually appropriate.
  - Validation of Extreme Values: Identify, validate, and address extreme values as needed, replacing them when necessary.

Below is the tabular representation of the expected schema for the clean data:

| Column_Name | Data Type | Nullable
| --- | --- | --- |
| customer_id | smallint | NO
| customer_age | tinyint | NO
| customer_income | money | YES
| home_ownership | nvarchar | NO
| employment_duration | tinyint | YES
| loan_intent | nvarchar | NO
| loan_grade | nvarchar | NO
| loan_amnt | money | YES
| loan_int_rate | float | YES
| term_years | tinyint | NO
| historical_default | bit | YES
| cred_hist_length | tinyint | NO
| Current_loan_status | nvarchar | YES




  
