---
title: SHELL字符分割的IFS变量
date: 2017-11-30 06:48:32
tags:
---

shell命令解析输入字符串的默认分割符是水平制表符（tab），空格（space)和换行（newline）
要改变当前shell的默认分割符，用IFS的环境变量。

* 默认的分割符
```
[root@centos01 tmp]# echo -n "$IFS" | hexdump
0000000 0920 000a
0000003
```
十六进制值0x20, 0x09和0x0a分别对应于空格(space), 水平制表符(tab)和换行符(newline)的值。


* 下面两个方法的运行结果直接说明问题：

```
#!/bin/sh

#default IFS are tab,space,and newline

not_change_IFS() {

echo "===========not change IFS=================="
for i in `df -h | head -n 2`;
do
    echo $i
done

}

change_IFS() {

echo "===========change IFS=================="

#only use \n as delimeter
oldIFS=$IFS
IFS=$'\n'
for i in `df -h | head -n 2`;
do
    echo $i
done
IFS=$oldIFS

}

not_change_IFS
change_IFS
```

运行结果
```
[root@centos01 tmp]# ./test.sh
===========not change IFS==================
Filesystem
Size
Used
Avail
Use%
Mounted
on
/dev/mapper/rhel-root
6.7G
5.7G
1.1G
85%
/
===========change IFS==================
Filesystem             Size  Used Avail Use% Mounted on
/dev/mapper/rhel-root  6.7G  5.7G  1.1G  85% /

```

