# Setup 
Done via the gcp UI
# Q1
select count(*) from `hw3.yellow_tripdata_20204`;
20332093
# Q2
select distinct PULocationID from `hw3.yellow_tripdata_20204`;
155.12mb

# Q3
select PULocationID, DOLocationID from `hw3.yellow_tripdata_20204`;
310.24mb

# Q4
select count(*) from `hw3.yellow_tripdata_20204` 
where fare_amount = 0;
8333





