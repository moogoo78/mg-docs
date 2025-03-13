---
date: 2025-01-15
categories:
  - Tech
tags:
  - csv
  - rust
---

# 處理CSV的命令列利器: XSV


## 安裝

因為是rust程式寫的，要先裝[Cargo](https://doc.rust-lang.org/cargo/getting-started/installation.html)。

Linux環境:

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

## 處理TaiCOL物種清單

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

速度超快、pipeline處理資料很直覺

