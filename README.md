# Analyzing-NYC-Public-School-Test-Result-Scores

## Project Description

Every year, school test results play a role in deciding the fate of millions of students. In America, the SAT is a major part of the college admissions process.

In this project, you will work with a SQL database containing test performance from NYC's public schools.

You will look at how performance varies by borough, identify how many schools fail to report information, and find the top ten performing schools across the city!

## Table of Contents

1. _schools_modified.csv dataset file contains information about the data._
2. _notebook.ipynb is a jupyter notebook which has been used for quering the data (using postgresql)

## Project Tasks:

1) _Inspecting the data_

```sql
%%sql
postgresql:///schools
    
-- Select all columns from the database
SELECT *
FROM schools
-- Display only the first ten rows
LIMIT 10;
```
2) _Finding missing values_
```sql
%%sql

-- Count rows with percent_tested missing and total number of schools
WITH percent_tested_missing
AS
(SELECT COUNT(*)-COUNT(percent_tested) AS num_tested_missing, 
 COUNT(*) AS num_schools
 FROM schools)

SELECT *
FROM percent_tested_missing;
```
3) _Schools by building code_
```sql
%%sql

-- Count the number of unique building_code values
SELECT COUNT(DISTINCT building_code) AS num_school_buildings
FROM schools;
```
4) _Best schools for math_
```sql
%%sql

-- Select school and average_math

SELECT school_name, average_math
FROM schools

-- Filter for average_math 640 or higher

WHERE average_math >= 640

-- Display from largest to smallest average_math

ORDER BY average_math DESC;
```
5) _Lowest reading score_
```sql
%%sql

-- Find lowest average_reading

SELECT MIN(average_reading) AS lowest_reading
FROM schools;
```
6) _Best writing school_
```sql
%%sql

-- Find the top score for average_writing

SELECT school_name, MAX(average_writing) AS max_writing
FROM schools

-- Group the results by school
GROUP BY 1
-- Sort by max_writing in descending order
ORDER BY 2 DESC
-- Reduce output to one school
LIMIT 1;
```
7) _Top 10 schools_
```sql
%%sql

-- Calculate average_sat
SELECT school_name, SUM(average_math+average_reading+average_writing) AS average_Sat
FROM schools
-- Group by school_name
GROUP BY 1
-- Sort by average_sat in descending order
ORDER BY 2 DESC
-- Display the top ten results
LIMIT 10;
```
8) _Ranking boroughs_
```sql
%%sql

-- Select borough and a count of all schools, aliased as num_schools
SELECT borough, COUNT(*) AS num_schools,
SUM(average_math+average_reading+average_writing)/COUNT(*) AS average_borough_sat
FROM schools
-- Organize results by borough
GROUP BY 1
-- Display by average_borough_sat in descending order
ORDER BY 3 DESC;
```
9) _Brooklyn numbers_
```sql
%%sql

-- Select school and average_math
SELECT school_name, average_math
FROM schools
-- Filter for schools in Brooklyn
WHERE borough = 'Brooklyn'
-- Aggregate on school_name
GROUP BY 1
-- Display results from highest average_math and restrict output to five rows
ORDER BY 2 DESC
LIMIT 5;
```
