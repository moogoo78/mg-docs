# SQLite

[SQLite Home Page](https://www.sqlite.org/index.html)

## tools

### GUI

- GUI client: [GitHub - little-brother/sqlite-gui: Lightweight SQLite editor for Windows](https://github.com/little-brother/sqlite-gui?utm_source=hackernewsletter&utm_medium=email&utm_term=data)

### schema

- [Screwtape / sqlite-schema-diagram · GitLab](https://gitlab.com/Screwtapello/sqlite-schema-diagram)

[SQLite is not a toy database](https://antonz.org/sqlite-is-not-a-toy-database/)

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

```text title="show run-time"
.timer on
```

### export

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

```text title="dump sql"
sqlite> .output /path/to/out.sql
sqlite> .dump table_name
```

### import

Import from csv

```text title="import from csv"

 sqlite> .headers on
 sqlite> .mode csv
 sqlite> .import {FILE} {TABLE_NAME}
```

```text title="import from tsv (tabbed separate)"
 sqlite> .mode tab
 sqlite> .import {FILE} {TABLE_NAME}
```

```text title="import from tsv (tabbed separate), can escape double quote in text"

 sqlite> .mode ascii # tab 或column都沒用
 sqlite> .separator "\t"
 sqlite> .import {FILE} {TABLE_NAME}

```
如果資料裡有 `|` (vertical bar)，SQLite import也會造成欄位判斷錯誤，如: `expected 1 columns but found 2 - extras ignored`，用以上方法也可以成功匯入。

[Command Line Shell For SQLite](https://www.sqlite.org/cli.html)

```
sqlite> .import --csv foo.csv footable
```

```bash title="export/import csv by command"
$ sqlite3 -header -csv c:/sqlite/chinook.db "select * from tracks;" > tracks.csv
$ sqlite3 -header -csv c:/sqlite/chinook.db < query.sql > data.csv
```

Import from SQL file

```bash
sqlite> .read db.sql
```

or

```bash
cat db.sql | sqlite3 database.db
```

### mode

設定輸出格式，預設是 `list` ("|"分隔)



## Data type

[Datatypes In SQLite](https://www.sqlite.org/datatype3.html)

### Storage Classes

以下在SQlite儲存時定義不同的儲存格式：

- **NULL**. The value is a NULL value.
- **INTEGER**. The value is a signed integer, stored in 0, 1, 2, 3, 4, 6, or 8 bytes depending on the magnitude of the value.
- **REAL**. The value is a floating point value, stored as an 8-byte IEEE floating point number.
- **TEXT**. The value is a text string, stored using the database encoding (UTF-8, UTF-16BE or UTF-16LE).
- **BLOB**. The value is a blob of data, stored exactly as it was input.

其他：

- SQLite does not have a separate Boolean storage class. Instead, Boolean values are stored as integers 0 (false) and 1 (true).
- SQLite does not have a storage class set aside for storing dates and/or times. 但可用以下方式儲存，搭配[Date And Time Functions](https://www.sqlite.org/lang_datefunc.html)處理。
    - **TEXT** as ISO8601 strings ("YYYY-MM-DD HH:MM:SS.SSS").
    - **REAL** as Julian day numbers, the number of days since noon in Greenwich on November 24, 4714 B.C. according to the proleptic Gregorian calendar.
    - **INTEGER** as Unix Time, the number of seconds since 1970-01-01 00:00:00 UTC.


## Full Text Search In SQLite

### Intro

via: [SQLite for beginners: Full Text Search - YouTube](https://www.youtube.com/watch?v=eXMA_2dEMO0)

一般*LIKE* query
```sql title="LIKE operator"
SELECT count(*)
FROM titles
WHERE primary_title LIKE '%star wars%';
```

加index快一點
```
CREATE INDEX title_primary_title_idx on titles (primary_title);
```

### using FTS5

```text title="check if has fts5 installed"
sqlite> PRAGMA compile_options;
```


```sql title="create virtual table using fts5"
CREATE VIRTUAL TABLE titles_fts
USING fts5(
  title_id, primary_titel, original_title
);
```

```sql title="insert data to fts virtual table"
INSERT INTO titles_fts (title_id, primary_title, original_title)
SELECT title_id, primary_title, original_title
FROM titles;
```

can use *TRIGGER* to sync both tables (watch youtube for more info)

```sql title="match all columns"
SELECT count(*)
FROM titles t
INNER JOIN tables_fts s on s.title_id = t.title_id
WHERE titles_fts MATCH 'star wars';
```

```sql title="match specific columns"
SELECT count(*)
FROM titles t
INNER JOIN tables_fts s on s.title_id = t.title_id
WHERE titles_fts MATCH '{primary_title original_title}: star wars';
```

```sql title="get matched top 10"
SELECT t.primary_title, t.original_title
FROM titles t
INNER JOIN tables_fts s on s.title_id = t.title_id
WHERE titles_fts MATCH '{primary_title original_title}: star wars'
LIMIT 10;
```

```sql title="bm25 ranking function (0, 10, 2 is weight of column)"
SELECT bm25(titles_fts, 0, 10, 2), rank, t.primary_title, t.original_title
FROM titles t
INNER JOIN tables_fts s on s.title_id = t.title_id
WHERE titles_fts MATCH '{primary_title original_title}: star wars'
LIMIT 10;
```
rank seems not work...

### spellfix1 (search for close matches)

[The Spellfix1 Virtual Table](https://www.sqlite.org/spellfix1.html)

In debian, need build spellfix1.so

```bash
apt install libsqlite3-dev
```
download [SQLite: spellfix.c at trunk](https://www.sqlite.org/src/file?name=ext/misc/spellfix.c&ci=trunk)

```bash
gcc -shared -fPIC -o spellfix1.so spellfix.c -lsqlite3
```

generate `spellfix1.so`

```text
sqlite> .load ./spellfix1.so
```

(每次開啟都要load)

```sql
CREATE VIRTUAL TABLE demo USING spellfix1;
INSERT INTO demo(word) SELECT foo FROM some_table;
SELECT word FROM demo WHERE word MATCH 'kennes*' AND top=5;
```

#### soundslike

查salm或chay時，psalm或Tchai...都會出現

```sql
INSERT INTO demo(word,soundslike) VALUES('psalm','salm');
INSERT INTO demo(word,soundslike) VALUES('Tchaikovsky','Chaykovsky');
```
