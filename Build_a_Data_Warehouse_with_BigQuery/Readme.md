# Challenge scenario
As a junior member of the Data Science team you've been assigned to use your data warehousing skills 
to develop a table containing the features for the machine learning model.
### Key Learnings: 
In This challenge I have learned **Creating New dataset and tables** , **Creating new column ,Struct and Updateing 
column values from other tables** , **Removing some values** , Doing **Partition** all of this I did in Bigquery Data warehouse.

U can find [ Lab Link](https://www.cloudskillsboost.google/course_templates/624/labs/489694) and my skill badge [Here](https://www.credly.com/badges/9b109e3a-b93a-4749-9d11-db393929ee4b/public_url)

## Task 1 : Create a table partitioned by date
The starting point for the machine learning model will be the oxford_policy_tracker table in the COVID 19 
Government Response public dataset which contains details of different actions taken by governments to curb
the spread of Covid-19 in their jurisdictions.

Given the fact that there will be models based on a range of time periods, you have to create a dataset and 
then create a date partitioned version of the oxford_policy_tracker table in your newly created dataset, 
with an expiry time set to 1445 days.

While creating a table, you have also been instructed to exclude the United Kingdom ( alpha_3_code=GBR), 
Brazil ( alpha_3_code=BRA), Canada ( alpha_3_code=CAN) & the United States of America (alpha_3_code=USA) 
as these will be subject to more in-depth analysis through nation and state specific analysis.

```
CREATE OR REPLACE TABLE covid.oxford_policy_tracker
 PARTITION BY date
 OPTIONS (
   partition_expiration_days=1445,
   description="partitioned by day"
 ) as 
( SELECT * FROM `bigquery-public-data.covid19_govt_response.oxford_policy_tracker` 
where alpha_3_code!='GBR' and alpha_3_code!='CAN' and alpha_3_code!='BRA'and  alpha_3_code!='USA')
```


## Task 2 : Add new columns to global_mobility_tracker_data table
In this step you need to add columns for population, country_area and a record column (named mobility) 
that will take six input fields representing average mobility data from the last six columns of the 
mobility_report table from the Google COVID 19 Mobility public dataset.

It is important to follow the column names and data types that have been specified when updating the 
schema for the table provided in the task description.

The dataset named 'covid_data' contains a table named global_mobility_tracker_data in which update the 
table to add new columns with the appropriate data types to ensure alignment with the specification 
provided to you:

```
ALTER TABLE `qwiklabs-gcp-01-f16071b9de6d.covid_data.global_mobility_tracker_data`
ADD COLUMN population INTEGER,
ADD COLUMN country_area FLOAT64,
ADD COLUMN mobility STRUCT<avg_retail FLOAT64, avg_grocery FLOAT64, avg_parks FLOAT64, avg_transit FLOAT64 ,
avg_workplace FLOAT64, avg_residential FLOAT64>;
```

### Task 3 : Add country population data to the population column
A colleague working on an ancillary task has provided you with the SQL they used for updating the daily new case
data in a similar data partitioned table through a JOIN with the covid_19_geographic_distribution_worldwide table 
from the European Center for Disease Control COVID 19 public dataset.

This is a useful table that contains a range of data, including recent national population data, which you need to 
populate the population column in the table provided in task description:

```
update `qwiklabs-gcp-01-f16071b9de6d.covid_data.consolidate_covid_tracker_data` t0
set t0.population=t2.pop_data_2019
from (
SELECT DISTINCT country_territory_code, pop_data_2019 FROM `bigquery-public-data.covid19_ecdc.covid_19_geographic_distribution_worldwide`) AS t2
WHERE t0.alpha_3_code = t2.country_territory_code;
```
## Task 4 : Add country area data to the country_area column
In the step, you need to add in country area data to table provided in the task description. 
The data for geographic country areas can be found in the country_names_area table from the Census Bureau International public dataset.

Within the BigQuery dataset named 'covid_data', add the country area data to the country_area column in 
'consolidate_covid_tracker_data' table with country_names_area table data from the Census Bureau International public dataset.
```
update `qwiklabs-gcp-01-f16071b9de6d.covid_data.consolidate_covid_tracker_data` t0
set t0.country_area=t2.country_area
from (
SELECT DISTINCT country_name, country_area FROM `bigquery-public-data.census_bureau_international.country_names_area`) AS t2
WHERE t0.country_name = t2.country_name;
```
