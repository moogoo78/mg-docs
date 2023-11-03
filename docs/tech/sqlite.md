# SQLite

[SQLite Home Page](https://www.sqlite.org/index.html)

## Notice

- SQLite 沒有 truncate, 用 delete from

## Commands

```bash
  $ sqlite3 new.db # create db
  $ sqlite3 myprecious.db ".dump" ＞ output.sql # dump sql
  $ sqlite3 new.db ＜ output.sql # import
  # or
  $ cat dumpfile.sql | sqlite3 new.db
```

```text title="sqlite basic commands"
  .tables  # MySQL的show tables
  .schema TABLENAME
  .help
  .quit
```

```text title="export to csv"

   sqlite> .headers on
   sqlite> .mode csv
   sqlite> .output data.csv
   sqlite> SELECT customerid,
      ...>        firstname,
      ...>        lastname,
      ...>        company
      ...>   FROM customers;
   sqlite> .quit
```

```text title="import from csv"

   sqlite> .headers on
   sqlite> .mode csv
   sqlite> .import data.csv table_name
```

```bash title="export/import csv by command"

    $ sqlite3 -header -csv c:/sqlite/chinook.db "select * from tracks;" > tracks.csv
    $ sqlite3 -header -csv c:/sqlite/chinook.db < query.sql > data.csv
```