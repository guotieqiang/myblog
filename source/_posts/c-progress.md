---
title: 用C语言模拟进度条
date: 2017-09-31 12:56:36
tags:
---
用C语言搞了一个模拟的进度条
首先要知道三个事情:
* 1 \r 和 \n 的不同

\r 是输出位置回到行首
\n 是输出位置到下一行

* 2 printf() 默认是行输出，所以没有完成一行的时候想要保证输出，每次要fflush(stdout);
 
* 3 在不断打印进度条的过程中，为了屏蔽来自键盘输入回车，用了[tcsetattr](https://blog.csdn.net/liuqz2009/article/details/51967763)。通过这个可以改键。




```

int progress()
{
    int i = 0;
    char progress[40] = "----------------------------------------";
    struct termios term_orig;
    struct termios term_modify;

    //shield return from keybord when printing
    tcgetattr(STDIN_FILENO, &term_orig);
    term_modify = term_orig;
    term_modify.c_iflag = term_orig.c_iflag | IGNCR;
    tcsetattr(STDIN_FILENO, TCSAFLUSH, &term_modify);

    for(i = 1; i <= 100; i++){
        write(STDOUT_FILENO, "\r", 1);
        write(STDOUT_FILENO, progress, i / 10 * 4);
        printf(" %%%d", i);
        fflush(stdout);
        usleep(50000);
    }

    printf("\n");
    tcsetattr(STDIN_FILENO, TCSAFLUSH, &term_orig);

    return 0;
}
```
