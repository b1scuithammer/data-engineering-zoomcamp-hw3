# Setup 
Done via the gcp UI:  <br />
1. Upload the data to a gcs external table month by month  <br />
2. create the monthly bigquery tables from the ui <br /> 
3. create the consolidated table with sql:  <br />
   create table hw3.yellow_tripdata_20204 as (  <br />
  select * from `hw3.yellow_tripdata_2024-01`  <br />
  union all  <br />
  select * from `hw3.yellow_tripdata_2024-02`  <br />
  union all  <br />
  select * from `hw3.yellow_tripdata_2024-03`  <br />
  union all  <br />
  select * from `hw3.yellow_tripdata_2024-04`  <br />
  union all  <br />
  select * from `hw3.yellow_tripdata_2024-05`  <br />
  union all  <br />
  select * from `hw3.yellow_tripdata_2024-06`  <br />
);  
4. create the external table: <br />
create or replace external table `dtc-de-course-484919.hw3.E_yellow_tripdata`<br />
options(<br />
  format = 'PARQUET',<br />
  uris = ['gs://dtc-de-course-484919-hw3/*.parquet']<br />
);<br />
PS: I could have done this a lot easier by creating the external table first and then creating the consolidated table from the external table with SQL
# Q1
select count(*) from `hw3.yellow_tripdata_20204`;<br />
20332093
# Q2
select distinct PULocationID from `hw3.E_yellow_tripdata`;<br />
0mb
select distinct PULocationID from `hw3.yellow_tripdata_20204`;<br />
155.12mb

# Q3
select PULocationID, DOLocationID from `hw3.yellow_tripdata_20204`;<br />
310.24mb

# Q4
select count(*) from `hw3.yellow_tripdata_20204` <br />
where fare_amount = 0;<br />
8333

# Q5
CREATE TABLE `dtc-de-course-484919.hw3.yellow_tripdata_p`<br />
PARTITION BY DATE(tpep_dropoff_datetime)<br />
CLUSTER BY VendorID<br />
AS<br />
SELECT<br />
  *<br />
FROM `dtc-de-course-484919.hw3.yellow_tripdata_20204`;

# Q6
select distinct VendorID from `hw3.yellow_tripdata_20204`<br />
where tpep_dropoff_datetime between '2024-03-01' and '2024-03-15';<br />
310.31mb<br />

select distinct VendorID from `hw3.yellow_tripdata_p`<br />
where tpep_dropoff_datetime between '2024-03-01' and '2024-03-15';<br />
26.84mb<br />








