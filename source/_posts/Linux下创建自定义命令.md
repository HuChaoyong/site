---
title: Linux(Mac)下创建自定义命令
catalog: true
date: 2019-02-28 21:24:34
subtitle: create a custom command
header-img:
tags:
- Linux
- Shell
catagories:
- Linux
- Shell
---

# Ubuntu

* 基于 shell 创建一个自定义指令，在全局调用

1. 先创建 shell 脚本( 名为 ngc )
> 此处shell目的为，输入参数，创建一个名为参数的目录，并在目录下建立3个文件
```bash
#!/bin/bash
if [ ! -d "$1" ];
then
	mkdir "$1"
fi

touch "./"$1"/"$1".component.ts"
touch "./"$1"/"$1".component.css"
touch "./"$1"/"$1".component.html"
echo "create file success!"
```
> 创建shell以后，如果没有执行权限，或者不能执行，使用 chmod 为其添加执行权
```bash
chmod +x ngc
```

2. 编辑 ~/.bashr 文件， 将shell所在文件夹导出为 全局PATH
> 我的 shell 所在目录为  /home/root/.shell
```bash
# custom command dir
export PATH=$PATH:/home/root/.shell
```

3. 执行source 命令，
```bash
source ~/.bashrc
```

4. 测试，在任意目录下运行命令
```bash
ngc animal
```
> 在目录下创建了 animal目录，并在其下创建了3个文件， OK ！ Success！

# Mac
1. 创建shell 脚本(同上)

2. Ubuntu中导出路径是在 ~/.bashrc文件， mac中是 ~/.bash_profile (同上)

3. 测试 
```bash
source ~/.bash_profile
```

> Success!



