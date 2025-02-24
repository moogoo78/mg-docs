# PostgreSQL

## How-to (snippets)

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

Query date and time
```sql
SELECT * FROM my_table WHERE datetime::date >= '2022-12-31' AND datetime::time >= '16:01'
```

#### split part

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
SELECT setval('my_table_id_seq', (SELECT max(id) FROM my_table));
```

通常是執行了帶有auto-increment id的INSERT INTO，造成sequence沒有更新


## Functions

- [PostgreSQL: Documentation: 16: Chapter 9. Functions and Operators](https://www.postgresql.org/docs/current/functions.html)


## Optimize

- [How we optimized PostgreSQL queries 100x | by Vadim Markovtsev | Towards Data Science](https://towardsdatascience.com/how-we-optimized-postgresql-queries-100x-ff52555eabe)

- [How we optimized Python API server code 100x | by Vadim Markovtsev | Towards Data Science](https://towardsdatascience.com/how-we-optimized-python-api-server-code-100x-9da94aa883c5)

- [Explain PostgreSQL](https://explain.tensor.ru/plan/) 視覺化 explain

Check active queries:

```sql
SELECT pid, query, state
FROM pg_stat_activity
WHERE state != 'idle';
```

Look for long-running operations:

```sql
SELECT pid, query, state, age(clock_timestamp(), query_start) as query_duration
FROM pg_stat_activity
WHERE state != 'idle'
ORDER BY query_duration DESC;
```

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


```sql
CREATE INDEX index_name
ON table_name (column1, column2, ...);
```

## JSON-related

```sql
SELECT *
FROM unit
WHERE EXISTS (
    SELECT 1
    FROM jsonb_array_elements(source_data->'dups') AS dups
    WHERE dups->>'dupID' = '184308'
);
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
- [Choosing a Postgres Primary Key](https://supabase.com/blog/choosing-a-postgres-primary-key)


## Versions

- [《 PostgreSQL 各版本特性及差異比較表 》 »... - Ant Yi-Feng Tzeng | Facebook](https://www.facebook.com/yftzeng.tw/posts/pfbid02ykJJUubLDfdQ3oZcr88P8WYK9it4UHqv9BKQSYS3UpAGKEwNeeUjC66Heice62cDl)

### Version 15

[熱騰騰！PostgreSQL 15 大版本更新報你知！【Webinar： PostgreSQL】| 歐立威科技 - YouTube](https://www.youtube.com/watch?v=I-HNa2JJBPA)

#### pg_basebackup

- pg_basebackup -Ft -Z server-zstd -p 5436 -D {PATH} -Pv
- gzip, LZ4, sztd
    - 壓縮比: zstd > gzip > LZ4
    - 花費時間: gzip >> zstd > LZ4
- zstd 壓縮比 gzip 大一點點，但是時間差很多
- select pg_current_wal_lsn();
- wal_compression=zstd
- ex: 早晚跑一次
    - 0/750071C8
    - 0/B90A17A8
- select pg_size_pretty(pg_wal_lsn_diff(‘0/B90A17A8’, ‘0/750071C8‘))
    - 1089 MB
    - 可以看整天的交易量多少

## Backup

- pg_* 的備份 (跟 postgres 版本相依)
    - pg_dump: logical backup
        - 產生 SQL statement
        - 執行比較久
        - can backup parts of a database
    - pg_basebackup: physical backup
        - copy database cluster files
        - can only backup whole cluster
        - 可以以時間點隨選還原 Continuous Archiving and Point-in-Time Recovery ([PITR](https://docs.postgresql.tw/server-administration/backup-and-restore/continuous-archiving-and-point-in-time-recovery-pitr))

- pgbackrest
    - support zstd (不受 Postgres 版本影響)
    - 金鑰備份
    - incremental backup and restore
    - 透過 pg_start_backup, pg_stop_backup, 不影響交易
    - standby 備份 ? (閒置資源時備份?)

## Tools
- [pgloader](https://pgloader.io/) - load data from files, such as CSV or Fixed-File Format; or migrate a whole database to PostgreSQL

- [dalibo/pg_activity: pg_activity is a top like application for PostgreSQL server activity monitoring.](https://github.com/dalibo/pg_activity)
