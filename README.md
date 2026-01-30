# Setup 
Done via the gcp UI:
1. Upload the data to a gcs external table month by month
2. create the monthly bigquery tables from the ui
3. create the consolidated table with sql:
   create table hw3.yellow_tripdata_20204 as (
  select * from `hw3.yellow_tripdata_2024-01`
  union all
  select * from `hw3.yellow_tripdata_2024-02`
  union all
  select * from `hw3.yellow_tripdata_2024-03`
  union all
  select * from `hw3.yellow_tripdata_2024-04`
  union all
  select * from `hw3.yellow_tripdata_2024-05`
  union all
  select * from `hw3.yellow_tripdata_2024-06`
);
4. create the external table
create or replace external table `dtc-de-course-484919.hw3.E_yellow_tripdata`
options(
  format = 'PARQUET',
  uris = ['gs://dtc-de-course-484919-hw3/*.parquet']
);
PS: I could have done this a lot easier by creating the external table first and then creating the consolidated table from the external table with SQL
# Q1
select count(*) from `hw3.yellow_tripdata_20204`;
20332093
# Q2
select distinct PULocationID from `hw3.E_yellow_tripdata`;
0mb
select distinct PULocationID from `hw3.yellow_tripdata_20204`;
155.12mb

# Q3
select PULocationID, DOLocationID from `hw3.yellow_tripdata_20204`;
310.24mb

# Q4
select count(*) from `hw3.yellow_tripdata_20204` 
where fare_amount = 0;
8333

# Q5
CREATE TABLE `dtc-de-course-484919.hw3.yellow_tripdata_p`
PARTITION BY DATE(tpep_dropoff_datetime)
CLUSTER BY VendorID
AS
SELECT
  *
FROM `dtc-de-course-484919.hw3.yellow_tripdata_20204`;

# Q6
select distinct VendorID from `hw3.yellow_tripdata_20204`
where tpep_dropoff_datetime between '2024-03-01' and '2024-03-15';
310.31mb

select distinct VendorID from `hw3.yellow_tripdata_p`
where tpep_dropoff_datetime between '2024-03-01' and '2024-03-15';
26.84mb








