---
date: 2025-03-14
categories:
  - Tech
tags:
  - csv
  - rust
---

# 處理CSV的命令列利器: XSV

在Global Names的Tutorial: [Parse names into a CSV file](https://globalnames.org/docs/tut-xsv-gnparser/)看到一個處理CSV超有效率的工具。

通常拿到一個csv，想快速得到這個csv的欄位有那些，總共幾筆，隨便列出幾筆看看資料長什麼樣子，然後決定只取其中幾個欄位資料，做簡單的條件篩選，最後匯出另一個我要的csv => 這些處裡可以用xsv一行指令做完。

## 安裝

因為是rust程式寫的，要先裝[Cargo](https://doc.rust-lang.org/cargo/getting-started/installation.html)。

以Linux為例:

```bash
curl https://sh.rustup.rs -sSf | sh
```

然後就可以用cargo安裝xsv

```bash
cargo install xsv
```

在shell的rc檔加上執行路徑

```bash
source "$HOME/.cargo/env""
```

## 處理情境: 整理TaiCOL物種清單

TaiCOL下載名錄，解壓縮是78MB，資料筆數233,440，不到10000元的NUC小PC處理完大概最多花不到2秒。

下載: [臺灣物種名錄 Catalogue of Life in Taiwan](https://taicol.tw/zh-hant/download)

列出headers

```bash
xsv headers TaiCOL_name_20250221.csv
```

找出type_name_id跟namecode欄位有值的，並且隨機列出10列 (`sample 10`)

```bash
xsv search -s type_name_id '.+' TaiCOL_name_20250221.csv |xsv search -s namecode '.+' | xsv select name_id,type_name_id,namecode,original_name_id | xsv sample 10
```


轉換檔案: 篩選出分類階層是種(Species)的

```bash
xsv search -s rank 'Species' TaiCOL_name_20250221.csv | xsv select simple_name,name_author,s2_rank,latin_s2,s3_rank,latin_s3,common_name_c,alternative_name_c,kingdom,kingdom_c,phylum,phylum_c,class,class_c,order,order_c,family,family_c,genus,genus_c | xsv fmt -t '\t' > out.csv
```

- `search -s rank 'Species'`: 找出rank欄位等於"Species"
- `xsv select simple_name,...`: 取用那些欄位
- `xsv fmt -t '\t'`: 轉成tab分隔 (避免處理雙引號)

篩選出有文獻的學名

```bash
xsv search -s protologue '.+' TaiCOL_name_20250221.csv |xsv select name_id,type_name_id,namecode,original_name_id,protologue | wc -l
```

## 結論

以前處理這些事情筆數少用Excel/LiberOffice，幾萬筆以上可能要開OpenRefine，不然就要寫script程式處理 (Python的話可能用Pandas之類的套件)或是開jupyter notebook處理，都是很麻煩。XSV速度超快、pipeline處理資料很直覺，適合處理大檔案。
