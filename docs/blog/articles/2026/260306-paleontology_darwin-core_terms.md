---
date: 2026-03-06
categories:
  - BiodiversityInformation
tags:
  - biodiversity
  - darwincore
  - paleontology
  - fossil
---

# 筆記一點關於古生物的Darwin Core詞彙

目前[Darwin Core](https://dwc.tdwg.org/terms/)已儼然成為生物多樣性開放資料共同標準，針對古生物標本也有一些相關詞彙可用，跟現生生物標本資料最大不同的是必填欄位basisOfRecord要用`FossilSpecimen`。(非`PreservedSpecimen`或`LivingSpecimen`)。

## Geological Context

[Geological Context](https://dwc.tdwg.org/terms/#geologicalcontext)這個Class應該就定義了大多數地層資訊用到的詞彙，列舉常用的如下：

地層學 (Stratigraphy)可以細分成可以細分岩石地層學 (Lithostratigraphy)、生物地層學 (Biostratigraphy)、年代地層學 (Chronostratigraphy)...


**年代地層學（Chronostratigraphy）**

| 層級 | Earliest 詞彙 | Latest 詞彙 | 範例 |
|------|--------------|------------|-----|
| Eon / Eonothem | `earliestEonOrLowestEonothem` | `latestEonOrHighestEonothem` | Phanerozoic |
| Era / Erathem | `earliestEraOrLowestErathem` | `latestEraOrHighestErathem` | Cenozoic |
| Period / System | `earliestPeriodOrLowestSystem` | `latestPeriodOrHighestSystem` | Neogene |
| Epoch / Series | `earliestEpochOrLowestSeries` | `latestEpochOrHighestSeries` | Holocene |
| Age / Stage | `earliestAgeOrLowestStage` | `latestAgeOrHighestStage` | Atlantic, Boreal |

!!! note

    國際地層委員會[ICS](https://stratigraphy.org/)會一直更新這個[年代地層表](https://stratigraphy.org/chart/)


**岩石地層學（Lithostratigraphy）**

| 層級 | 詞彙 |
|------|------------|
| 群 | `group` |
| 層（組） | `formation` |
| 段 | `member` |
| 小層 | `Bed` |


**生物地層學 (Biostratigraphy)**

| Earliest 詞彙 | Latest 詞彙 | 範例 |
|--------------|------------|----------|
| `lowestBiostratigraphicZone` | `highestBiostratigraphicZone` | Maastrichtian |


但原始資料遇到"晚中新世" (Late Miocene) 這種資料要如何處理？

問了一輪[Digital TaiBIFer](https://chatgpt.com/g/g-67e4ab141e4c81919e6635141b4fcf4d-digital-taibifer) (TaiBIF開發，架構在ChatGPT上專注於生物多樣性開放資料的AI問答小幫手)，得到的答案整理如下：

Late/Early/Middle加上地層單元名稱很常使用，但不是正式單元名稱，原始資料的"晚中新世"可以用以下表示：

| terms | value |
|-------|-------|
| earliestAgeOrLowestStage | Tortonian |
| latestAgeOrHighestStage | Messinian |

我的理解：晚中新世下有6個Age/Stage，剛好3等分(Early/Late/Middle)後，最"Late"的2個是Tortonian跟Messinian => 但身為一個中間處理資料的角色，我認為不應該任意解讀原始資料的轉換，所以決定保留"Miocene"，再想辦法處理"Late"這個字。因為早/中/晚一個想要表達精準一點但又沒那麼精準的定義，而且也不是每個單元名稱都可以3等分，有些除了早/中/晚也會用地層的上/下表示，這些用詞雖然模糊，但也保留了彈性，應該要被留下來，所以控制詞彙用以下處理：

| terms | value |
|-------|-------|
| earliestEpochOrLowestSeries | Miocene |
| latestEpochOrHighestSeries | Miocene |

ChatGPT叫我把"Late"放到geologicalContextRemarks或verbatimGeologicalContext紀錄裡，聽起來很合理，但目前(2026-03-06)標準並沒有這個詞彙（ChatGPT腦補?）
我目前也還沒想到放那邊比較適合，另外可以放的地方是occurrenceRemarks跟dynamicProperties (萬用自訂欄位)，但也怕會埋藏在雜亂的訊息中不容易被連結到，我先放個 `TODO` 在這邊，等那天智慧比較開了再來補充。

## 另外有一個看起來有關聯的 Chronometric Age Extension

[Chronometric Age Extension](https://chrono.tdwg.org/)，如果資料有特別做"年代測定"的方法、可以用精準一點的數值表示的話，就適合加上這個Extension。這邊我就懶的整理直接照抄AI給的答案跟TDWG網頁的內容。

The Chronometric Age (Chrono) terms are specifically for numerical ages (e.g., 10,000 years BP) or the specific process of dating a material. A "Formation" is a spatial/geological unit, which Darwin Core handles in its GeologicalContext class.

| terms | value |
|-------|---------------|
| chrono:verbatimChronometricAge | 76.6 to 74.5 Ma |
| chrono:chronometricAgeProtocol | Ar-Ar dating of volcanic ash |

比較跟DarwinCore的Geological Context不同

| Feature | Geological Context (DwC) | Chronometric Age (Chrono) |
|---------|--------------------------|--------------------------|
| Data Type | Descriptive names (Strings) | Numbers & Units (Floats/Integers) |
| Primary Goal | Classification | Measurement |
| Example Value | "Late Miocene" | "11.6 to 5.33 Ma" |
| Method Info | Usually implied or omitted | Explicit (e.g., "AMS Radiocarbon") |
| Uncertainty | Hard to quantify (Vague) | Precise (e.g., "± 50 years") |
| Best For | General collection management | Research-grade geochronology |


Use Chrono only if you start adding columns like "Date Method," "Lab Number," or "Error Margin."

## 另外還有一個頭更大的學名處理

除了舊學名這個老問題，古生物還會遇到：已滅絕物種、分類不確定等、這個我暫時也是先放個 `TODO` 在這邊，有空再回來。

## 參考資料

- [Darwin Core List of Terms - Darwin Core](https://dwc.tdwg.org/list/)
- [Chronometric Age Vocabulary List of Terms - Chronometric Age Extension](https://chrono.tdwg.org/list/)
- [International Commission on Stratigraphy](https://stratigraphy.org/)
- [年代地層學 - 維基百科，自由的百科全書](https://zh.wikipedia.org/zh-tw/年代地层学)
- [地層學-古生物學入門（三）](https://vocus.cc/article/64e4e1d7fd8978000151fe67)

### 地層名稱相關

- [台灣的化石地層簡介](https://taiwanaster2003.pixnet.net/blog/posts/9267191909)
- [地質雲加值應用平臺 - 首頁](https://www.geologycloud.tw/geohome) 有提供API，swagger文件
- [二十五萬分之一臺灣區域地質圖數值檔-臺灣 ｜ 政府資料開放平臺](https://data.gov.tw/dataset/6695) 提供csv，但裡面是WMS的URL清單
- [地質資料整合查詢](https://geomap.gsmma.gov.tw) 線上圖台做的不錯，但可能沒有下載資料的地方 (也許是我不會用)
