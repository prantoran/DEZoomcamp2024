https://github.com/DataTalksClub/data-engineering-zoomcamp/blob/main/cohorts/2024/01-docker-terraform/homework.md

## Q1

```bash
docker --help
docker build --help
docker run --help | grep "Automatically remove the container when it exits"
```

## Q2

```bash
docker run -it --entrypoint=bash python:3.9
pip list | grep "wheel"
```

## Q3

```bash
wget https://github.com/DataTalksClub/nyc-tlc-data/releases/download/green/green_tripdata_2019-09.csv.gz
sudo chown -R 5050:5050 data_pgadmin # https://stackoverflow.com/questions/64781245/permission-denied-var-lib-pgadmin-sessions-in-docker
docker-compose up
```

```sql
SELECT * from green_trip LIMIT 10;

SELECT count(*)
FROM green_trip 
WHERE 
	date(lpep_pickup_datetime) = '2019-09-18' AND 
	date(lpep_dropoff_datetime) = '2019-09-18';
```

## Q5

```sql
SELECT zones."Borough" as Borough, SUM(total_amount)
FROM green_trip 
JOIN zones ON "PULocationID" = zones."LocationID"
WHERE date(lpep_pickup_datetime) = '2019-09-18'
GROUP BY zones."Borough";
```

## Q6
```sql
SELECT dz."Zone", "tip_amount"
FROM green_trip 
JOIN zones as pz ON "PULocationID" = pz."LocationID"
JOIN zones as dz ON "DOLocationID" = dz."LocationID"
WHERE 
	date(lpep_pickup_datetime) >= '2019-09-01' 
	AND date(lpep_pickup_datetime) <= '2019-09-30'
	AND pz."Zone" = 'Astoria'
ORDER BY "tip_amount" DESC;
```