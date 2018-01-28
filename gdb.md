## gdb
gdb是GNU debugger的缩写，是编程调试工具

## 基本使用
```
// 编译源文件
gcc -Wall -g xxx.c -o xxx 

// 用gdb来运行
gdb xxx 

list/l [行号/函数名] // 查看行号或函数处代码，不带参数是查看最近10行源码
list file:fun // 查看文件中函数，中间用冒号隔开

// 设置断点
break 行号/函数 在当前文件指定行号或函数处设置断点
break file:行号 // 在指定文件行号打断点
break file:fun // 在指定文件的函数处打断点
break if <condition> // 条件断点
如：
result = 0;
int i;
for(i = 0; i<100; i++)
{
  result += i;
}
break if i=50; // 在i等于50时停止
print/p i // 查看i的值

// 查看断点信息
info break // 查看所有断点

i b // 缩写

// 设置观察点
watch expr // 查看一个表达式，当表达式值有改变程序就会停下来

delete/d 断点号 // 删除断点

run/r [agr1 arg2] // 运行程序 参数

// 单步调试
step/s // 单步跟踪，进入函数，step in
continue/c // 继续执行，运行至下一个断点
next/n // 不进入函数的单步跟踪，step out
finish // 函数返回，直到当前函数完成返回
until // 当厌倦了在一个循环体内单步跟踪时，这个命令可以运行程序直到退出循环体




```

## 查看运行时错误

print/p - 查看变量值
ptype - 查看类型
ptype 变量名

print array - 查看数组
print *array@len - 查看动态内存

p array[1] 查看某个值
p &array[1] 查看地址


print x=5 - 改变运行时数据

## 段错误 Segmentation fault
```
段错误是由于访问非法地址而产生的错误。
访问系统数据区，尤其是往系统保护的内存地址写数据。最常见就是给一个指针以0地址 
内存越界(数组越界，变量类型不一致等) 访问到不属于你的内存区域

gdb bugging
r
0x08048304 in segfault () at bugging.c:7
bt // 


```

## core文件调试
```
在程序崩溃时，一般会生成一个文件叫core文件。core文件记录的是程序崩溃时的内存映像，并加入调试信息。core文件生成的过程叫做core dump

设置生成core文件
ulimit -c  查看core-dump状态
ulimit -c 数字 （如：ulimit -c 1024）
或ulimit -c unlimited 没有限制

db利用core文件调试
gdb 程序的文件名 core文件 // gdb bugging core.9351
bt // 查看堆栈
```
