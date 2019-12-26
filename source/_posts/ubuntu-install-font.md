---
title: ubuntu-install-font
catalog: true
date: 2019-12-26 23:54:30
subtitle:
header-img:
tags:
- Linux
- Font
---
>How to install font on ubuntu.<br>
* If your arcgis server print gp can't print Chinese, after installed font. it will be resolved.
----------------------

##  Get font file.
>Usually we can find it on windows C:/Windows/Fonts

##  Saved font.
> save font file on you ubuntu this dir.(if don't have, please create it)
```bash
/usr/share/font/FromWindows
```

##  Change File Property.
```bash
chmod 755 ./*
```

## Run flow command.

```bash
mkfontscale
# if command not exist. use "apt install xfonts-utils" install it.
# if don't have "apt", use "yum install mkfontscale" install it.

# then run flow.
mkfontdir
```

## Run flow command.

```bash
fc-cache -fv
# if command not found, use "apt install fontconfig",
# if "apt" is not exists. use "yum install fontconfig".
```

## Check installed font.

```bash
fc-list
## check Chinese font.
fc-list :lang=zh
```







