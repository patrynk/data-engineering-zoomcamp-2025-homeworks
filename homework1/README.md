# Question 1: Version of pip in python:3.12.8 docker image
```bash
docker run -it --entrypoint /bin/bash python:3.12.8
pip --version
```
pip 24.3.1 from /usr/local/lib/python3.12/site-packages/pip (python 3.12)

# Question 2: No associated code

# Question 3: Trip Segmentation Count
## 3.1, up to 1 mile
```sql
SELECT 
	COUNT(*)
FROM 
	green_taxi
WHERE 
	CAST(lpep_dropoff_datetime AS TIMESTAMP) >= '2019-10-01' AND 
	CAST(lpep_dropoff_datetime AS TIMESTAMP) < '2019-11-01' AND 
	trip_distance <= 1;
```
104,802
## 3.2, 1 to 3 miles
```sql
SELECT 
	COUNT(*)
FROM 
	green_taxi
WHERE 
	CAST(lpep_dropoff_datetime AS TIMESTAMP) >= '2019-10-01' AND 
	CAST(lpep_dropoff_datetime AS TIMESTAMP) < '2019-11-01' AND 
	trip_distance > 1 AND
	trip_distance <= 3;
```
198,924
## 3.3, 3 to 7 miles
```sql
SELECT 
	COUNT(*)
FROM 
	green_taxi
WHERE 
	CAST(lpep_dropoff_datetime AS TIMESTAMP) >= '2019-10-01' AND 
	CAST(lpep_dropoff_datetime AS TIMESTAMP) < '2019-11-01' AND 
	trip_distance > 3 AND
	trip_distance <= 7;
```
109,603
## 3.4, 7 to 10 miles
```sql
SELECT 
	COUNT(*)
FROM 
	green_taxi
WHERE 
	CAST(lpep_dropoff_datetime AS TIMESTAMP) >= '2019-10-01' AND 
	CAST(lpep_dropoff_datetime AS TIMESTAMP) < '2019-11-01' AND 
	trip_distance > 7 AND
	trip_distance <= 10;
```
27,678
## 3.5, > 10 miles
```sql
SELECT 
	COUNT(*)
FROM 
	green_taxi
WHERE 
	CAST(lpep_dropoff_datetime AS TIMESTAMP) >= '2019-10-01' AND 
	CAST(lpep_dropoff_datetime AS TIMESTAMP) < '2019-11-01' AND 
	trip_distance > 10;
```
35,189

# 4. Longest trip for each day
```sql
SELECT 
    trip_distance,
	lpep_pickup_datetime
FROM green_taxi
ORDER BY trip_distance DESC
LIMIT 1;
```
2019-10-31 23:23:41

# 5. Three biggest pickup zones
```sql
SELECT 
    z."Zone",
    SUM(t.total_amount) as total
FROM 
    green_taxi t
    JOIN zones z ON t."PULocationID" = z."LocationID"
WHERE 
    CAST(t.lpep_pickup_datetime AS DATE) = '2019-10-18'
GROUP BY 
    z."Zone"
HAVING 
    SUM(t.total_amount) > 13000
ORDER BY 
    total DESC;
```
East Harlem North, East Harlem South, Morningside Heights

# 6. Largest Tip
```sql
SELECT 
    dropoff."Zone" as dropoff_zone,
    t.tip_amount
FROM 
    green_taxi t
    JOIN zones pickup 
        ON t."PULocationID" = pickup."LocationID"
    JOIN zones dropoff 
        ON t."DOLocationID" = dropoff."LocationID"
WHERE 
    CAST(t.lpep_pickup_datetime AS TIMESTAMP) >= '2019-10-01'
    AND CAST(t.lpep_pickup_datetime AS TIMESTAMP) < '2019-11-01'
    AND pickup."Zone" = 'East Harlem North'
ORDER BY 
    t.tip_amount DESC
LIMIT 1;
```
JFK Airport