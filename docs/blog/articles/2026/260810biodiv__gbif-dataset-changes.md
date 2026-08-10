---
date: 2026-08-10
categories:
  - BiodiversityInformation
tags:
  - biodiversity
  - gbif
  - museum
  - dataset
  - citizen-science
slug: gbif-dataset-changes
---

# 紀錄一下 GBIF 上幾個台灣的資料集變遷

GBIF 上從 [TaiBIF](https://taibif.tw) 發布的資料集經過幾十年的累積，有一些狀態變遷，並非只是單純的筆數增加，同一批資料很可能已經換成另一個 dataset：新的 key、新的 DOI...，這常會讓資料查詢者造成困擾，這邊就我所知道的簡單紀錄一下各資料集的變遷歷程。

<!-- more -->

## 資料集變遷的原因

大概是這幾種原因：

- **當初是為了某個短期計畫而發布**：計畫有期程，上架的內容通常侷限在那幾個年度、那幾個類群；但後端資料庫還是持續累積資料，久了計畫版的資料集就跟現況差很多。
- **發布單位變動**：早期不少資料是由數位典藏計畫（TELDAP）或主管機關（林務局）代為上架，發布單位掛的是計畫而不是資料的產生者。後來各單位自己註冊成為 GBIF 的發布單位，資料就搬到自己名下重新發布。
- **處理資料的人員異動**：原本資料的匯出流程沒有交接，很難接續，所以乾脆重新整理發布。
- **處理資料的流程異動**：可能因為管理系統更新，或是對資料填入資料標準有不同的處理方式，舊資料集難更新。
- **資料核心結構 (core) 變化**：例如從 occurrence 改成 sampling event，欄位對應整個重做，也就不會沿用原本的資料集。

不管是哪一種，對使用者來說結果都一樣：**資料集的 key 跟 DOI 換了**。

## 臺灣兩棲類資源調查

以前是因為林務局的計畫而上傳/發布資料集，計畫結束後就停在那裡，但是後續資料庫持續累積資料。另外這次也換了發布單位跟資料集結構，兩份舊的都被新的一份取代：

| | 舊（GBIF） | 舊（只在 IPT） | 新 |
|---|---|---|---|
| 名稱 | 臺灣兩棲類資源調查與教育宣導推廣計畫 | 外來種斑腿樹蛙控制與監測計畫 | 臺灣兩棲類資源調查網 (Taiwan Amphibians Database) |
| 位置 | [2de58bfe…](https://www.gbif.org/dataset/2de58bfe-1bf1-4318-97a3-d97efc269a4f) | [ipt.taibif.tw `a10200616`](https://ipt.taibif.tw/resource?r=a10200616) | [1d9b55a1…](https://www.gbif.org/dataset/1d9b55a1-1dfb-4b9d-9431-47de185ceea7) |
| DOI | 10.15468/n3rqfv | 無（未註冊到 GBIF） | 10.15468/kcja4b |
| 發布單位 | 林務局／FANCA | 林務局／FANCA | 台灣兩棲類動物保育協會 |
| 型態 | Occurrence | Occurrence | **Sampling Event** |
| 筆數 | 91,356（102–105 年） | 1,516（2013 斑腿樹蛙） | 379,397 |
| 最後發布 | 2017-06[^ipt_admin] | 2014-09[^ipt_admin] | 2026-03 |

兩份舊的都是林務局的計畫成果，以計畫為單位上架；新的改由協會自己維護、持續更新，而且從 occurrence 改成 sampling event，有調查事件（時間、樣點、方法）可以接，做族群趨勢分析比較有用。`a10200616` 那份斑腿樹蛙移除監測資料一直只放在 IPT、沒有註冊到 GBIF，所以用 GBIF 搜是找不到的。

## 國立臺灣博物館 NTM

| | 舊 | 新 |
|---|---|---|
| 名稱 | National Taiwan Museum | Biological Collection of the National Taiwan Museum, Taipei, Taiwan |
| dataset | [ca22abc0…](https://www.gbif.org/dataset/ca22abc0-98a1-11de-b4da-b8a03c50a862) | [24f1ac6a…](https://www.gbif.org/dataset/24f1ac6a-8f1d-4d23-8453-e5ce18bff54c) |
| DOI | 10.15468/mpqvtt | 10.15468/fxzm63 |
| 發布單位 | TELDAP | National Taiwan Museum |
| 筆數 | 1,470（只有魚類） | 75,432（魚、鳥、貝、甲殼、兩爬…） |
| 最後發布 | 2014-03 | 2025-11 |

舊的那份是數位典藏時期（TELDAP）代發布的魚類標本；新的是臺博館自己以發布單位身分上架的生物學組典藏，範圍大很多。舊 dataset 目前還在線上，但內容停在 2014 年那版，沒有再更新。

## 特生中心植物標本館 TAIE

這份資料集除了原始資料管理系統更新，機構名字也換了（特有生物研究保育中心於 2023 年改制為農業部生物多樣性研究所）：

| | 舊 | 新 |
|---|---|---|
| 名稱 | Endemic Species Research Institute–Herbarium | Herbarium of Taiwan Biodiversity Research Institute, TAIWAN (TAIE) |
| dataset | [bc76c690…](https://www.gbif.org/dataset/bc76c690-60a3-11de-a447-b8a03c50a862) | [19c3400b…](https://www.gbif.org/dataset/19c3400b-b7bb-425f-b8c5-f222648b86b2) |
| DOI | 10.15468/zbmpzt | 10.15468/4uvvfx |
| 發布單位 | TELDAP | Taiwan Biodiversity Research Institute |
| 型態／筆數 | Metadata-only，0 筆 | Occurrence，54,508 筆 |
| 最後發布 | 2023-03 | 2026-07 |

跟 NTM 一樣是 TELDAP → 機構自己發布，不過舊的沒有被刪除，而是降成 metadata-only，並且在描述最上面直接寫了一句「The dataset has been updated to …」附新網址，交代的很清楚。

## 中研院生物多樣性研究博物館植物標本館 HAST

| | 舊 | 新 |
|---|---|---|
| 名稱 | Database of Native Plants in Taiwan | Herbarium, Biodiversity Research Museum, Academia Sinica (HAST) |
| dataset | [96c33790…](https://www.gbif.org/dataset/96c33790-a2b5-11de-9f79-b8a03c50a862) | [10cfe1fe…](https://www.gbif.org/dataset/10cfe1fe-ea15-4b73-b495-66c20d839bef) |
| DOI | 10.15468/h1txwb | 10.15468/c664ha |
| 發布單位 | TELDAP | Biodiversity Research Museum, Academia Sinica, Taiwan |
| 筆數 | 84,866 -> 0（已降為 metadata-only） | 126,706 |
| 狀態 | 2014-03，2026-05-18 已被 GBIF 刪除，標記為新資料集的 duplicate | 2025-10 上架 |

舊的已經被 GBIF 標為 `deleted`，並且填了 `duplicateOfDatasetKey` 指向新的那份，所以舊頁面只剩存查用。授權也從 CC BY 4.0 變成 CC BY-NC 4.0，要注意。

## 要注意的地方

- **DOI 換了，引用不連續**。舊 DOI 在 GBIF literature 累積的引用（HAST 1,282 篇、TAIE 551 篇、NTM 167 篇、兩棲類 110 篇）不會自動轉到新 dataset 名下（新的分別是 19、597、210、65 篇）。做引用統計或成效報告時要記得兩份都算。

（資料狀態查詢時間：2026-08-10，來自 GBIF Registry API）

[^ipt_admin]: 排除掉因為IPT管理員更新造成的"新發布版本"。
