---
title: Shell生成随机数
date: 2017-08-14 18:31:24
tags:
---

由C语言做随机数想到的，shell里做随机数的方式怎么办？

1 用$RANDOM环境变量
这个是bash自带的, 简单
```
[root@localhost tmp]# echo $RANDOM
15485
[root@localhost tmp]# echo $RANDOM
28251
```

2 用date命令
date +%s是到Epoch的秒数，同一秒内执行就出现重复值了，差了点意思
date +%N是纳秒，这个可以。

```
[root@localhost tmp]# for((i=0;i<10;i++)) do date +%s;done
1563268127
1563268127
1563268127
1563268127
1563268127
1563268127
1563268127
1563268127
1563268127
1563268127
[root@localhost tmp]# for((i=0;i<10;i++)) do date +%N;done
073253018
074129069
075084042
076401830
077565403
078989196
080028619
080871567
081590442
082393070
```


3 用系统目录下提供的random
也就是/dev/urandom，是持续输出，所以用head -n限制一下。还要配合cksum才能生成数字。

```
[root@localhost tmp]# head -n 1 /dev/urandom
▒8▒d{▒▒uzlx▒▒▒
[root@localhost tmp]# head -n 1 /dev/urandom | cksum
3580720445 276
[root@localhost tmp]# head -n 1 /dev/urandom | cksum | awk '{print $1}'
1937857628

```

4 用系统提供的uuid
这个和上一种情况类似，目录的位置是/proc/sys/kernel/random/uuid, 每次产生一条所以可以用cat

```
[root@localhost tmp]# cat /proc/sys/kernel/random/uuid
9b40c368-000c-4fd8-b2a3-1205e965222b
[root@localhost tmp]# cat /proc/sys/kernel/random/uuid | cksum
3272696125 37
[root@localhost tmp]# cat /proc/sys/kernel/random/uuid | cksum | cut -f1
1621319366 37
[root@localhost tmp]# cat /proc/sys/kernel/random/uuid | cksum | cut -d' ' -f1
1174319192
```


