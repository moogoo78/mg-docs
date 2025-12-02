---
date: 2025-11-27
categories:
  - Tech
tags:
  - database
  - duckdb
  - csv
---

- 2025-12-02 補充

# DuckDB - 直接用SQL語法篩選資料，免匯入

想收集[TBIA](https://tbiadata.tw/)所有「紀錄者/採集者」 (`recordedBy`)的名字，先想到用之前有使用過的[XSV](./250314tech__xsv.md)，但要安裝時發現已經沒有在維護了，XSV的介紹叫大家改用[qsv](https://github.com/dathere/qsv)，跟XSV一樣參數簡單、速度超快。TBIA下載所有的資料，解壓縮完有27.9GB (29,536,885筆資料)，用qsv讀取recordedBy跟拿掉重複輸出，大概幾十秒就做完了。

```bash
qsv select recordedBy tbia_{hash}.csv | qsv dedup

```

但我想到要比對一下人名跟資料集名稱 (`datasetName`)對映，順便也統計一下筆數，似乎就要寫一點點程式，查一下資料才發現duckdb這套強大的資料庫可以直接用我熟悉的SQL語法處理，甚至不用匯入，直接對CSV欄位名稱下語法就好，速度一樣超快，還有漂亮的輸出，真是相見恨晚。

先簡單列出不重複的人名

![duckdb-recordedBy](../../../assets/blog/2025/duckdb-1.png)

加上資料集名稱與統計關聯的資料筆數

```bash
duckdb -c "SELECT recordedBy, datasetName, count(*) FROM 'tbia_6927dd263f39e705b9843d70.csv' GROUP BY recordedBy, datasetName ORDER BY count(*)"
```

![duckdb-recordedBy-count](../../../assets/blog/2025/duckdb-2.png)

好直覺、好快 ~~

[DuckDB – An in-process SQL OLAP database management system](https://duckdb.org/)


補充應用:

篩選出資料集名稱為: "科博典藏 (NMNS Collection)-真菌學門" 的分類資訊`。


```bash
duckdb -csv -c " select id, kingdom, kingdom_c, phylum, phylum_c, 'class', class_c, 'order', order_c, family, family_c, genus, genus_c, scientificName, common_name_c from 'tbia_6927dd263f39e705b9843d70.csv' where datasetName='科博典藏 (NMNS Collection)-真菌學門'" > nmns-fungi.csv
```

-csv 設定 `.mode csv`，不然的話預設會匯出duckbox (有框線)
