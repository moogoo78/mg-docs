# PostgreSQL

## How-to

### Dump/Import

如果要部分資料匯出，如果不是用client界面，似乎只能用csv匯出，csv 匯入也是用 `\copy` ? [PostgreSQL: Documentation: 16: COPY](https://www.postgresql.org/docs/current/sql-copy.html)

```bash title="dump"

$ pg_dump -U USERNAME DBNAME > dbexport.pgsql
$ # PGPASSWORD="mypassword" pg_dump -U myusername dbname 密碼 > output.sql$ pg_dump -U postgres -f /tmp/dump.sql.gz --compress=5 --no-owner dbname
```

```bash title="import"
$ psql -f backup.sql dbname dbuser
```

```sql title="import from csv(tab txt) without header"
COPY target_table FROM '/path/to/file.txt' DELIMITER E'\t' csv;
```
`E` is escape to `\t`


#### dump specific table

``` bash
pg_dump -U xxx public.TABLE_NAME DATABASE_NAME > out.sql
pg_dump -U xxx -d DB_NAME -t TABLE_NAME > out.sql
```

#### dump schema

```bash
pg_dump -U xxx --schema-only DATABASE_NAME > schema.sql
```

#### export csv
```bash
$ psql -U user -d db_name -c "Copy (Select * From foo_table LIMIT 10) To STDOUT With CSV HEADER DELIMITER ',';" > foo_data.csv
```

```sql title="export csv with ',' delimiter and header"
\copy (Select * From foo) To '/tmp/test.csv' With CSV DELIMITER ',' HEADER
```

```sql title="export csv with ',' delimiter no header"
\copy (Select * From foo) To '/tmp/test.csv' With CSV
```

```sql title="export csv"
copy (SELECT * FROM employees) to 'path/to/file.csv' with csv
```

### Datetime related

daterange

```sql title="overlap"
SELECT * FROM my_table WHERE DATERANGE(my_start::date , my_end::date, '[)') && DATERANGE( date '2018-01-01', date '2018-01-31', '[)')
```

```sql title="group by date"
select date_trunc ('hour', created), count(*) from mytable group by 1;
```

```sql title="extract hour, day of week"
select extract(hour from created), count(*) from mytable group by 1;
select extract(dow from created), count(*) from mytable group by 1
```

```sql title="split string to array and extract"
-- memo: APP-v0.1.2/foo/20230101
select
    split_part(memo::text, '/', 1) as app,
    split_part(memo::text, '/', 2) as name,
from some_table
```


ref: [PostgreSQL: Documentation: 16: 9.9. Date/Time Functions and Operators](https://www.postgresql.org/docs/current/functions-datetime.html)

### Big data



```sql title="estimated count"
SELECT reltuples FROM pg_class WHERE relname = 'large_table'
```

```sql title="產生10M records"
INSERT INTO users
SELECT
    --- Ten million records
	generate_series(1,10000000) AS id,
    --- Example: "e6f2c6842d146c518185e1e47add9532"
    substr(md5(random()::text), 0, 50) AS name;
```

### Clean

```sql title="truncate & auto increment 從頭開始"
TRUNCATE table_name RESTART IDENTITY;
```

```sql title="sequence 亂掉 duplicate key error"
SELECT setval('my_tabel_id_seq', (SELECT max(id) FROM my_table));
```

通常是執行了帶有auto-increment id的INSERT INTO，造成sequence沒有更新


### Functions

- [PostgreSQL: Documentation: 16: Chapter 9. Functions and Operators](https://www.postgresql.org/docs/current/functions.html)


### Optimize

- [How we optimized PostgreSQL queries 100x | by Vadim Markovtsev | Towards Data Science](https://towardsdatascience.com/how-we-optimized-postgresql-queries-100x-ff52555eabe)

- [How we optimized Python API server code 100x | by Vadim Markovtsev | Towards Data Science](https://towardsdatascience.com/how-we-optimized-python-api-server-code-100x-9da94aa883c5)

- [Explain PostgreSQL](https://explain.tensor.ru/plan/) 視覺化 explain

## Admin

list indexes
via: [PostgreSQL List Indexes](https://www.postgresqltutorial.com/postgresql-indexes/postgresql-list-indexes/)

```sql
SELECT
    tablename,
    indexname,
    indexdef
FROM
    pg_indexes
WHERE
    schemaname = 'public'
ORDER BY
    tablename,
    indexname;
```

## PostGIS

enable postgis extension:

```
create extension postgis;
```

check version:

```sql
SELECT PostGIS_Full_Version();
```

### shp2pgsql

Shapefile data loader

```bash
shp2pgsql -i -s 4326 -D foo.shp > output.sql
```
[Chapter 4. Data Management](https://postgis.net/docs/using_postgis_dbmanagement.html#shp2pgsql_usage)

- -D: dump format (faster than default insert)
- -I: greate GiST index on the geometry column
- -s: SRID


```sql title="Query multipolygon from longitude, latitude"
SELECT id,name
FROM named_area
WHERE ST_Within(ST_SetSRID(ST_POINT(121.51, 24.93),4326), geom_mpoly::geometry);
```
## Reference
- [《 PostgreSQL 各版本特性及差異比較表 》 »... - Ant Yi-Feng Tzeng | Facebook](https://www.facebook.com/yftzeng.tw/posts/pfbid02ykJJUubLDfdQ3oZcr88P8WYK9it4UHqv9BKQSYS3UpAGKEwNeeUjC66Heice62cDl)
- [Choosing a Postgres Primary Key](https://supabase.com/blog/choosing-a-postgres-primary-key)









