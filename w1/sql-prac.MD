
```bash
SELECT * FROM yellow_taxi_trips LIMIT 2;
```

```bash
SELECT * 
FROM 
	yellow_taxi_trips t,
	zones zpu, 
	zones zdo
WHERE 
	t."PULocationID" = zpu."LocationID" AND
	t."DOLocationID" = zdo."LocationID"
LIMIT 100;
```

---

```bash
SELECT
	tpep_pickup_datetime,
	tpep_dropoff_datetime,
	total_amount,
	CONCAT(zpu."Borough", ' / ', zpu."Zone") AS "pick_up_loc",
	CONCAT(zdo."Borough", ' / ', zdo."Zone") AS "drop_up_loc"
FROM 
	yellow_taxi_trips t,
	zones zpu, 
	zones zdo
WHERE 
	t."PULocationID" = zpu."LocationID" AND
	t."DOLocationID" = zdo."LocationID"
LIMIT 100;
```

This is same as:
```bash
SELECT
	tpep_pickup_datetime,
	tpep_dropoff_datetime,
	total_amount,
	CONCAT(zpu."Borough", ' / ', zpu."Zone") AS "pick_up_loc",
	CONCAT(zdo."Borough", ' / ', zdo."Zone") AS "drop_up_loc"
FROM 
	yellow_taxi_trips t 
	JOIN zones zpu 
		ON t."PULocationID" = zpu."LocationID"
	JOIN zones zdo
		ON t."DOLocationID" = zdo."LocationID"
LIMIT 100;
```

---

Checking whether a column or two are null:
```bash
SELECT
	tpep_pickup_datetime,
	tpep_dropoff_datetime,
	total_amount,
	CONCAT(zpu."Borough", ' / ', zpu."Zone") AS "pick_up_loc",
	CONCAT(zdo."Borough", ' / ', zdo."Zone") AS "drop_up_loc",
	"PULocationID",
	"DOLocationID"
FROM 
	yellow_taxi_trips t 
	JOIN zones zpu 
		ON t."PULocationID" = zpu."LocationID"
	JOIN zones zdo
		ON t."DOLocationID" = zdo."LocationID"
WHERE
	"PULocationID" is NULL OR
	"DOLocationID" is NULL	
LIMIT 100;
```

---

Checking whether a column value is present in one table but not in another that are joined together:

```bash
SELECT
	*
FROM 
	yellow_taxi_trips t 
WHERE
	"DOLocationID" NOT IN (SELECT "LocationID" FROM zones) OR
	"PULocationID" NOT IN (SELECT "LocationID" FROM zones)
LIMIT 100;
```

---

Delete an item and check with the prev query:

```bash
DELETE FROM zones WHERE "LocationID" = 142;
```
 
```bash
SELECT
	tpep_dropoff_datetime,
	DATE_TRUNC('DAY', tpep_dropoff_datetime),
	CAST(tpep_dropoff_datetime AS DATE) AS "casted day	",
	total_amount
FROM 
	yellow_taxi_trips t 
LIMIT 100;
```


Group by:
```bash
SELECT
	CAST(tpep_dropoff_datetime AS DATE) AS "casted day	",
	COUNT(1)
FROM 
	yellow_taxi_trips t 
GROUP BY 
	CAST(tpep_dropoff_datetime AS DATE);
```

```bash
SELECT
	CAST(tpep_dropoff_datetime AS DATE) AS "casted day",
	COUNT(1)
FROM 
	yellow_taxi_trips t 
GROUP BY 
	CAST(tpep_dropoff_datetime AS DATE)
ORDER BY "casted day" ASC;
```
- ORDER BY param is case-sensitive


```bash
SELECT
	CAST(tpep_dropoff_datetime AS DATE) AS "casted day",
	COUNT(1) AS "count",
	MAX(total_amount),
	MAX(passenger_count)
FROM 
	yellow_taxi_trips t 
GROUP BY 
	CAST(tpep_dropoff_datetime AS DATE)
ORDER BY "count" DESC;
```

```bash
SELECT
	CAST(tpep_dropoff_datetime AS DATE) AS "casted day",
	"DOLocationID",
	COUNT(1) AS "count",
	MAX(total_amount),
	MAX(passenger_count)
FROM 
	yellow_taxi_trips t 
GROUP BY 
	1, 2
ORDER BY "count" DESC;
```


```bash
SELECT
	CAST(tpep_dropoff_datetime AS DATE) AS "day",
	"DOLocationID",
	COUNT(1) AS "count",
	MAX(total_amount),
	MAX(passenger_count)
FROM 
	yellow_taxi_trips t 
GROUP BY 
	1, 2
ORDER BY 
	"day" ASC,
	"DOLocationID" ASC;
```