## BigQuery

```sql
SELECT * FROM astral-outpost-413713.ny_taxi.yellow_cab_data LIMIT 5;
```

```sql
CREATE OR REPLACE EXTERNAL TABLE `astral-outpost-413713.ny_taxi.titanic_clean`
OPTIONS (
  format = 'CSV',
  uris = ['gs://mage-zoomcamp-pinku/titanic_clean.csv']
);
```

```sql
CREATE OR REPLACE EXTERNAL TABLE `astral-outpost-413713.ny_taxi.ny_taxi_yellow_data`
OPTIONS (
  format = 'CSV',
  uris = [
    'gs://mage-zoomcamp-pinku/nyc_taxi_data/tpep_pickup_date=2019-03-17/*.parquet', 
    'gs://mage-zoomcamp-pinku/nyc_taxi_data/tpep_pickup_date=2019-03-19/*.parquet'
  ]
);
```


```sql
CREATE OR REPLACE EXTERNAL TABLE `astral-outpost-413713.ny_taxi.ny_taxi_yellow_data_full`
OPTIONS (
  format = 'CSV',
  uris = [
    'gs://mage-zoomcamp-pinku/nyc_taxi_data.parquet'
  ]
);
```


```sql
CREATE OR REPLACE TABLE `astral-outpost-413713.ny_taxi.titanic_non_partitioned` AS
SELECT * FROM `ny_taxi.titanic_clean`;
```

```sql
CREATE OR REPLACE TABLE `astral-outpost-413713.ny_taxi.titanic_partitioned`
PARTITION BY 
  DATE(Pclass) AS
SELECT * FROM `ny_taxi.titanic_clean`;
```

```sql
SELECT DISTINCT(VendorID)
FROM `astral-outpost-413713.ny_taxi.yellow_tripdata_partitioned`
WHERE DATE(tpep_pickup_datetime) BETWEEN '2019-06-01' AND '2019-06-30'; 
```

```sql
SELECT table_name, partition_id, total_rows
FROM `astral-outpost-413713.ny_taxi.INFORMATION.PARTITIONS`
WHERE table_name = 'yellow_tripdata_partitioned'
ORDER BY total_rows DESC;
```

```sql
CREATE OR REPLACE TABLE `astral-outpost-413713.ny_taxi.yellow_tripdata_partitioned_clustered`
PARTITION BY DATE(tpep_pickup_datetime)
CLUSTER BY VendorID AS
SELECT * FROM `astral-outpost-413713.ny_taxi.external_yellow_tripdata`;
```
```sql
SELECT count(*) as trips
FROM `astral-outpost-413713.ny_taxi.yellow_tripdata_partitioned`
WHERE DATE(tpep_pickup_datetime) BETWEEN '2019-06-01' AND '2019-06-30'
AND VendorID=1; 
```
```sql

```
```sql

```