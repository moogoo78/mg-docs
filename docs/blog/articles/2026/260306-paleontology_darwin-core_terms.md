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

# 關於古生物的Darwin Core詞彙

跟現生生物不同的是basisOfRecord標本資料是用`FossilSpecimen`控制詞彙。


[Geological Context](https://dwc.tdwg.org/terms/#geologicalcontext)這個Class應該就定義了大多數情境用到的詞彙，但遇到"晚中新世" 這種資料要如何處理


Early/Middle/Late 常用
Early Miocene

Middle Jurassic

Late Cretaceous

Early Pleistocene


但在地層命名體系裡，不是正式單元名稱
國際地層委員會 ICS 

沒有 geologicalContextRemarks
沒有 verbatimGeologicalContext


時間階層（Chronostratigraphy）

| 層級 | Earliest 詞彙 | Latest 詞彙 |
|------|--------------|------------|
| Eon / Eonothem | `earliestEonOrLowestEonothem` | `latestEonOrHighestEonothem` |
| Era / Erathem | `earliestEraOrLowestErathem` | `latestEraOrHighestErathem` |
| Period / System | `earliestPeriodOrLowestSystem` | `latestPeriodOrHighestSystem` |
| Epoch / Series | `earliestEpochOrLowestSeries` | `latestEpochOrHighestSeries` |
| Age / Stage | `earliestAgeOrLowestStage` | `latestAgeOrHighestStage` |


地層單位（Lithostratigraphy）
formation
group
member
bed

[Chronometric Age Extension](https://chrono.tdwg.org/)

The Chronometric Age (Chrono) terms are specifically for numerical ages (e.g., 10,000 years BP) or the specific process of dating a material. A "Formation" is a spatial/geological unit, which Darwin Core handles in its GeologicalContext class.

Chronometric (Chrono)	chrono:verbatimChronometricAge	76.6 to 74.5 Ma
Chronometric (Chrono)	chrono:chronometricAgeProtocol	Ar-Ar dating of volcanic ash


| Feature | Geological Context (DwC) | Chronometric Age (Chrono) |
|---------|--------------------------|--------------------------|
| Data Type | Descriptive names (Strings) | Numbers & Units (Floats/Integers) |
| Primary Goal | Classification | Measurement |
| Example Value | "Late Miocene" | "11.6 to 5.33 Ma" |
| Method Info | Usually implied or omitted | Explicit (e.g., "AMS Radiocarbon") |
| Uncertainty | Hard to quantify (Vague) | Precise (e.g., "± 50 years") |
| Best For | General collection management | Research-grade geochronology |


Stick to Darwin Core (dwc:earliestEpoch) for your "晚中新世" column to ensure compatibility with most biodiversity portals (like GBIF).

Use Chrono only if you start adding columns like "Date Method," "Lab Number," or "Error Margin."


dynamicProperties ?
