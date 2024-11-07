# Data Cleaning In MySQL

Situation 🔍
   - The dataset provided contains information about layoffs across various companies. 
     It included multiple columns such as company names, locations, industries, total layoffs, percentage laid off, and other relevant details. 
     However, the data was messy with issues like duplicates, null values, inconsistent formats, and incorrect data types. This made the data unreliable for any meaningful analysis.

Task 🎯
   - The task was to clean this dataset so that it could be used for further analysis. 

The goals were:
   - Remove duplicates to avoid redundant data.
   - Handle null values by replacing them with meaningful values (e.g., mode, average).
   - Standardize columns to ensure consistent formats across the dataset.
   - Correct data types for columns to ensure they match the values they contain.

Action ⚙️
I took the following steps to clean the dataset:

   - Identifying Duplicates:
          I began by identifying duplicate records based on multiple attributes like company name, location, industry, total layoffs, etc.
          This helped pinpoint exact rows that were repeated.
          
  - Removing Duplicates:
          I created a new table with an additional column (row_num) that uniquely identified each record. Using this column, I deleted all rows where row_num was greater than 1,
          ensuring that only one instance of each record remained.
    
  - Trimming Extra Spaces:
          I ensured that the company and location columns had no leading or trailing spaces, which could cause issues during analysis or visualization.
    
  - Standardizing Column Values: 
          The industry column had multiple variations of terms, such as "CryptoCurrency" and "Crypto Currency." I unified these into a single term ("Crypto").
          For some columns like country, I removed unnecessary punctuation (like trailing periods) to standardize country names.
    
  - Handling Null Values:
        total_laid_off: I replaced missing values with the mode of the column, i.e., the most frequent value.
        percentage_laid_off: Missing values were replaced with the average percentage of layoffs.
        industry and stage: These columns had null values, which I filled with the most frequent non-null value for each.
    
  - Standardizing Dates:
        The date column contained dates in different formats. I standardized these into a consistent YYYY-MM-DD format for easier analysis.
    
  - Dealing with Outliers:
    I identified that the funds_raised_millions column had some outliers with values like 0. I deleted these rows as they were irrelevant for the analysis.
    
  - Correcting Data Types:
        After cleaning the data, I ensured that each column had the correct data type.
        For example, I converted the date column to a DATE data type, and columns like funds_raised_millions,
        percentage_laid_off, and total_laid_off were changed to INT to facilitate numerical analysis.
    
Result 🎉
After cleaning the dataset, it was fully standardized, consistent, and ready for analysis.

  - All duplicates were removed, ensuring that each record is unique.
  -  Missing values were appropriately handled with modes, averages, or frequent non-null values.
  -  The columns were standardized, and irrelevant data (such as 0 values in the funds_raised_millions column) was deleted.
  -  The correct data types were applied, making the dataset suitable for further analysis or reporting.
  -  The dataset is now clean, reliable, and ready for use in any analytical processes!
