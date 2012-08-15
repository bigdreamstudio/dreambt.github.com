---
layout: post
category : Linux
tags : [intro, beginner, Linux, tutorial, c, gdb]
title: linux下用gdb调试c程序的总结
wordpress_id: 173
wordpress_url: http://www.im47cn.nt102.idcwind.net/wordpress/?p=173
date: 2009-08-10 22:33:00.000000000 +08:00
---
目录
<ol>
	<li>单步执行和跟踪函数调用</li>
	<li>断点</li>
	<li>观察点</li>
	<li>段错误</li>
</ol>
1. 单步执行和跟踪函数调用

看下面的程序:

#include &lt;stdio.h&gt;

int add_range(int low, int high)

{

int i, sum;

for (i = low; i &lt;= high; i++)

sum = sum + i;

return sum;

}

int main(void)

{

int result[100];

result[0] = add_range(1, 10);

result[1] = add_range(1, 100);

printf("result[0]=%d\nresult[1]=%d\n", result[0], result[1]);

return 0;

}

$ gcc -g main.c -o main

$ gdb main

-g的作用是在目标文件中加入源代码的信息,比如目标文件中第几条机器指令对应源代码的第几行,但并不是把整个源文件嵌入到目标文件中,所以在调试时目标文件时必须保证gdb也能找到源文件。

* list(或l) 行号 列出从第几行开始的源代码

* list(或l) 函数名 列出某个函数的源代码

* start命令  开始执行程序

* run(简写为r)  从头开始连续而非单步执行程序

* next命令(简写为n) 控制这些语句一条一条地执行

* step命令(简写为s) 进入函数中去执行

* continue命令(简写为c)连续运行而非单步运行,程序到达断点会自动停下来

* finish命令  执行到当前函数返回,然后停下来等待命令

* backtrace命令(简写为bt)可以查看函数调用的栈帧（查看各级函数调用及参数）

* frame命令(简写为f) 选择栈帧

* info命令(简写为i) i breakpoints 查看已经设置的断点

i 变量名        查看局部变量的值

i locals  查看当前栈帧局部变量的值

* print命令(简写为p) 打出变量的值

例如：

(gdb) bt  //查看函数调用的栈帧

#0  add_range (low=1, high=10) at gdb1.c:6

#1  0x0000000000400516 in main () at gdb1.c:13

(gdb) f 1  //选择1号栈帧然后查看局部变量

#1  0x0000000000400516 in main () at gdb1.c:13

13  result[0] = add_range(1, 10);

(gdb) i locals //查看局部变量

result = {-7984, 32767, 291519488, 62, 0, 0, -134231144, 0}

* set命令  修改变量的值

* print命令  也可修改变量的值,因为print命令后面跟的是表达式,而我们知道赋值和函数调用也都是表达式,所以还可以用print来修改变量的值,或者调用函数。例如：

(gdb) p result[2]=33

$5 = 33

(gdb) p printf("result[2]=%d\n", result[2])

result[2]=33

$6 = 13  //printf的返回值表示实际打印的字符数,所以$6的结果是13。

2. 断点

断点调试实例：输入123回车，输入123456回车，观察结果

#include &lt;stdio.h&gt;

int main(void)

{

int sum = 0, i = 0;

char input[5];

while (1) {

scanf("%s", input);

for (i = 0; input[i] != '\0'; i++)

sum = sum * 10 + input[i] - '0';

printf("input=%d\n", sum);

}

return 0;

}

如果变量要赋初值,start不会跳过变量定义语句。因此sum被列为重点怀疑对象,我们可以用display命令使得每次停下来的时候都显示当前sum值，undisplay可以取消对先前设置的那些变量的跟踪。

* break命令  表示在某一个函数开头设断点。参数可以使行号，也可以是函数名。

//可以带条件 break 9 if sum != 0

* delete breakpoints (n)命令  表示删除某个或者全部断点

* disable breakpoints (n)命令 表示禁用某个或者全部断点

* enable breakpoints (n)命令  表示启用某个或者全部断点

* display命令(简写为d) display i 使得每次停下来的时候都显示当前i值

* undisplay  取消对先前设置的那些变量的跟踪

3. 观察点

观察点调试实例：输入123回车，输入123456789回车，输入12345回车

#include &lt;stdio.h&gt;

int main(void)

{

int sum = 0, i = 0;

char input[5];

while (1) {

sum = 0;

scanf("%s", input);

for (i = 0; input[i] != '\0'; i++)

sum = sum * 10 + input[i] - '0';

printf("input=%d\n", sum);

}

return 0;

}

* info命令(简写为i) i breakpoints 查看已经设置的断点

i 变量名        查看局部变量的值

i locals  查看当前栈帧局部变量的值

* watch  设置观察点

* info watchpoints 查看当前设置了哪些观察点

* x 从某个位置开始打印存储器的一段内容,全部当成字节来看,而不区分哪些字节属于哪些变量

x/7b input    //7b是打印格式,b表示每个字节一组,7表示打印7组

删除原来设的断点,从头执行程序,重复上次的输入,用watch命令设置观察点

(gdb) delete breakpoints

Delete all breakpoints? (y or n) y

(gdb) start

Breakpoint 1 at 0x80483b5: file main.c, line 5.

Starting program: /home/akaedu/main

main () at main.c:5

5               int sum = 0, i = 0;

(gdb) n

9                       sum = 0;

(gdb) (直接回车)

10                      scanf("%s", input);

(gdb) (直接回车)

12345

11                      for (i = 0; input[i] != '\0';i++)

(gdb) watch input[5]

Hardware watchpoint 2: input[5]

(gdb) i watchpoints

Num     Type           Disp Enb Address     What

2       hw watchpoint keep y                input[5]

(gdb) c

Continuing.

Hardware watchpoint 2: input[5]

Old value = 0 '\0'

New value = 1 '\001'

0x0804840c in main () at main.c:11

11                      for (i = 0; input[i] != '\0';i++)

(gdb) c

Continuing.

Hardware watchpoint 2: input[5]

Old value = 1 '\001'

New value = 2 '\002'

0x0804840c in main () at main.c:11

11                      for (i = 0; input[i] != '\0';i++)

(gdb) c

Continuing.

Hardware watchpoint 2: input[5]

Old value = 2 '\002'

New value = 3 '\003'

0x0804840c in main () at main.c:11

11                      for (i = 0; input[i] != '\0';i++)

如果输入的不是数字而是字母或别的符号也能算出结果来,这显然是不对的,可以在循环中加上判断条件:

while (1) {

sum = 0;

scanf("%s", input);

for (i = 0; input[i] != '\0'; i++) {

if (input[i] &lt; '0' || input[i] &gt; '9') {

printf("Invalid input!\n");

sum = -1;

break;

}

sum = sum*10 + input[i] - '0';

}

printf("input=%d\n", sum);

}

4. 段错误

段错误调试实例一

#include &lt;stdio.h&gt;

int main(void)

{

int man = 0;

scanf("%d", man);

return 0;

}

调试过程如下:

$ gdb main

......

(gdb) r

Starting program: /home/akaedu/main

123

Program received signal SIGSEGV, Segmentation fault.

0xb7e1404b in _IO_vfscanf () from /lib/tls/i686/cmov/libc.so.6

(gdb) bt

#0 0xb7e1404b in _IO_vfscanf () from /lib/tls/i686/cmov/libc.so.6

#1 0xb7e1dd2b in scanf () from /lib/tls/i686/cmov/libc.so.6

#2 0x0804839f in main () at main.c:6

在gdb中运行,遇到段错误会自动停下来,这时可以用命令查看当前执行到哪一行代码了。gdb显示段错误出现在_IO_vfscanf函数中,用bt命令可以看到这个函数是被我们的scanf函数调用的,所以是scanf这一行代码引发的段错误。仔细观察程序发现是man前面少了个&amp;。

上一节我们调试了一个输入字符串转整数的程序,最后提出修正Bug的方法是在循环中加上判断条件,如果不是数字就报错退出,不仅输入字母可以报错退出,输入超长的字符串也会报错退出。然而真的把两个Bug一起解决了吗?这其实是一种治标不治本的办法,因为并没有制止scanf的访问越界。我说过了,使用scanf函数是非常凶险的。表面上看这个程序无论怎么运行都不出错了,但假如我们把while (1) 循环去掉,每次执行程序只转换一个数:

段错误调试实例二

#include &lt;stdio.h&gt;

int main(void)

{

int sum = 0, i = 0;

char input[5];

scanf("%s", input);

for (i = 0; input[i] != '\0'; i++) {

if (input[i] &lt; '0' || input[i] &gt; '9') {

printf("Invalid input!\n");

sum = -1;

break;

}

sum = sum*10 + input[i] - '0';

}

printf("input=%d\n", sum);

return 0;

}

然后输入一个超长的字符串,看看会发生什么:

$ ./main

1234567890

Invalid input!

input=-1

看起来正常。再来一次,这次输个更长的:

$ ./main

1234567890abcdef

Invalid input!

input=-1

Segmentation fault

又段错误了。我们按同样的方法用gdb调试看看:

$ gdb main

......

(gdb) r

Starting program: /home/akaedu/main

1234567890abcdef

Invalid input!

input=-1

Program received signal SIGSEGV, Segmentation fault.

0x0804848e in main () at main.c:19

19      }

(gdb) l

14                     }

15                     sum = sum*10 + input[i] - '0';

16             }

17             printf("input=%d\n", sum);

18             return 0;

19      }

gdb指出,段错误发生在第19行。可是这一行什么都没有啊,只有表示main函数结束的}括号。这可以算是一条规律,如果某个函数中发生访问越界,很可能并不立即产生段错误,而在函数返回时却产生段错误。

想要写出Bug-free的程序是非常不容易的,即使scanf读入字符串这么一个简单的函数调用都会隐藏着各种各样的错误,有些错误现象是我们暂时没法解释的:为什么变量i的存储单元紧跟在input数组后面?为什么同样是访问越界,有时出段错误有时不出段错误?为什么访问越界的段错误在函数返回时才出现?还有最基本的问题,为什么scanf输入整型变量就必须要加&amp;,否则就出段错误,而输入字符串就不要加&amp;?
