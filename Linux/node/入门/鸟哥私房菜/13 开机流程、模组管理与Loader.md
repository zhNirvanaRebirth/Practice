# 开机流程、模组管理与Loader  
## Linux的开机流程  
1. 载入BIOS的硬件信息并进行自我测试，依据设定取得第一个可开机的装置  
2. 读取并执行第一个开机装置内MBR的boot Loader（也就是grub2，spfdisk等程序）  
3. 依据boot loader的设定载入Kernel，kernel会开始侦测硬件与载入驱动程序  
4. 在硬件驱动成功后，Kernel会主动呼叫systemd程序，并以detault.target流程开机  
* systemd执行sysinit.target初始化系统及basic.target准备操作系统  
* systemd启动multi-user.target下的本机与服务器服务  
* systemd执行multi-user.target下的/etc/rc.d/rc.local文件  
* systemd执行multi-user.target下的getty.target及登录服务  
* systemd执行graphical需要的服务  
## BIOS，boot loader与kernel载入  
> 这里的BIOS包括传统的BIOS和UEFI BIOS  
> 这里说的MBR是分区使用MBR或GPT中安装boot loader的部分  
### BIOS，开机自我检测与MBR/GPT  
在个人电脑架构下，要启动整个系统就要先让系统去载入BIOS(Basic Input Output System)，并通过BIOS程序去载入CMOS信息，根据CMOS中的设定值取得主机的各项硬件设定（如：CPU与周边设备的贯通时脉，开机装置的搜索顺序，磁盘的大小，系统时间，周边汇流排是否启动Plug and Play（PnP，随插即用装置），周边设备的I/O位置，与CPU贯通的IRQ中断等），取得这些信息之后，BIOS进行开机自我测试(Power-on Self Test,POST)，然后开始执行硬件测试的初始化，并设定PnP装置，之后定义出可开机的装置顺序，然后就会开始进行开机装置的资料读取了  
由于系统软件大多放置在磁盘中，所有BIOS会指定开机的装置以便我们可以读取磁盘中的操作系统核心档案，但是不同的操作系统的文件系统格式不相同，因此我们必须要以一个开机管理程序来处理核心档案的载入(load)问题，这个开机管理程序就是Boot Loader了，Boot Loader安装在开机装置的第一个磁区(sector)中，也就是我们说的MBR(Master Boot Record,主要开机记录区)，这里有个问题，核心档案需要loader来读取，但是不同的操作系统的loader也是不同的，BIOS是怎么读取MBR中的boot loader的呢？其实只要BIOS能够侦测到磁盘，他就有办法通过INT 13这条通道来读取该磁盘中的第一个磁区中的MBR  
### Boot Loader的功能  
> Loader最主要的功能是认识操作系统中的文件格式从而能载入核心到内存中去执行  
  
#### 多重操作系统  
每种操作系统都需要使用自己的loader才能将核心文档载入到内存中执行，由于每个文件系统(filesystem,或者partition)都会保留一块开机磁区(boot sector)提供操作系统安装boot loader，而通常操作系统预设都会安装一份loader到他的根目录所在的文件系统的boot secotr上。  
但是系统的MBR只有一个，我们怎么确定执行那个boot sector中的loader呢，这就与boot loader的以下功能有关了：  
* 提供选单：提供使用者可以选择的不同开机项目  
* 载入核心文件：直接指向可开机的程序区段来开始操作系统  
* 转交其他loader：将开机功能转交给其他的loader来负责  
**注意：我们在安装系统的时候，系统会保留一个开机磁区来供我们安装MBR，而boot loader是安装在MBR上的，在安装多种系统的时候，后安装系统的MBR会覆盖执行系统的MBR（因为MBR只有一个），而Windows的loader不具有控制权转交的功能，所以在安装多系统的时候，需要先按照Windows，再安装Linux**  
### 载入核心侦测硬件与initramfs的功能  
> 核心文件放置的位置：/boot/文件夹下  
  
当boot loader读取核心文件后，Linux会将核心解压到内存中，然后利用核心的功能，开始测试和驱动各周边装置（包括：存储装置，CPU，网卡，声卡等），这时Linux核心会以自己的功能重新测试一次硬件  
