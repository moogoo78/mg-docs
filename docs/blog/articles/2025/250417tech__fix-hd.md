---
date: 2025-04-17
categories:
  - Tech
tags:
  - ssd
  - linux
  - hd
---

# 硬碟壞掉處理

我的Intel Nuc小電腦的電源不知道什麼原因按了都沒反應，本來以為是電源線接觸不良或變壓器壞掉，送檢測結果是硬碟壞掉，但是不知道為什麼連開機BIOS也沒出現。

總之就把硬碟拆下來接外接盒，其他電腦也是讀不到。

`fdisk` 有看到sdb device，想要手動mount，出現以下錯誤。

```bash
sudo mount -t ext4 /dev/sdX1 /mnt/mydisk
```

> mount: /mnt/mydisk: wrong fs type, bad option, bad superblock on /dev/sdb1, ...


以下靠Chat-GPT亂試，至少把資料救回來了。

---

用 `lsblk`看也是有device，但是沒有partition。

```bash
lsblk -f
```

用`fsck`檢查 (忘記這個可以幹嘛)

```bash
sudo fsck -b 32768 /dev/sdb1
```

看仔細

```bash
lsblk -o NAME,SIZE,FSTYPE,TYPE,MOUNTPOINT
```

會有以下幾種狀況

| Situation                                    | Meaning                      | Action                           |
| :------------------------------------------- | :--------------------------- | :------------------------------- |
| Blank disk                                   | Partition and format         | Partition and format             |
| Corrupted disk                               | Broken partition table or FS | Try recovery                     |
| Filesystem directly on disk but not detected | Rare, but possible           | Try force mounting or inspecting |


## 重新請Chat-GPT統整

### Step 1: Identify the Device

```bash
sudo file -s /dev/sdX
```

###  Step 2: Check Filesystem Signature

```bash
sudo file -s /dev/sdb
```

### Step 3: Recovery or Format?

=> Recover data

先安裝`testdisk`這個神奇工具

```bash
sudo apt install testdisk
```

進入文字互動介面

```bash
sudo testdisk
```

#### testdisk 操作

- [Create] a new log file
- Select /dev/sdb
- Choose partition table type (Intel = MBR, or EFI GPT if it's a newer disk)
- [Analyse] to find lost partitions
- [Quick Search] or [Deeper Search] if needed
- When partitions are found, press P to list files inside
- If files are there, you can:
  - Copy files (press C)
  - Or [Write] to restore the partition table

`reboot` 重開機，用`lsblk`看，partition回來了。testdisk太神了!!!

開機磁區還是壞的，開機抓不到硬碟，但是可以用外接盒讀回其他資料，至少資料有救回來。
