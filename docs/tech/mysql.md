# MySQL

## How-to

mysql command

```bash
mysql -uroot -p {database_name}

mysql> show databases;
mysql> show tables;
mysql> show status;
```

### Export

#### mysqldump

```bash title="import sql"
mysql -f -u username -p database_name < file.sql # -f 跳過錯誤繼續匯入 (重複 create table)
```

#### export to csv file

```bash
mysql -u root -pexample mydatabase -e "SELECT * FROM my_table INTO OUTFILE '/var/lib/mysql/my_table.csv' FIELDS TERMINATED BY ',' ENCLOSED BY '\"' LINES TERMINATED BY '\n';"
```
docker裡會遇到

> ERROR 1290 (HY000): The MySQL server is running with the --secure-file-priv option so it cannot execute this statement

在MySQL client裡確認

```sql
SHOW VARIABLES LIKE "secure_file_priv";
```

出現Value是NULL，用docker compose設定

```
    volumes:
      - ../taicol-volumes/mysql-data:/var/lib/mysql
    command:
      --secure-file-priv=/var/lib/mysql
```

如果設定成其他目錄會另外遇到權限問題

 > Permission denied (OS errno 13)

另外要改docker裡的權限

不過這個csv沒有header，如果要的話要hard-code select xx1, xx2 union select ...

### User權限
主要是在名為mysql的資料庫裡的user (table)

```
mysql> CREATE USER '{username}'@'{hostname}' IDENTIFIED BY '{password}'; -- hostname可以為'%'，表示任意ip都可進來

mysql> GRANT CREATE, ALTER, INSERT, UPDATE, DELETE, SELECT ON *.* TO '{username}@{hostname}'; -- *.* 是 {database.table}
mysql> GRANT ALL PRIVILEGES ON *.* TO '{username}@{hostname}';
```


## Function

### 時間

```sql
SELECT NOW(),CURDATE(),CURTIME() ;-- | 2012-06-04 14:19:42 | 2012-06-04 | 14:19:42 |
SELECT MONTH(FROM_UNIXTIME(add_date)) as month
SELECT NOW(), NOW() - INTERVAL 1 DAY ;今天跟昨天
```

ref: http://dev.mysql.com/doc/refman/5.5/en/date-and-time-functions.html


unfinished:
http://moogoo78.github.io/comp/database/mysql.html

## 問題

mysqldump error:

```bash
mysqldump: Got error: 145: Table './nc_biodiv/vbb35threadviews' is marked as crashed and should be repaired when using LOCK TABLES
```

在這邊看到[Fix error “MySQL is marked as crashed and should be repaired”](https://www.nucleustechnologies.com/blog/fix-error-mysql-is-marked-as-crashed-and-should-be-repaired/)，用以下指令解決。


```bash
myisamchk -s /var/lib/mysql/*/*.MYI
```
但也遇到錯誤

```text
myisamchk: error: 140 when opening MyISAM-table '/var/lib/mysql/{my_database}/{my_table}.MYI'
```

據說是mysql5.6的bug，我的是5.5

```bash
myisamchk  --force --update-state /var/lib/mysql/nc_biodiv/{my_table}
```

就是去掉MYI就好了

