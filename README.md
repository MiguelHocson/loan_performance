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

  1. All columns are relevant to the analysis.
  2. Five columns contained missing values: employment_duration, loan_amnt, loan_int_rate, historical_default, and current_loan_status.
  3. The employment_duration and customer_age columns included extreme values that required confirmation or validation.
  4. The loan_amnt column contained an unusual character preceding the British Pound symbol.
  5. Six duplicate rows were found and removed.

![Duplicates_removed](assets/images/Duplicates_removed.png)



## Data Cleaning, Transforming & Testing

The foilowing were the criteria for a clean data:

  - Column Relevance: Retain only the columns relevant to the analysis or application.
  - Duplicate Checking: Ensure there are no duplicate rows to maintain data accuracy and integrity.
  - Completeness: Ensure no null values exist in any column, except where their presence is valid and contextually appropriate.
  - Validation of Extreme Values: Identify, validate, and address extreme values as needed, replacing them when necessary.
  - Data Types: Ensure data types align appropriately with the contents of each column.

Below were the steps taken to clean, transform and test the data:

1. Load the dataset in SQL
2. Examine dataset
   
       ```sql
   
       SELECT *
       FROM loan_dataset
   
       ```



3. Double-check for duplicates

       ```sql
   
       /*
       double-checking duplicates
       */
   
       SELECT
       	customer_id,
       	COUNT(*)
       FROM loan_dataset
       GROUP BY 
	       customer_id
       HAVING
      	 COUNT(*) > 1;
       ```

	![Duplicates_doublechecking](assets/images/Duplicate_doublechecking.png)




4. Identify Extreme Values/Outliers and replace as necessary


 - customer_age

       ```sql
  
       /*
       identifying and correcting extreme values/outliers under customer_age
       */
      
       SELECT customer_age
       FROM loan_dataset
       ORDER BY customer_age ASC;     -- checking for the extreme lows, query returned 3,6,8
      
       SELECT customer_age
       FROM loan_dataset
       ORDER BY customer_age DESC;    -- checking for the extreme highs, query returned 123 & 144
      
       SELECT
      	  AVG(customer_age) AS avg_age     -- getting average customer_age to replace extreme values
       FROM loan_dataset;
      
       UPDATE loan_dataset
       SET customer_age = 27                      -- updating extreme rows in customer_age column with computed average value
       WHERE customer_age IN (3,6,8,123,144);
      
       ```

	![Duplicates_doublechecking](assets/images/extreme_custage.png)




 - employment_duration

       ```sql
  
       /*
       identifying and correcting extreme values/outliers under employment_duration
       */

      
  
       SELECT employment_duration
       FROM loan_dataset
       ORDER BY employment_duration DESC;     -- checking for the extreme highs, query returned 123 months 

       SELECT
      	  AVG(employment_duration) AS avg_employment_duration       -- getting average employment_duration to replace extreme values
       FROM loan_dataset;

       UPDATE loan_dataset
       SET employment_duration = 4         	-- updating extreme rows in customer_age column with computed average value (average value is comparable to customers w/in same age bracket)
       WHERE employment_duration = 123;
      
       ```

	 ![Duplicates_doublechecking](assets/images/extreme_empduration.png)




5. Check for NULL values

        ```sql
   
        /*
        Checking for null values
        */ 
        
        SELECT *
        FROM loan_dataset
        WHERE
           customer_id IS NULL OR customer_age IS NULL OR customer_income IS NULL OR
           home_ownership IS NULL OR employment_duration IS NULL OR loan_intent IS NULL OR
           loan_grade IS NULL OR loan_amnt IS NULL OR loan_int_rate IS NULL OR
           term_years IS NULL OR historical_default IS NULL OR cred_hist_length IS NULL OR
           Current_loan_status IS NULL;
      
        ```



 - customer_id

       ```sql
  
       /*
       identifying NULL customer_id and removing rows with null values as it is assumed to be missing data
       */

       SELECT *
       FROM loan_dataset
       WHERE customer_id IS NULL;     -- checking for NULL values under customer_id, query returned 3 rows with NULL values

       DELETE FROM loan_dataset       -- removed from dataset as considered missing data
       WHERE customer_id IS NULL;

       ```




 - loan_int_rate

       ```sql
  
       /*
       identifying NULL interest rates and updating with the average int rate, assuming all loans are interest-bearing
       */

       SELECT
      	  loan_intent,
          loan_int_rate
       FROM
          loan_dataset
       WHERE
          loan_int_rate IS NULL;     -- checking for NULL values under loan_int_rate, query returned 3000 rows

       SELECT
          AVG(loan_int_rate) AS avg_int_rate_per_intent      -- getting average int_rate to replace NULL values, query returned avg_int_rate of 11.01%
       FROM
          loan_dataset;

       SELECT
          loan_intent,
          AVG(loan_int_rate) AS avg_int_rate_per_intent      -- checking average int_rate per loan type, query returned avg_int_rate of around 11% per loan type
       FROM
          loan_dataset
       GROUP BY
          loan_intent;

       UPDATE loan_dataset
       SET loan_int_rate = 11.01         -- Updating NULL values with computed average int_rate, assuming all loans are interest-bearing
       WHERE loan_int_rate IS NULL
       
       ```

	 ![Duplicates_doublechecking](assets/images/average_intrate.png)


  

 - current_loan_status

       ```sql
       /*
       identifying NULL values under current_loan_status and updating based on the historical_default
       */

      
       SELECT
          current_loan_status,
          historical_default           -- checking NULL values under current_loan_status, query returned 4 rows in which all have NO historical default
       FROM 
          loan_dataset
       WHERE
          current_loan_status IS NULL;

       UPDATE loan_dataset
       SET Current_loan_status = 'NO DEFAULT'    -- updating NULL values as "No Default" since all rows have no historical default
       WHERE Current_loan_status IS NULL;

       ```

	 ![Duplicates_doublechecking](assets/images/currentloanstatus_defaultrows.png)




 - historical_default

       ```sql
       /*
       identifying NULL values under historical_default and updating based on the historical_default distrubtion per loan_grade
       */

      
       SELECT
          loan_grade,                  -- getting count of historical default status per loan grade, query returned "A" loan_grade had less histo-
          historical_default,          -- rical default (N-1954, Y-292) while loans with "B to E" loan_grade had a higher number of historical defaults.
          COUNT(*)
       FROM 
          loan_dataset
       GROUP BY
          loan_grade, historical_default
       ORDER BY
          loan_grade, historical_default;

       UPDATE loan_dataset
       SET historical_default = 0                                    -- updating historical_default to 0 for A-grade loans
       WHERE loan_grade = 'A' AND historical_default IS NULL;

       UPDATE loan_dataset
       SET historical_default = 1                                                   -- updating historical_default to 1 for B to E-grade loans
       WHERE loan_grade IN ('B','C','D','E') AND historical_default IS NULL;

       ```


	 ![Duplicates_doublechecking](assets/images/historicaldefault_pergrade.png)



6. Check if the data_type and IS_NULLABLE per column are correct

        ```sql
        /*
        double checking column data types and IS_NULLABLE
        */

        SELECT
           column_name,
           data_type,
           IS_NULLABLE
        FROM
           INFORMATION_SCHEMA.COLUMNS;


	 ![Duplicates_doublechecking](assets/images/information_schema.png)






  
