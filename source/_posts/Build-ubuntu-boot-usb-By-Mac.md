---
title: Build ubuntu boot usb By Mac
catalog: true
date: 2020-02-20 22:47:42
subtitle:
header-img:
tags:
- Mac
catagories:
- Mac
---

> How to build a ubuntu boot use by your mac os.

# convert.
* convert *.iso file to mac use file.

```bash
hdiutil convert -format UDRW -o ubuntu-18.04.iso ubuntu-18.04.4-desktop-amd64.iso
```

* after that. on your dir ,you will find a file called "ubuntu-18.04.iso.dmg",

* then rename it.

```bash
mv ubuntu-18.04.iso.dmg ubuntu-18.04.iso
```

# unmount your usb device

* first use this command ,fand your usb device.

```bash
diskutil list
```

* you will find your disk path. like "/dev/disk3"

* unmount it.
```bash
diskutil unmountDisk /dev/disk3
```

# dd 
* build boot usb!

```bash
sudo dd if=./ubuntu-18.04.iso of=/dev/disk3 bs=1m
```

* it will for several minutes

* after command success.  means built success !