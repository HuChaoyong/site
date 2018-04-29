---
title: 常用Linux命令(小松鼠)
catalog: true
date: 2018-04-07 10:03:06
subtitle: "日常折腾Ubuntu"
header-img: "Ubuntu.png"
tags:
- Linux
- Ubuntu
catagories:
- Linux
---
> 日常使用Ubuntu中，使用频率较高的命令(基础得不能再基础了)
>

# rm
---
> rm => remove , delete 
> -r Remove directories and their contents recursively(删除本目录及其递归的子目录文件)
> -f Ignore nonexistent files, never prompt, means force (忽略不存在文件，不产生任何提示)
```bash
 # delete file 'abc.txt' on current directory
rm -rf abc.txt
 # delete directory 'fld'
rm -rf fld
# delete all file and directory which match 'mysql-*' by regular express
rm -rf mysql-*
```

# cp
---
> cp => copy
> -r => copy with director recursively(复制时，递归子目录)
> -f => force
```bash
# copy 'aaa.txt' on current directory and named 'bbb.txt.backup'
cp aaa.txt bbb.txt.backup
# copy 'bbb' director to the './a-aaa' directory
cp -rf bbb ./a-aaa
# copy 'bbb' director's all file to the './a-aaa' directory
cp -rf bbb/* ./a-aaa 
```

# mv
> mv => move / rename
> -i => if there are some same file, prompt (若指定目录已有同名文件，则先询问是否覆盖旧文件)
> -f => force

```bash
# rename 'aaa.txt' to 'bbb.txt'
mv aaa.txt bbb.txt 
# move 'aaa.txt' to 'bbb' directory
mv aaa.txt bbb

# if 'bbb' directory is exit, move 'aaa' directory to 'bbb' directory, else rename
# (若bbb目录存在，则将aaa目录放在bbb目录下，否则，重新命名aaa 为bbb)
mv aaa bbb
```

# md5

```bash
md5sum netease-cloud-music.deb
```
it will show md5 code

# install .deb

```bash
sudo dpkg -i netease-cloud-music.deb
```

```bash
# if there are some dependencies problem, repaired
sudo apt-get install -f
```
```bash
# uninstall
sudo apt-get purege netease-cloud-music
```

# boot u-disk make
```bash
# get u-disk path, like '/dev/sdc1'
sudo fdisk -l

# clean u-disk
sudo umount /dev/sdc1
sudo mkfs.vfat /dev/sdc1 -I

# use dd make it, if there is a 'ubuntu18.04.iso' file on current directory 
sudo dd if=ubuntu18.04.iso of=/dev/sdc1
# maybe waite for 10 minutes, it done
```