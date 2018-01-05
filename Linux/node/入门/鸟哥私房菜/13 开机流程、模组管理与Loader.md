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
> 核心文件放置的位置：/boot/文件夹下，核心模组放置位置：/lib/modules/目录下  
  
当boot loader读取核心文件后，Linux会将核心解压到内存中，然后利用核心的功能，开始测试和驱动各周边装置（包括：存储装置，CPU，网卡，声卡等），这时Linux核心会以自己的功能重新测试一次硬件  
为了硬件开发商与其他核心功能开发者的便利，Linux核心是可以通过动态载入核心模组（module，其实就是我们所说的驱动），这些核心模组放置在/lib/modules/目录下，因为核心模组放置在磁盘根目录下（就是需要/lib和/必须在一个partition中），在开机过程中核心必须挂载根目录，这样才能读取核心模组提供载入驱动程序的功能  
一般来说，非必须的功能都可以编译成为模组的核心功能，现在有一种情况，假设Linux安装在SATA磁盘上面，我们可以通过BIOS的INT 13来取得boot loader与Kernel文件来开机，然后kernel开始接管系统并侦测硬件以及尝试挂载根目录来取得额外的驱动程序，但是，核心根本不认识SATA磁盘，所以需要载入SATA的驱动程序，否则无法挂载根目录，但是SATA的驱动程序在/lib/modules/内，在没有挂载根目录的情况下怎么能读取到/lib/modules/中的驱动程序呢，在这种情况下，我们需要使用虚拟文件系统来处理这个问题  
虚拟文件系统(Initial RAM Disk或Initial RAM Filesystem)一般使用的文件名为/boot/initrd或/boot/initramfs，他也能够通过boot loader载入到内存中，然后这个文件被解压并在内存中模拟成一个根目录，且这个模拟在内存中的文件系统能提供一支可执行的程序，通过该程序来载入开机过程中所需要的核心模组，等载入完成后，会帮助核心重新呼叫systemd来开始后续的正常开机流程  
### initramfs  
> boot loader可以载入kernel和initramfs，然后在内存中让initramfs解压成为根目录，kernel就能以此载入适当的驱动程序，最终释放虚拟文件系统，并挂载实际的根目录文件系统，开始后续的正常开机流程  
  
initramfs其实是一个小型的根目录，这个小型的根目录也是通过systemd来进行管理，并通过initrd.target来开机，initrd.target也需要读入一堆basic.target，sysinit.target等硬件侦测、核心功能开启的流程，然后开始让系统顺利运作，最终卸载initramfs文件系统，实际挂载系统的根目录  
### 第一支程序systemd及使用default.target进入开机程序  
> systemd是当核心载入完毕，运行完成硬件侦测和驱动载入之后，核心主动呼叫的第一支程序，其主要功能是准备软件执行的环境，包括系统的主机名称，网络设定，语言处理，文件系统格式和其他服务等，所有的动作都通过/etc/systemd/system/default.target设定来启动服务  
  
#### 常见的操作环境target和相容与runlevel的等级  
> 预设的操作环境(default.target)包括multi-user.target和graphical.target,以及rescure.target, emergency.target, shutdown.target, initrd.target等  
  
systemd为了相容与systemV操作行为，将runlevel与操作环境做了结合，使用指令："ls -lid /usr/lib/systemd/system/runlevel*.target | cut -c 28-"可查看runlevel和操作环境的对应关系  
systemd的处理流程：  
1. 连接到/usr/lib/systemd/system/目录去取得multi-user.target或graphical.target，根据/usr/lib/systemd/system/graphical.target或/usr/lib/systemd/system/multi-user.target中的设定进行相关项目的载入  
2. 找到/etc/systemd/system/graphical.target.wants/或/etc/systemd/system/multi-user.target.wants/目录下的使用者设定载入的unit  
3. 找到/usr/lib/systemd/system/graphical.target.wants/或/usr/lib/systemd/system/multi-user.target.wants/目录下系统设定载入的unit  
### systemd执行sysinit.target初始化系统、basic.target准备系统  
sysinit.target执行的操作：  
* 特殊文件系统装置的挂载：包括dev-hugepages.mount, dev-mqueue.mount等挂载服务，挂载成功后会在/dev/目录下建立对应的目录  
* 特殊文件系统的启用：包括磁盘阵列，网络磁盘(iscsi)，LVM文件系统，文件系统对照服务(multipath)等  
* 开机过程的信息传递与动画执行：使用plymouthd服务搭配playmouth指令来传递动画与信息  
* 日志式登录档的使用：就是启动systemd-journald服务  
* 载入额外的核心模组：通过/etc/modules-load.d/*.conf文件的设定  
* 载入额外的核心参数设定：包括/etc/sysctl.conf以及/etc/sysctl.d/*.conf内部的设定  
* 启动系统随机数产生器：随机数产生器可以帮助系统进行一些密码加密演算的功能  
* 设定终端机(console)字体  
* 启动动态装置管理员：就是udevd，用于动态对应实际装置存钱与装置文件名对应的一个服务  
basic.target执行的操作：  
* 载入alsa音效驱动程序  
* 载入firewalls防火墙  
* 载入CPU的微指令功能  
* 启动与设定SELinux的安全文本  
* 将目前开机过程中产生的开机信息写到/var/log/dmesg中  
* 由/etc/sysconfig/modules/*.modules及/etc/rc.modules载入管理员指定的模组  
* 载入systemd支援的timer功能  
完成这个阶段之后，系统已经可以顺利的运行，就差登录服务，网络服务，本机认证服务等service的启动了  
### systemd启动multi-user.target下的服务  
> 在载入核心驱动后，经过sysinit.target初始化流程让系统可以存取之后，basic.target让系统成为操作系统的基础之后，若想让服务器顺利运行，就需要启动各种主机服务以及提供服务器功能的网络服务，这些服务大多挂载在multi-user.target环境下（可查看/etc/systemd/system/multi-user.target.wants/里面的内容）  
  
### 开机过程中会使用的主要设定档  
* /etc/modules-load.d/*.conf:要核心载入模组的位置  
* /etc/modprobe.d/*.conf:可以加上模组参数的位置  
* /etc/sysconfig/*:常见的环境设定档  
## 核心与核心模组  
> 在开机过程中，能否成功的驱动我们主机的硬件设备，是核心(kernel)的工作，核心一般是压缩文件，使用核心之前，需要将其解压之后，才能载入到内存中；目前的核心都具有读取模组化驱动程序的功能，就是modules(模块化)的功能  
  
核心和核心模块相关文件：  
* 核心：/boot/vmlinuz或/boot/vmlinuz-version  
* 核心解压所需RAM Disk：/boot/initramfs（/boot/initramfs-version）  
* 核心模块：/lib/modules/version/kernel或/lib/modules/$(uname -r)/kernel  
* 核心原始码:/usr/src/linux或/usr/src/kernels/  
* 核心版本：/proc/version  
* 系统核心功能：/proc/sys/kernel/  
### 当我们有个新的硬件，但是系统不支持，我们需要做的操作  
1. 重新编译核心，并加入新的硬件驱动程序源码  
2. 将硬件的驱动程序编译成模块，并在开机时载入该模块  
### 核心模组的观察  
* 使用指令"lsmod"查看目前核心载入了多少模块  
* 使用指令"modinfo [-F field] [-adln] [module_name|filename]"查看某个模块的信息  
* 使用指令"insmod [/full/path/module_name] [parameters]"来载入指定的模块  
* 使用指令"rmmod [-fw] module_name"来卸载指定的模块  
* 使用指令"modprobe [-cfr] module_name"来加载或卸载指定的模块（modprobe相对于insmod/rmmod的好处在于modprobe能搜索安装/卸载模块的相依性(就是modules.dep中的内容)并且不需要绝对路径）  
## Boot Loader：Grub2  
> boot loader是载入核心的重要工具，没有boot loader，kernel根本没有办法被系统载入  
  
boot loader安装在MBR中，MBR是整个磁盘的第一个sector内的一个区块，大小只有446bytes，所以我们的boot loader的程序码和设定资料不可能全部放置在MBR中，所有Linux在执行boot loader的程序码和设定值载入会分为两个阶段(stage)  

### boot loader的两个阶段(stage)  
1. 执行boot loader主程序：这个主程序必须被安装在开机区(MBR)  
2. 主程序载入设定档：通过boot loader载入所有的设定档与相关的环境参数文件(在/boot下，如与grub2有关的设定档放置在/boot/grub2/目录下)  
### grub2的设定档：/boot/grub2/grub.cfg  
grub2的优点：  
* 认识与支援较多的文件系统，并可以使用grub2的主程序直接在文件系统中搜索核心文件名  
* 开机的时候，可以自行编译与修改开机设定项目，类似bash的指令模式  
* 可以动态的搜索设定档，而不需要在修改设定档后重新安装grub2，也就是只要修改/boot/grub2/grub.cfg中的内容，下次开机就生效了  
