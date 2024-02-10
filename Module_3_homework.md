Basicly I uploaded green taxi data manually in the below gcp bucket
- mage-zoomcamp-meto/nyc_greentaxidata_2022/
- then started the homework answers as the following:

-- Creating external table referring to gcs path
CREATE OR REPLACE EXTERNAL TABLE `airy-cortex-297320.ny_taxi.green_tripdata_2022`
OPTIONS (
  format = 'PARQUET',
  uris = ['gs://mage-zoomcamp-meto/nyc_greentaxidata_2022/green_tripdata_2022-*.parquet']
);

-- Create a BQ table from external table
CREATE OR REPLACE TABLE airy-cortex-297320.ny_taxi.green_tripdata_2022_non_partitoned AS
SELECT * FROM airy-cortex-297320.ny_taxi.green_tripdata_2022;

-----------------------------------------------------------------------------------
-- Answer of question 1 in module 3
SELECT count(*) TotalRecords FROM airy-cortex-297320.ny_taxi.green_tripdata_2022;
-----------------------------------------------------------------------------------
-- Answer of question 2 in module 3
SELECT DISTINCT(PULocationID)
FROM airy-cortex-297320.ny_taxi.green_tripdata_2022;

SELECT DISTINCT(PULocationID)
FROM airy-cortex-297320.ny_taxi.green_tripdata_2022_non_partitoned;
-----------------------------------------------------------------------------------
-- Answer of question 3 in module 3
SELECT count(*) TotalRecords 
FROM airy-cortex-297320.ny_taxi.green_tripdata_2022
WHERE fare_amount =0;
-----------------------------------------------------------------------------------
-- Answer of question 4 in module 3
CREATE OR REPLACE TABLE airy-cortex-297320.ny_taxi.green_tripdata_2022_partitoned_clustered
PARTITION BY DATE(lpep_pickup_datetime)
CLUSTER BY PULocationID AS
SELECT * FROM airy-cortex-297320.ny_taxi.green_tripdata_2022;
-----------------------------------------------------------------------------------
-- Answer of question 5 in module 3
SELECT DISTINCT(PULocationID)
FROM airy-cortex-297320.ny_taxi.green_tripdata_2022_non_partitoned
WHERE DATE(lpep_pickup_datetime) BETWEEN '2022-06-01' AND '2022-12-31';

SELECT DISTINCT(PULocationID)
FROM airy-cortex-297320.ny_taxi.green_tripdata_2022_partitoned
WHERE DATE(lpep_pickup_datetime) BETWEEN '2022-06-01' AND '2022-12-31';
-----------------------------------------------------------------------------------