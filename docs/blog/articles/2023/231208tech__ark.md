---
title: create ARK noid 
date: 2023-12-08
categories:
  - Tech
tags:
  - unfinished
draft: true
---

[jkunze/docker-arknoid](https://github.com/jkunze/docker-arknoid/tree/main)

```bash title="build image
docker run -it -d --rm --name arknoid jakkbl/arknoid
```

```bash title="initialize with organization's 5-digit NAAN"
docker exec -it arknoid arknoid init 12345
```


```bash title="mint 1 ARK"
docker exec -it arknoid arknoid mint 1
```

default shoulder: h7
```bash
docker exec -it arknoid arknoid mint 36000 XY
```

```
docker exec -it arknoid arknoid mint 36000 > MyFirstARKs
```


depositar
redededk (7碼)
[操作手冊 — 研究資料寄存所 6.6.2 說明文件](https://docs.depositar.io/zh-tw/stable/user-guide.html?highlight=ark#ark)


jk
reedeedk (7碼, 編碼數:70,728,100)



