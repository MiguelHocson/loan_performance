# Overview

I have chosen to undertake this project as my first end-to-end project, aiming to leverage my extensive experience in credit risk analysis. The main goal of the dashboard is to help analyze and identify different factors that influence loan default rates. The project covers the entire process—from data exploration and cleaning to manipulation and preparation—using tools such as Excel, SQL, and Power BI. The dataset, sourced from Kaggle, includes customer profiles, as well as historical and current loan statuses, along with loan details such as amounts, terms, and interest rates.

# Table of Contents

- [Objective](#objective)
- [Data Source](#data-source)
- [Stages](#stages)
- [Development](#development)
  - [Process](#process)
  - [Data Exploration](#data-exploration)

# Objective

Provide a clear visualization of key performance metrics, identify emerging risk trends, and recommend data-driven decision-making to optimize loan portfolio management and mitigate potential risks.

# Data Source

The data is sourced from Kaggle (an Excel extract), [see here to find it.](https://www.kaggle.com/datasets/prakashraushan/loan-dataset/data)

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

This was the stage where data was scanned for any errors, inconsistencies, blanks, duplicates, and unusual characters. 

- Initial observations with the dataset:

1. All columns are relevant for the analysis.
2. There were 5 columns which contained blanks: employment_duration, loan_amnt, loan_int_rate, historical_default, current_loan_status. 
3. Employment_duration column contained extreme values which needed to be confirmed or validated. 
4. Loan_amnt column contained unusual character which can be seen before the British Pound symbol.
  
