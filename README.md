# 03-data-warehouse
Data Engineering Zoomcamp week 3 - Data Warehouse
Question 1: 

    CREATE OR REPLACE EXTERNAL TABLE `zoomcamp.yellow_tripdata_0104`
    OPTIONS (
      format = 'PARQUET', 
      uris = ['gs://dezoomcamp_hw3_2025_aly/*.parquet']
    );

Question 2 :
        SELECT count(*) FROM `kestra-soundbox.zoomcamp.yellow_tripdata_0104` ;
        
        CREATE OR REPLACE TABLE `zoomcamp.yellow_tripdata_0104_materialized`
        AS
        SELECT * FROM `zoomcamp.yellow_tripdata_0104`;
        
        select count (*) from zoomcamp.yellow_tripdata_0104_materialized;

Question 4: 

        SELECT COUNT(fare_amount) 
        FROM `zoomcamp.yellow_tripdata_0104`
        where fare_amount = 0;

Question 5: 
--A: 2.72 GB
--B:none
--C: PARTITION BY expression must be DATE(<timestamp_column>), DATE(<datetime_column>), DATETIME_TRUNC(<datetime_column>, --DAY/HOUR/MONTH/YEAR), a DATE column, TIMESTAMP_TRUNC(<timestamp_column>, DAY/HOUR/MONTH/YEAR), DATE_TRUNC----(<date_column>, MONTH/YEAR), or RANGE_BUCKET(<int64_column>, GENERATE_ARRAY(<int64_value>, <int64_value>[, <int64_value>]))
--D:none 

        create or replace table `kestra-soundbox.zoomcamp.yellow_tripdata_partitioned_clustered`
        partition by date(tpep_dropoff_datetime)
        cluster by VendorID AS
        SELECT * FROM zoomcamp.yellow_tripdata_0104;

Question 6: 
--btyes processed : 310.24MB

      select distinct VendorID
      from zoomcamp.yellow_tripdata_0104
      where DATE(tpep_dropoff_datetime) BETWEEN '2024-03-01' AND '2024-03-31'
;
--bytes process 54.68MB

        select distinct VendorID
        from kestra-soundbox.zoomcamp.yellow_tripdata_partitioned_clustered
        where DATE(tpep_dropoff_datetime) BETWEEN '2024-03-01' AND '2024-03-31';

