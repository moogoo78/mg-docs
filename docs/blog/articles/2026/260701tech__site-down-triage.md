---
date: 2026-07-01
title: 檢查網站流程
categories:
  - Tech
tags:
  - debugging
  - ops
  - cloudfront
---

# 檢查網站流程

!!! note "說明"
    這篇文章由 Claude（Anthropic 的 Claude Code）協助撰寫。

有人回報 `example.com` 連不上。與其直接 ssh 進 server 亂翻，先從外面照順序探一遍，
在**第一個失敗的那一層**停下來——那就是兇手。

<!-- more -->

## 從自己電腦上探（不是在 server 上）

```bash
# 1. DNS —— 有沒有解析？解到哪？
getent hosts example.com

# 2. TLS —— 憑證有效、沒過期？
echo | openssl s_client -servername example.com -connect example.com:443 2>/dev/null \
  | openssl x509 -noout -dates -subject

# 3. 完整請求 —— 狀態碼，還有「卡在哪」
curl -4 -sSI --max-time 25 -w "\nconnect=%{time_connect}s total=%{time_total}s\n" https://example.com

# 4. 對照組 —— 證明是網站的問題，不是你自己的網路
curl -4 -sSI --max-time 15 https://www.google.com | head -1
```

## 怎麼判讀

- DNS 失敗 → 網域 / DNS 設定問題。到此為止。
- TLS 失敗或過期 → 憑證問題。到此為止。
- `connect` 成功但 `total` 一路卡到 timeout、**0 bytes** → origin 卡住（app / DB / worker pool）。
- 很快回 `502 / 504` → origin 直接掛了，不是卡住。
- 連對照組（google.com）也失敗 → 問題在**你自己的網路**，不是網站。

第 4 步一定要跑。有一半的「網站掛了」其實是回報者自己的網路。先證明不是，再去動 production。

## 這次的實況

- **DNS** 正常，解到 CloudFront（`dxxxxxxxxxxxxx.cloudfront.net`）。
- **TLS** 正常，`CN=*.example.com` 憑證有效到 2026-10。
- **TCP connect** 27ms 就通了。
- **HTTP 回應** 卻完全沒東西——0 bytes 卡 25 秒才 timeout，連 CloudFront 的錯誤頁都沒有。
- 對照組 google.com、cloudflare.com 都秒回 200。

結論：**origin 卡住（origin hang），不是 origin 掛掉。**
CloudFront edge 活著（TLS 收得下來），但它把請求送到後面的 origin（Flask app）後，
origin 收了連線卻永遠不回，CloudFront 就一直等在 origin timeout 上。

如果 origin 是整個掛掉，CloudFront 會很快丟 502/504。**「卡住」代表 app 還在、但塞死了**——
最常見是 worker pool 被吃光、DB connection pool 耗盡，或某個沒設 timeout 的外部呼叫卡住。

## 確認 origin 是兇手後，上 server 查

```bash
docker compose ps                     # app process 還在嗎？
docker compose logs --tail=200 flask  # crash？WORKER TIMEOUT？traceback？
docker stats --no-stream              # CPU / 記憶體爆了？OOM？
df -h                                 # 硬碟滿了會讓一切卡死
```

最快的分流法：重啟 app container。一重啟就好 → 是資源 / pool 耗盡，不是程式碼 crash，
接著去抓那個洩漏的地方（通常是某條 request path 上沒設 timeout 的 DB / HTTP / subprocess 呼叫）。
