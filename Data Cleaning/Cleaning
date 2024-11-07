SELECT * FROM layoffs;

-- Identify Duplicates  
WITH CTE AS(
	SELECT *,
	ROW_NUMBER() OVER(PARTITION BY company,location,industry,total_laid_off,
    percentage_laid_off,date,stage,country,funds_raised_millions) AS row_num
	FROM layoffs)
SELECT *
FROM CTE
WHERE row_num > 1;


-- Removing Duplicates.
-- Removing Duplicates involve sevral setps, because a CTE is not Updateable, Following are the Steps. 

-- 1) Create the Same Table Along with row_num Column.
CREATE TABLE layoffs_2 (
    company VARCHAR(30),
    location VARCHAR(20),
    industry VARCHAR(20),
    total_laid_off VARCHAR(20),
    percentage_laid_off VARCHAR(20),
    date VARCHAR(20),
    stage VARCHAR(20),
    country VARCHAR(20),
    funds_raised_millions VARCHAR(20),
    row_num INT(1)
);

-- 2) Insert All the Data from Old Table to New Tabel, Along with row_num Column.
INSERT INTO layoffs_2
SELECT *,
	ROW_NUMBER() OVER(PARTITION BY company,location,industry,total_laid_off,
    percentage_laid_off,date,stage,country,funds_raised_millions) AS row_num
	FROM layoffs;

-- 3) Now when You have a row_num Column in the new Tabel you can Easily remove the Duplicates. 
DELETE 
FROM layoffs_2
WHERE row_num > 1;

-- Ensure that the Duplicates are Deleted.
SELECT * 
FROM layoffs_2
WHERE row_num > 1;

-- Keeing the Tabel without Duplicates. 
DROP TABLE layoffs;

-- Renam Table layoffs_2 to layoffs.
ALTER TABLE layoffs_2
RENAME layoffs;

-- Now Get ride of row_num Column.
ALTER TABLE layoffs
DROP COLUMN row_num;

-- 	Remove all the white Space.
UPDATE layoffs
SET company = TRIM(company);
UPDATE layoffs
SET location = TRIM(location);

-- industry column also has some null values to deal with.
-- But i have notice that Crypto in indusrty column has different variation so first lest correct that.
UPDATE layoffs
SET industry = 'Crypto'
WHERE industry IN ('CryptoCurrency', 'Crypto Currency');
-- Some of the United Sates has a '.' in the end let remove that.
UPDATE layoffs
SET country = TRIM(TRAILING '.' FROM country);

-- Now let's Standerized the Data column.
UPDATE layoffs
SET `date` = STR_TO_DATE(`date`,'%m/%d/%Y');

-- Mode of the total_laid_off.
SELECT total_laid_off,COUNT(total_laid_off) AS frq 
FROM layoffs
GROUP BY 1
ORDER BY 2 DESC;

-- Meidan of the total_laid_off.
WITH CTE AS(
SELECT 
	total_laid_off,
    CAST(ROW_NUMBER() OVER(ORDER BY company ASC) AS SIGNED) AS row_asc,
    CAST(ROW_NUMBER() OVER(ORDER BY company DESC) AS SIGNED) AS row_des
FROM layoffs)
SELECT total_laid_off, (row_des - row_asc) AS Median
FROM CTE
WHERE (row_des - row_asc) IN (-1,0,1);

-- Replace Null values of Total_Laid_Off with Mode. 
UPDATE layoffs t1
JOIN
	(SELECT total_laid_off,COUNT(total_laid_off) AS frq
		FROM layoffs
		GROUP BY 1
		ORDER BY 2 DESC
		LIMIT 1) t2
	SET t1.total_laid_off = t2.total_laid_off
    WHERE t1.total_laid_off IS NULL;
    
-- Replace Null values of percentage_laid_off with Avrage. 
UPDATE layoffs t1
JOIN 
	(SELECT ROUND(AVG(percentage_laid_off),2) AS percentage_laid_off
		FROM layoffs) t2
SET t1.percentage_laid_off = t2.percentage_laid_off
WHERE t1.percentage_laid_off IS NULL;

-- Replace Null values of industry with the most occuring industry. 
UPDATE layoffs t1
JOIN (
	SELECT industry,COUNT(industry) AS frq
	FROM layoffs
	GROUP BY 1
	ORDER BY 2 DESC
	LIMIT 1) t2
SET t1.industry = t2.industry
WHERE t1.industry IS NULL;

-- Fill the Nulls Values of Stage Column
UPDATE layoffs t1
JOIN (
		SELECT Stage, COUNT(Stage) AS frq
        FROM layoffs
        GROUP BY 1
        ORDER BY 2 DESC
        LIMIT 1) t2
SET t1.Stage = t2.Stage
WHERE t1.Stage IS NULL;


-- So the rest of the null values look in funds_raised_millions look's fine to me so I'm gonna just Delete those.
DELETE 
FROM layoffs
WHERE funds_raised_millions = 0;

-- While I will aslo delete the Null's of industry column. 
DELETE 
FROM layoffs
WHERE industry = '';


-- So Now Let's Work on the Data Type of the Columns. 
-- Let's Check the Data Type of All the Columns
SELECT COLUMN_NAME,DATA_TYPE
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_SCHEMA = 'unclean_data' AND TABLE_NAME = 'layoffs';


-- I'm Gonna assign Data Type to Each column accordingly.
-- Date Column.
ALTER TABLE layoffs
MODIFY COLUMN date DATE;

-- Numarical Columns.
ALTER TABLE layoffs
MODIFY COLUMN funds_raised_millions INT(6);

ALTER TABLE layoffs
MODIFY COLUMN percentage_laid_off INT(4);

ALTER TABLE layoffs
MODIFY COLUMN total_laid_off INT(4);

-- Now let's take a Look Again at the Data Type of the column. 
SELECT COLUMN_NAME,DATA_TYPE
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_SCHEMA = 'unclean_data' AND TABLE_NAME = 'layoffs';


SELECT * FROM layoffs;
