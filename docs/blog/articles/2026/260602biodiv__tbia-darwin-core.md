---
date: 2026-06-02
categories:
  - BiodiversityInformation
tags:
  - biodiversity
  - darwincore
  - tbia
  - data-standard
---

# TBIA 生物多樣性領域資料標準跟 Darwin Core 不同的地方

[臺灣生物多樣性資訊聯盟(Taiwan Biodiversity Information Alliance, TBIA)](https://tbiadata.tw/zh-hant/about#about)，推出了[TBIA 生物多樣性領域資料標準](https://docs.tbiadata.tw/biodiversity_data_standard/)，也上架到[資料標準列表 - 政府資料標準平臺 schema.gov.tw](https://schema.gov.tw/lists?ct=113,114,115,116,117)，我大概知道其中很多欄位定義是取用於[Darwin Core](https://dwc.tdwg.org/) (DwC, 目前全世界流通的生物多樣性資料標準)，但還是有少數不同的地方，很好奇的就請AI比較一下。

但我用自己的方法去理解: DarwinCore terms是Buffet裡提供所有食材(詞彙標準)，想吃什麼就自己去拿裝滿一盤就是那次要發布的資料集。而領域資料標準是單點的串燒(交換規範)，用[台灣物種名錄 TaiCOL](https://taicol.tw)把所有資料串在一起，這個單點的串燒，有一些規定要遵守，例如︰一定會有青椒(必填欄位)，其他如果有串培根肉的話就一定要捲金針菇(條件必填)，可以選擇熟度(敏感資料層級)，也有一些食材沒提供但單點串燒才有的豬血糕(`selfProduced`, `{taxonRank}Chinese`)，國內政府單位要點菜或出菜的話，不管是那一家餐廳，菜單都會有客製化串燒，不會有太多驚喜，但也不容易走味。

我也不知道我在說什麼了，以下讓AI來說明... `TBIA`代表領域資料標準，`DwC`代表TDWG的Darwin Core (我懶的修改，先直接丟上來，可能會有錯)。

```
=== 我是害羞的分隔線 (灬ºωº灬) : 以下是AI產生 =====
```

## 1. 詞彙標準 vs 交換規範

DwC 提供的是一組欄位（term），像 `occurrenceID`、`eventDate`、`scientificName`、`decimalLatitude`，但很少規定哪些必填、型別限制或控制詞彙。

TBIA 補上了這些規則：

- 欄位必要性：必填 (M)、條件必填 (C)、選填 (O)
- 控制詞彙表、資料格式要求
- 空值規範（文字補 `NA`、數值補 `-99999`）

## 2. 增加台灣在地欄位

DwC 沒有、但 TBIA 加上的欄位例如：

| TBIA 欄位 | 用途 |
|-----------|------|
| `countyCode` | 台灣行政區代碼 |
| `kingdomChinese` 等 | 中文分類階層名稱 |
| `selfProduced` | 是否第一手資料 |
| `sensitiveCategory` | 敏感層級 |

並引用數位發展部的共通資料標準（經緯度、日期格式、授權欄位等）。

## 3. 分類名錄綁定 TaiCoL

DwC 在分類欄位不強制綁定特定名錄，可自由對接 GBIF Backbone、Catalog of Life 等。

TBIA 則**明文以中研院「臺灣物種名錄 (TaiCoL)」為核心**，並保留原始鑑定到標準學名的校正流程：

```text
verbatimIdentification（原始鑑定名稱）
        ↓ 名錄校正
scientificName（標準學名）
```

## 4. 敏感資料管理

DwC 只有 `informationWithheld`、`dataGeneralizations` 等通用欄位。TBIA 直接定義敏感層級（輕度、重度、縣市、坐標不開放、物種不開放…），並規範坐標模糊化程度，對應台灣的生物敏感資料開放政策。

## 5. 結合台灣調查情境的延伸模板

TBIA 針對國內政策與常見調查技術發展出主題模板，這是國際 DwC 沒有的行政維度：

- 生態檢核資料交換（對應公共工程生命週期）
- 自動相機資料交換（參考 Camtrap DP）
- 自然史典藏資料交換

## 怎麼選？

- 要上傳國際平台（GBIF、OBIS）→ 直接用 **Darwin Core**
- 在台灣做生態調查、政府生態檢核、與國內單位介接 → 用 **TBIA 資料標準**

因為 TBIA 本來就是基於 DwC 發展，未來要轉回 DwC 輸出到國際，欄位銜接也不會有太大障礙。

```
=== 我是害羞的分隔線 (灬ºωº灬) : 以上是AI產生 =====
```
