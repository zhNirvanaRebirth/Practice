# 软件安装：源码与Tarball  
## 开放源码的软件安装与升级  
> Linux上的软件几乎都是经过GPL的授权，因此每个软件都提供了原始程序，并且我们可以自行修改程序，以符合个人的需求  
  
### 可执行文件  
> Linux系统中能不能执行一个文件，需要看有没有对该文件的执行权限，当然，前提是该文件是一个可执行文件，Linux系统真正认识的可执行文件其实是二进制文件(binary program),可通过指令"file 文件名"查看文件是否为可执行文件  
  
要获得可执行的binary程序，我们需要通过编辑器编写程序源码，将源码编译成系统看的懂的binary program  
### 函数库  
> 函数库包括静态函数库与动态函数库，Linux核心提供了很多核心相关的函数库与外部参数，这些核心功能在设计硬件的驱动程序是非常有用，相关的文件位置：/usr/include,/usr/lib,/usr/lib64  
  
### make与configure  
1. 我们在执行make时，make会在当前目录下搜索Makefile文件，Makefile文件中记录了原始码如何编译的详细信息，make会自动判断原始码是否经过变动而自动更新执行文件  
2. configure(config)是一个测试程序的文件，用于测试操作环境是否有软件开发商所需要的其他功能，当测试完毕之后，就会主动建立其软件需要的Makefile  
  
configure侦测程序会侦测的内容：  
* 是否有适合编译器可以编译该软件的程序码  
* 是否已经存在该软件所需要的函数库或者其他相依的软件  
* 操作系统是否适合该软件，包括Linux的核心版本  
* 核心表头定义文件(header include)是否存在  
### Tarball软件  
> 程序源码文件就是写了程序源码的文字文件，这是一种比较浪费空间的文件格式，Tarball文件就是将软件的所有源码文件先tar打包，再使用压缩技术进行压缩，比如常见的gzip，所以其一般后缀名为*.tar.gz或者*.tgz  
  
Tarball软件包解压之后包括的文件：  
* 源代码文件  
* 侦测程序文件(configure/config)  
* 软件的说明与安装说明(INSTALL/README)  
### 软件的安装与升级  
* 直接以源码通过编译来安装或升级  
* 直接以编译好的binary program来安装或升级  
### 编译生成binary program的步骤  
1. 使用gcc进行源码的编译（会生成目标文件object files）  
2. 通过gcc进行函数库、主程序、副程序的连接，形成主要的binary file  
### gcc的一般用法  
* gcc -c hello.c:编译源码，生成对应的目标文件(hello.o)，但不会生成binary执行文件  
* gcc -o hello hello.o:将目标文件生成对应的binary file  
* gcc -O -c hello.c:编译源码,生成最佳化参数的目标文件(hello.o) 
* gcc -o thanks thanks1.o thanks2.o:将目标当连接制作成一个binary执行文件  
* gcc sin.c -lm -L/lib64 -I/usr/include:-lm表示编译时加入额外的函数库("-l"表示加入某个函数库(library)的意思，"m"表示"libm.so"或"libm.a",就是去掉函数库开头的"lib"和后缀名(".so",".a"))；-L表示在哪里搜索-l加入的函数库；-I表示源码(sin.c)中include导入的"*.h"文件所在的目录  
