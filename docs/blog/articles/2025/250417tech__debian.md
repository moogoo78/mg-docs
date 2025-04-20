---
date: 2025-04-17
categories:
  - Tech
tags:
  - debian
  - linux
---

# 我的Debian GNU/Linux 桌面環境

安裝Debian 12、Gnome Desktop，語系選繁體中文。

新的Gnome不習慣，login的畫面點齒輪改成Gnome Classic

(2025.04.16)

## 基本桌面設定

- 輸入法: 英文Dvorak、中文新酷音 (設定直接改就好了)
- 終端機: 換一下配色

### Add sudoer

edit `/etc/sudoers`

> %your-username     ALL=(ALL:ALL) ALL

logout/login 後才有作用

### Caps Lock改成 Ctrl

修改 `/etc/default/keyboard`

> XKBOPTIONS="ctrl:nocaps"


## Install Packages/Tools

Update packages

```bash
sudo apt update
sudo apt upgrade
```

## Development

```bash
sudo apt install vim emacs git zsh tmux curl sqlite3
```

Set default SHELL to zsh

```bash
chsh -s /bin/zsh
```
logout/login 後才有作用

**Docker**

[https://docs.docker.com/engine/install/debian/](https://docs.docker.com/engine/install/debian/)

## 好用軟體

**Floorp**

到 https://ppa.floorp.app/ 按步驟安裝


**Chromium**

```bash
apt get install chrome
```

**Zotero**

[Installation Instructions](https://www.zotero.org/support/installation)

Debian 12會報錯，要 install `libdbus-glib-1-2`

## 系統管理

- bmon: command-line 看網路狀況 
