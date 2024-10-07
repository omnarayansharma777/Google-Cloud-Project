We have given 250+ GB covid data .This dataset contains country-level datasets of daily time-series data related to COVID-19 globally.We need to answer some question below .


# Task 1. Total confirmed cases
Build a query that will answer "What was the total count of confirmed cases on April 10, 2020?"   
The query needs to return a single row containing the sum of confirmed cases across all countries. The name of the column should be total_cases_worldwide.  
```
select sum(cumulative_confirmed) as total_cases_worldwide from  bigquery-public-data.covid19_open_data.covid19_open_data 
where date ='2020-04-10' ;
```

# Task 2. Worst affected areas
"How many states in the US had more than 100 deaths on April 10, 2020?"  
```
WITH death AS ( 
SELECT subregion1_name , SUM(cumulative_deceased) as all_death
FROM `bigquery-public-data.covid19_open_data.covid19_open_data`
where country_name='United States of America' AND date='2020-04-10'AND subregion1_name is not null
group by subregion1_name
)
SELECT COUNT(* ) AS count_of_states
FROM death WHERE all_death>100;
```

# Task 3. Identify hotspots
Build a query that will answer "List all the states in the United States of America that had more than 3000 confirmed cases on April 10, 2020?"   
The query needs to return the State Name and the corresponding confirmed cases arranged in descending order.   
Name of the fields to return state and total_confirmed_cases.  
 ```
WITH confirmed AS ( 
SELECT subregion1_name as state, SUM(cumulative_confirmed) as total_confirmed_cases
FROM `bigquery-public-data.covid19_open_data.covid19_open_data`
where country_code='US' AND date='2020-04-10' and subregion1_name is not null
group by subregion1_name
)

SELECT  *
FROM confirmed WHERE total_confirmed_cases>3000
order by total_confirmed_cases desc ;
```

# Task 4. Fatality ratio
1.Build a query that will answer "What was the case-fatality ratio in Italy for the month of April 2020?"
Case-fatality ratio here is defined as (total deaths / total confirmed cases) * 100.  
2. Write a query to return the ratio for the month of April 2020 and contain the following fields in the output: total_confirmed_cases, total_deaths, case_fatality_ratio.  
```
select sum(cumulative_confirmed) as total_confirmed_cases, sum(cumulative_deceased) as total_deaths, (sum(cumulative_deceased)/sum(cumulative_confirmed))*100 as case_fatality_ratio from `bigquery-public-data.covid19_open_data.covid19_open_data`
where country_name='Italy' and date between '2020-04-01' and '2020-04-30'; 
```

# Task 5. Identifying specific day
Build a query that will answer: "On what day did the total number of deaths cross 12000 in Italy?"   
The query should return the date in the format yyyy-mm-dd.  
```
select date from `bigquery-public-data.covid19_open_data.covid19_open_data`
where country_name='Italy' and cumulative_deceased>12000
order by date asc limit 1; 
```

# Task 6. Finding days with zero net new cases
The following query is written to identify the number of days in India between 23, Feb 2020 and 10, March 2020 when there were zero increases in the number of confirmed cases.   
However it is not executing properly.  
```
WITH india_cases_by_date AS (
  SELECT
    date,
    SUM(cumulative_confirmed) AS cases
  FROM
    `bigquery-public-data.covid19_open_data.covid19_open_data`
  WHERE
    country_name="India"
    AND date between '2020-02-23' and '2020-03-10'
  GROUP BY
    date
  ORDER BY
    date ASC
 )

, india_previous_day_comparison AS
(SELECT
  date,
  cases,
  LAG(cases) OVER(ORDER BY date) AS previous_day,
  cases - LAG(cases) OVER(ORDER BY date) AS net_new_cases
FROM india_cases_by_date
)
select count(*) from india_previous_day_comparison
where net_new_cases=0;
```

# Task 7. Doubling rate
Using the previous query as a template, write a query to find out the dates on which the confirmed cases increased by more than 20% compared to the previous day (indicating doubling rate of ~ 7 days) in the US between the dates March 22, 2020 and April 20, 2020.   
The query needs to return the list of dates, the confirmed cases on that day, the confirmed cases the previous day, and the percentage increase in cases between the days.  
Use the following names for the returned fields: Date, Confirmed_Cases_On_Day, Confirmed_Cases_Previous_Day and Percentage_Increase_In_Cases.  
```
WITH us_cases_by_date AS (
    SELECT date, SUM(cumulative_confirmed) AS cases
    FROM `bigquery-public-data.covid19_open_data.covid19_open_data`
    WHERE country_name='United States of America' AND date BETWEEN '2020-03-22' AND '2020-04-20'
    GROUP BY date
    ORDER BY date ASC
), us_previous_day_comparison AS (
    SELECT date, cases, LAG(cases) OVER(ORDER BY date) AS previous_day,
           cases - LAG(cases) OVER(ORDER BY date) AS net_new_cases,
           (cases - LAG(cases) OVER(ORDER BY date))*100/LAG(cases) OVER(ORDER BY date) AS percentage_increase
    FROM us_cases_by_date
)
SELECT Date, cases AS Confirmed_Cases_On_Day, previous_day AS Confirmed_Cases_Previous_Day, percentage_increase AS Percentage_Increase_In_Cases
FROM us_previous_day_comparison
WHERE percentage_increase >20;
```

# Task 8. Recovery rate
Build a query to list the recovery rates of countries arranged in descending order (limit to 20) upto the date May 10, 2020.  
Restrict the query to only those countries having more than 50K confirmed cases.  
The query needs to return the following fields: country, recovered_cases, confirmed_cases, recovery_rate.  
```
with this_table as (
  select country_name as  country,sum(cumulative_recovered) as recovered_cases,sum(cumulative_confirmed) as confirmed_cases 
  from `bigquery-public-data.covid19_open_data.covid19_open_data`
  where date= '2020-05-10'
  group by country_name 

), full_table as( 
  select * , (recovered_cases/confirmed_cases)*100 as recovery_rate
from this_table
)
select * from full_table 
where confirmed_cases>50000 
order  by recovery_rate desc
limit 20;
```

# Task 9. CDGR - Cumulative daily growth rate
The following query is trying to calculate the CDGR on April 10, 2020(Cumulative Daily Growth Rate) for France since the day the first case was reported.The first case was reported on Jan 24, 2020.  
The CDGR is calculated as:  
((last_day_cases/first_day_cases)^1/days_diff)-1)  
Where :  
last_day_cases is the number of confirmed cases on May 10, 2020  
first_day_cases is the number of confirmed cases on Jan 24, 2020  
days_diff is the number of days between Jan 24 - May 10, 2020  
The query isn’t executing properly. Can you fix the error to make the query execute successfully?  
```
WITH
  france_cases AS (
  SELECT
    date,
    SUM(cumulative_confirmed) AS total_cases
  FROM
    `bigquery-public-data.covid19_open_data.covid19_open_data`
  WHERE
    country_name="France"
    AND date IN ('2020-01-24',
      '2020-06-10')
  GROUP BY
    date
  ORDER BY
    date)
, summary as (
SELECT
  total_cases AS first_day_cases,
  LEAD(total_cases) OVER(ORDER BY date) AS last_day_cases,
  DATE_DIFF(LEAD(date) OVER(ORDER BY date),date, day) AS days_diff
FROM
  france_cases
LIMIT 1
)
select first_day_cases, last_day_cases, days_diff, POWER((last_day_cases/first_day_cases),(1/days_diff))-1 as cdgr
from summary
```

# Task 10. Create a Looker Studio report
Create a Looker Studio report that plots the following for the United States:  
Number of Confirmed Cases  
Number of Deaths  
Date range : 2020-03-30 to 2020-04-19  

```
select date,sum(cumulative_confirmed) as country_cases,sum(cumulative_deceased) as country_deaths
from `bigquery-public-data.covid19_open_data.covid19_open_data`
where country_code='US' and date between '2020-03-30' and '2020-04-19'
group by date
ORDER BY date;
```
