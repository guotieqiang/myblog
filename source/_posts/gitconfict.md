---
title: "git 冲突的解决"
date: 2017-04-10 11:31:52
tags:
---
冲突的类型:
* 逻辑冲突
 这种比较坑，自动merge的时候没有问题，但是实际内容中有问题了。比如说函数的返回值含义变了，不是git中解决的。需要自己看好，看不好就等测试吧。anyway不在这篇的讨论范围，好吧。

* 树冲突
 文件名修改造成的冲突，称为树冲突。
比如，a用户把文件改名为a.c，b用户把同一个文件改名为b.c，那么b将这两个commit合并时，会产生冲突。
```
$ git status
    added by us:    b.c
    both deleted:   origin-name.c
    added by them:  a.c
```
 如果最终确定用b.c，那么解决办法如下：
```
git rm a.c
git rm origin-name.c
git add b.c
git commit
```
 执行前面两个git rm时，会告警“file-name : needs merge”，可以不必理会。
树冲突也可以用git mergetool来解决，但整个解决过程是在交互式问答中完成的，用d 删除不要的文件，用c保留需要的文件。
最后执行git commit提交即可。

* 内容冲突
 merge的时候会显示CONFLICT
```
$git fetch origin
$git merge origin/wombat_master
Auto-merging src/allocator_server.c
Auto-merging src/arg_parse.c
Auto-merging src/cli.c
CONFLICT (content): Merge conflict in src/cli.c
Auto-merging src/include/allocator.h
CONFLICT (content): Merge conflict in src/include/allocator.h
Auto-merging src/include/arg_parse.h
Auto-merging src/spawn.c
Automatic merge failed; fix conflicts and then commit the result.
```
 编辑src/cli.c和src/include/allocator.h
 冲突产生后，文件系统中冲突了的文件（这里是test.txt）里面的内容会显示为类似下面这样：
```
a123
<<<<<<< HEAD
b789
=======
b45678910
>>>>>>> 6853e5ff961e684d3a6c02d4d06183b5ff330dcc
c
```
 其中：冲突标记<<<<<<< （7个<）与=======之间的内容是我的修改，=======与>>>>>>>之间的内容是别人的修改。

 编辑完，可以git status看一下， 
 然后git add -u **注：-u 表示把所有已track的文件的新的修改加入缓存，但不加入新的文件。**
 然后git commit 
[Git下的冲突解决](http://www.cnblogs.com/sinojelly/archive/2011/08/07/2130172.html)
