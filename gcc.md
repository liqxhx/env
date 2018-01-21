# gcc
gcc（GNU C Compiler）编译器的作者是Richard Stallman，也是GNU项目的奠基者  
GNU Compiler Collection  

gcc常用选项  


| 选项名      | 作用    |
| --------   | :-----: |
| -o      |产生目标（.i、.s、.o、可执行文件等）     |
| -Idir      |将dir目录加入搜索头文件的目录路径     |
| -Ldir      |将dir目录加入搜索库的目录路径     |
| -Ilib      |链接lib库，-Im表示要链接libm.so或者libm.a库文件    |
| -Wall      |使gcc对源文件的代码有问题的地方发出警告    |
| -g         |在目标文件中嵌入调试信息，以便gdb之类的调试程序调试     |
| -E      | 只运行C预编译器     |
| -S      | 告诉编译器产生汇编语言文件后停止编译，产生的汇编语言文件扩展名为.s     |
| -c      | 通知gcc取消链接步骤，即编译源码并在最后生成目标文件     |



## 预处理 Pre-Processing
```
gcc -E hello.c -o hello.i
```
## 编译 Compiling
```
gcc -S hello.i -o hello.s
```
## 汇编 Assembling
```
gcc -c hello.s -i hello.o
```

##`链接 Linking
```
gcc hello.o -o hello
```


## 生成静态库
```
gcc -Wall hello_fn.c -o hello_fn.o
ar rcs libhello.a hello_fn.o #ar是gnu归档工具，rcs表示(replace and create)
gcc -Wall main.c libhello.a -o main
gcc -Wall -L. main.c -o main -lhello
```


## 生成共享库
```
shared: 表示生成共享库格式
fPIC：产生位置无关码(position independent code)
库名规则：libxxx.so
示例：gcc -shared -fPIC hello.o –o libhello.so
使用共享库
编译选项
I：链接共享库，只要库名即可(去掉lib以及版本号)
L：链接库所在的路径.
示例：
gcc main.o -o main –L. -Ihello
```

## 其它
```
头文件及库文件位置
/usr/include及其子目录底下的include文件夹
/usr/local/include及其子目录底下的include文件夹 
/usr/lib
/usr/local/lib 

库的搜索路径
C_INCLUDE_PATH、LIBRARY_PATH
从左到右搜索-I -L指定的目录
由环境变量指定的目录
由系统指定的目录

运行共享库
1、拷贝.so文件到系统共享库路径下,一般指/usr/lib
2、更改LD_LIBRARY_PATH
3、ldconfig
　配置ld.so.conf，ldconfig更新ld.so.cache
```
