# Data Analyst Challenge

# Local Setup
1. Download SQL dump from GS bucket;
2. Extract it and import dump into a SQL engine;

## Hints
The SQL dump is based on a postgres engine, you should stick to using postgres and avoid extra fuss.

### 1. Install Docker and a postgres (cli)
Docker instalation is well documented, for ubuntu [docs here](https://docs.docker.com/engine/install/ubuntu/). 

A Postgres client can be installed in ubuntu with:
```
sudo apt-get update
sudo apt-get install postgresql
```

Other plaforms have well documented install guides for these requirements and you shouldn't have any trouble, but if you are struggling just ping us and we'll find a way to get you moving.


### 2. Download and extract sql dump
```
curl https://storage.googleapis.com/dott-data-staging-sandbox/dott_exercise.sql.tar.gz > dump.sql.tar.gz

tar -xzf dump.sql.tar.gz
```

### 3. Run a postgres server
```
docker run --name dott-postgres -p 5432:5432 -e POSTGRES_PASSWORD=pwd -e POSTGRES_USER=docker -d postgres:9.6
```

### 4. Import the SQL dump into your new server
```
psql -h localhost -p 5432 -U docker -f dott_exercise.sql
```

### 5. Test it
```
psql -h localhost -p 5432 -U docker -d dott
SELECT COUNT(1) FROM exercise.sales;
# 4993
```
All done!

# SQL Exercises
Get the following insights:
## 1. Sales by Year Month
We are interested in covering **all** months, so if we have zero sales over a month, we still need to show that month in the result set.

## 2. Cumulative Evolution of offers with/without stock over time till current year month
Assume offers have stock on creation time. If we have an offer with the following record:
| offer_id | stock | created_at | modified_at |
| - | - | - | - |
| 1 | 0 | 2020-01-05 | 2020-03-21
We can interpret this as: the last date this offer had stock was at 2020-03-21. 

If this was the single offer in the DB, the expected result would look like:
| year_month | nr_offers_stock | total_nr_offer |
| - | - | - |
| 2020-01 | 1 | 1 |
| 2020-02 | 1 | 1 |
| 2020-03 | 1 | 1 |
| 2020-04 | 0 | 1 |
| ... | 0 | 1 |
| 2021-01 | 0 | 1 |


# Dimensional Modelling
1. Identify key business processes within the relational data model;
2. Detail your approach on how to model facts and dimension tables for at least one of the previous business processes;

Hints: 
- [Kimball four step dimensional modelling process](https://www.kimballgroup.com/data-warehouse-business-intelligence-resources/kimball-techniques/dimensional-modeling-techniques/four-4-step-design-process/)

# Story Telling with powerBi
Based on the raw data model, the SQL views you had to build, and on any factual or dimension tables that you might have built for the previous assignement, expose those tables into a powerBi data model.

You are expected to build a dashboard where you should tell a story on a business process of your own chosing.
