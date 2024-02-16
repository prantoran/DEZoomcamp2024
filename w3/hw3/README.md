
## Create an external table using the Green Taxi Trip Records Data for 2022. 

```sql
CREATE OR REPLACE EXTERNAL TABLE astral-outpost-413713.ny_taxi.green_tripdata_2022
OPTIONS (
  format = 'PARQUET',
  uris = ['gs://mage-zoomcamp-pinku/nyc_taxi_2022_green.parquet']
);
```


## Question 1
```sql
SELECT COUNT(*)
FROM astral-outpost-413713.ny_taxi.green_tripdata_2022_nonpartitioned;
```

## Question 2
```sql
SELECT DISTINCT(PULocationID) 
FROM astral-outpost-413713.ny_taxi.green_tripdata_2022_nonpartitioned;

SELECT DISTINCT(PULocationID) 
FROM astral-outpost-413713.ny_taxi.green_tripdata_2022;
```

## Question 3
```sql
SELECT COUNT(*)
FROM astral-outpost-413713.ny_taxi.green_tripdata_2022_nonpartitioned
WHERE fare_amount = 0;
```

## Question 4 and 5

```sql
CREATE OR REPLACE TABLE astral-outpost-413713.ny_taxi.green_tripdata_2022_partitioned
PARTITION BY DATETIME(lpep_pickup_datetime)
CLUSTER BY PUlocationID AS (
  SELECT *, CAST(lpep_pickup_datetime as DATE) FROM astral-outpost-413713.ny_taxi.green_tripdata_2022_nonpartitioned
);
```

```sql
SELECT DISTINCT PULocationID 
FROM astral-outpost-413713.ny_taxi.green_tripdata_2022_partitioned
WHERE lpep_pickup_datetime BETWEEN '06-01-2022' AND '06-30-2022';
```

```sql
SELECT DISTINCT PULocationID 
FROM astral-outpost-413713.ny_taxi.green_tripdata_2022_nonpartitioned
WHERE lpep_pickup_datetime BETWEEN '06-01-2022' AND '06-30-2022';
```