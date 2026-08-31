---
date: 2025-04-17
categories:
  - Tech
tags:
  - debian
  - linux
---

# 我的Debian GNU/Linux 桌面環境

2026 改用Debian 13 (2026.08.10 updated)


Debian 12 (2025.04.16)

```text
安裝Debian 12、Gnome Desktop，語系選繁體中文。
新的Gnome不習慣，login的畫面點齒輪改成Gnome Classic

```

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
sudo apt install vim emacs git zsh tmux curl wget tig sqlite3
sudo apt install build-essential libreadline-dev libsqlite3-dev zlib1g-dev libssl-dev liblzma-dev libbz2-dev tk-dev libffi-dev llvm libncurses5-dev libncursesw5-dev liblzma-dev net-tools
```

Set default SHELL to zsh

```bash
chsh -s /bin/zsh
```
logout/login 後才有作用

**Docker**

[https://docs.docker.com/engine/install/debian/](https://docs.docker.com/engine/install/debian/)

## 好用軟體

### Command line

- lazydocker, lazygit
- fastfetch (好用好看的system info)
- Alacritty (好用的 Terminal Emulator，標榜OpenGL、速度快)

### Desktop Tools

**flameshot (snapshot)**

```bash
apt install flameshot
```

安裝完設定 Keyboard -> Custom Shortcuts

command: `sh -c "XDG_CURRENT_DESKTOP=sway flameshot gui"`

Debian 13如果只用 `flameshot gui` 會錯

**Heptabase**

```bash
#!/usr/bin/env bash
# Launch Heptabase.
# --gtk-version=3: force GTK 3 (GTK 4 crashes on this setup)
# (FUSE via libfuse2t64 handles AppImage mounting — no extract-and-run needed)
appimage=$(ls ~/d/bin/Heptabase-*.AppImage 2>/dev/null | sort -V | tail -1)
if [[ -z "$appimage" ]]; then
    echo "No Heptabase AppImage found in ~/d/bin/" >&2
    exit 1
fi
exec "$appimage" \
    --no-sandbox \
    --gtk-version=3 \
    "$@"
```

### Browser

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
