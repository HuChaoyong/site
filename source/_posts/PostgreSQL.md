---
title: PostgreSQL
catalog: true
date: 2018-11-01 00:08:31
subtitle:
header-img:
tags:
- Database
- PostgreSQL
---
<center>PostgreSQL</center>

# Insert Geometry into Table[PostGIS]
```sql
-- insert a wkid 4326 point. langitude 103.24, latitude 30.28
insert ST_GeomFromText('POINT(103.24 30.28)',4326)

-- insert a line
insert ST_GeomFromText('LINESTRING(103.24 30.28, 103.29 30.33, 103.51 30.15)',4326)

-- insert a polygon
insert ST_GeomFromText('POLYGON((103.24 30.28, 103.29 30.33, 103.51 30.15))',4326)
```

# Concat All column value and split column
## Concat
```sql
select name from students
```
| name        |
| -----------|
| 张三        |
| 李四        |
| 王五        |
```sql
select string_agg(name, ',') as nameConcat from students
```
| nameConcat          |
| --------------------|
| 张三,李四,王五        |
---------------------------------------------------
## Split
```sql
select nameConcat from ConcatTB
```
| nameConcat          |
| --------------------|
| 张三,李四,王五        |

```sql
select regexp_split_to_table(name, ',') as name from ConcatTB
```
| name        |
| -----------|
| 张三        |
| 李四        |
| 王五        |
# Get all column info of table
``` sql
select * from information_schema.columns
where table_schema='public' and table_name='student'
```