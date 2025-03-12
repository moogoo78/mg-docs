# Operation System / Desktop

## Linux

Open file manager of current path in command line

```bash
xdg-open .
```

### Debian

[如何知道目前Debian版本號碼](../blog/articles/2023/231214tech__how-to-check-debian-version.md)

#### Install

1. download [Debian](https://www.debian.org/distrib/) iso image file (default is amd64, compatible with Intel & AMD chip)
2. make a bootable USB drive (recommanded: [Rufus](https://rufus.ie/en/))

#### Desktop

install [Debian -- Details of package gnome-tweaks in bookworm](https://packages.debian.org/bookworm/gnome-tweaks), can change `Caps` to `Ctrl`

```bash title="install multiple desktop environment"
sudo tasksel
```

[Desktop Entry](https://specifications.freedesktop.org/desktop-entry-spec/latest/) (launcher, icon)

path: `~/.local/share/applications/{app_name}/{app_name}.desktop`

```ini
[Desktop Entry]
Name=Zotero
Exec=bash -c "$(dirname $(realpath $(echo %k | sed -e 's/^file:\\/\\///')))/zotero -url %U"
Icon=/opt/zotero/icons/icon128.png
Type=Application
Terminal=false
Categories=Office;
MimeType=text/plain;x-scheme-handler/zotero;application/x-research-info-systems;text/x-research-info-systems;text/ris;application/x-endnote-refer;application/x-inst-for-Scientific-info;application/mods+xml;application/rdf+xml;application/x-bibtex;text/x-bibtex;application/marc;application/vnd.citationstyles.style+xml
X-GNOME-SingleWindow=true
```

#### 輸入法

切換語系 `Super Key + Space` | 切換輸入法 `Ctrl + Shift`

[ChineseInputMethod - Debian Wiki](https://wiki.debian.org/ChineseInputMethod)

```bash title="設定輸入法"
im-config
```

```bash title="Install fcitx5 traditional chinese input"
apt install fcitx5 fcitx5-chinese-addons fcitx5-chewyin
```

reference:

- [Fcitx - 維基百科，自由的百科全書](https://zh.wikipedia.org/zh-tw/Fcitx)
- [小麥注音輸入法 on X: "我們更新了首頁 (https://t.co/7hY7IzhQJi)，修改了我們使用的社群網路平台，未來版本資訊也會從該處發布。歡迎舊雨新知前往，繼續給予指教！" / X](https://twitter.com/McBopomofo/status/1714799933969023391)


##### Development Environment

Install Python using Pyenv:

```bash
apt install build-essential \
libssl-dev zlib1g-dev libbz2-dev \
libreadline-dev libsqlite3-dev llvm libncurses5-dev libncursesw5-dev \
xz-utils tk-dev libffi-dev liblzma-dev python3-openssl
```

### CentOS

```bash title="list all supported version"
yum --showduplicate list {package name}
```

## Windows

### WSL

mount external hard drive
```bash
sudo mount -t drvfs D: /mnt/d
```

### Desktop

screenshot: `Windows+Shift+S`

[分享一個讓老舊 Windows 筆電執行速度更快的方法 | The Will Will Web](https://blog.miniasp.com/post/2024/07/02/Share-a-method-to-make-old-Windows-laptops-run-faster?fbclid=IwZXh0bgNhZW0CMTEAAR0Xk15d7T_bwY3ItQTGaqHMWhoUoJA-msgH5C4kZ-0tJLq4ZYCMJ19Y5y8_aem_0FAYhoheokA85RcvDx-k8g)

### Servers
修改local DNS

`C:\WINDOWS\system32\drivers\ect\hosts` 要有管理員權限

## File System

via: https://stackoverflow.com/a/466596/644070

|                                       | FAT32        | NTFS                     | EXT4          |
| ------------------------------------- | ------------ |--------------------------|---------------|
| Max. # of files               | 268,173,300  | 2^32 - 1 (4,294,967,295) | 2^32 - 1 (4,294,967,295) |
| Max. # of files per directory | 2^16 - 1 (65,535) |     | 10 million approximately (can be extended with large_dir feature)  |
| Max. file size                     | 2 GiB - 1 without LFS, 4 GiB - 1 with | - Implementation: 2^44 - 2^6 bytes (16 TiB - 64 KiB) </br>- Theoretical: 2^64 - 2^6 bytes (16 EiB - 64 KiB) | 2^44 - 1 bytes (16 TiB - 1) |
| Max. volume size            |        | - Implementation: 2^32 - 1 clusters (256 TiB - 64 KiB)</br>- Theoretical: 2^64 - 1 clusters (1 YiB - 64 KiB)  | 2^48 - 1 bytes (256 TiB - 1)  |

```bash title="list filesystem type"
lsblk -f
```
