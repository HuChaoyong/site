---
title: Vi
catalog: true
date: 2019-12-07 13:01:28
subtitle: "how to use vi"
header-img:
tags:
- Linux
- Vi
---

# 说明.

* 以下快捷操作，均是在　'命令'　模式下.

* 移动行或者光标前后移动
```
    > h  : 向前移动一位.
    > j  : 向下移动一行
    > k  : 向上移动一行
    > l  : 向右移动一位.
    > 4l : 往右移动4位.
```
* 行首或行为.
```
    > 0  : 移动到行首
    > $  : 移动到行尾
```
* 一段移动.
```
    > b  : 往前移动一段(类似于 ctrl + \<=)
    > w  : 往后移动一段.
```
* 文件行数定位.
```
    > G  : 移到最后一行.
    > 11G : 移动到11行..
    > 1G : 移动到文件首行.
```
* 单个替换.
```
    > rr: 用'r'替换掉光标位置的文本.
    > rA: 用'A'替换掉光标位置的文本.
```
* 替换模式.
```
    >　R:　　替换模式下，所有的输入都会覆盖掉当前光标位置的文本.
```
* 删除.
```
    > x:   删除当前位置的文本.
```
* 粘贴.
```
    > p : 在当前位置后，粘贴内容.　如果是多行，那么将会从这行后开始粘贴.
    > P : 在当前位置前，粘贴内容.　如果是多行，那么将会从这行后开始粘贴.
```
* 内容删除.
```
    > c0 : 删除行首到当前位置(删除部分，不包括光标位置的文本.)
    > c$ : 删除当前位置到行为的内容(删除部分，包括当前光标位置.)
    > cc : 清空当前行内容
    > D: 删除当前位置到行为的内容(删除部分，包括当前光标位置.完成后，光标会往前移动一位)
    > dG: 删除当前行以及以后的内容.

    > dw: 删除下一个段.
    > d2w: 删除下2段
    > db: 删除前一段.
    > d3b: 删除前3段.
    > dd : 删除当前行.
    > 2dd : 删除2行
```
* 其他.
```
    >~: 将光标位置的字母转换成大写.并往后移动一位.
    > u: 回滚操作. 按多次则回滚多次
```
# English

* line and single letter.
> h  : move left a letter.<br>
> j  : move next line<br>
> k  : move previouse line<br>
> l  : move right a letter.

> 0  : current line first position.<br>
> $  : current line last position.<br>

> b  : previous word<br>
> w  : next word

> 4l : move right 4 letters.


> G  : move to end line of the file.<br>
> 11G : move to line 11 .<br>

> rr: replace current position to 'r'<br>
> rA: replace current position to 'A'<br>

> R:  change to replace mode. now typed letter will replace current letter.

> x:   delete a letter which is on the cursor position<br>

> p : paste block after current line.<br>
> P : paste block content before current line<br>

> c0 : delete from first position to current position(not include current posotion , will not move cursor ,will convert to 'insert' mode)<br>
> c$ : delete from current position to end of line.(include current position, will not move cursor, will convert to 'insert' mode)<br> 
> cc : clear content of current line<br>

> ~: convert lowercase to uppercase. and after that move position to next position.<br>
> dw: delete next one word.<br>
> d2w: delete next two words.<br>
> db: delete previous one word.<br>
> d3b: delete previous three words.<br>
> dd : delete current line.<br>
> 2dd : delete current line and next line<br>

> D: delete from current position to end of line.and move cursor to previous position (not include current position)<br>

> u: rollback your last execution.(type many means rollback many times)<br>




