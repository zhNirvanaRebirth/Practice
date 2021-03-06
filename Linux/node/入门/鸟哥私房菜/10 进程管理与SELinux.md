# 进程管理与SELinux  
## 进程(Process)  
> 在Linux系统中，触发任何一个事件时，系统都将其定义为一个进程，并给这个进程一个ID，称为PID，同时依据触发这个程序的使用者与相关属性关系，给予这个PID一组有效的权限设定  
  
### 进程与程序(process & program)  
我们执行一个程序或指令，就可以触发一个事件，这个事件就是一个进程，且其拥有一个PID；因为系统仅认识binary file，所以我们让系统工作是需要启动一个binary file的，这个binary file就是程序(Program)  
* 进程(process):程序被触发后，执行者的相关权限与属性、程序的程序码与所需资料等都会被载入内存，操作系统给予这个内存中的单元一个PID，因此，进程就是一个正在运行的程序  
* 程序(program):通常为binary program，放置在存储媒体中（如硬盘、光碟、软盘、磁带等），为实体档案的形态存在  
### fork and exec:程序呼叫流程  
Linux的进程呼叫通常称为fork-and-exec流程：进程都会由父进程以复制(fork)的方式产生一个一模一样的子进程，然后被复制出来的子进程再以exec的方式来执行实际要运行的程序，最终称为一个子进程的存在  
### 系统或网络服务：常驻在内存的进程  
常驻在内存中的进程通常负责一些系统提供的功能以服务使用者的各项任务，因此这些常驻进程被我们称为：服务(daemon)  
### Linux上的多人多工环境  
* 多人环境：在Linux系统上具有多种不同的账号，每个账号都有特殊的权限，而每个账号进入Linux的环境设定都可以由个人喜好来设定(在~/.bashrc中设定)  
* 多工行为：CPU的速度代表CPU每秒钟能执行多少次指令，而每个工作仅占去CPU几个指令次数，所以CPU每秒就能在各个进程之间进行切换了  
## 工作管理(job control)  
> 当我们登录系统取得bash shell之后，在单一终端界面下同时进行多个工作的行为管理  
  
进行工作管理的行为中，每个工作都是目前bash的子进程，彼此之间有相关性，我们不能使用job control的方式由tty1的环境去管理tty2的bash  
job control的限制：  
* 这些工作所触发的进程必须来自于目前shell的子进程(只管理自己的bash)  
* 前景工作：可以控制与下达指令的环境称为前景的工作  
* 背景工作：可以自行运行的工作，无法使用[ctrl]+c来终止，可使用bg/fg呼叫该工作  
* 背景中执行的程序不能等待terminal/shell的输入(input)  
### job control的管理  
* 直接将指令丢到背景中执行：&  (vim指令不能在背景中执行，因为背景中执行的认为不能等待输入)  
* 将目前的工作丢到背景中暂停：[ctrl]+z  (vim工作可以在背景中暂停)  
* 观察目前背景工作的状态：jobs [-lrs]  
* 将背景工作拿到前景来处理：fg  
* 让工作在背景中的状态变成运行中：bg  
* 管理背景中的工作：kill  
### 离线管理问题  
工作管理的背景与终端机有关，而不是将工作放到系统的背景中去  
我们可以设置工作在终端离线情况下也执行：  
* 使用at指令：at指令是将工作放到系统背景中，与终端机无关  
* 使用nohup指令：nohup指令可以让工作在离线或登出系统的情况下继续执行  
## 进程管理  
进程管理的重要性：  
* 操作系统的各项工作其实都是经过某个PID来达成的，能不能进行某项工作，就与该程序的权限有关了  
* 当系统很忙碌，整个系统的资源快被用光时，需要找到最耗资源的进程，并删除该进程，使系统恢复正常  
* 若某个程序写的不好，导致一个有问题的进程再内存中，我们需要找到它并删除  
* 同时在系统中执行多项工作，要让最重要的工作优先执行  
### 进程的观察  
* ps：将某个时间点的进程运作情况截取下来（ps -l:查询自己bash程序;ps aux:查阅系统运作的程序）  
* top：动态观察进程的变化  
* pstree  
### 进程间的管理  
> 进程之间通过传递信号(signal)来进行管理（告诉该进程该做什么）  
  
* kill -signal PID  
* killall -signal 指令名称  
### 进程的执行顺序  
> 系统同时间有非常多的进程在运行，只是绝大部分都在休眠(sleeping)，如果所有进程被同时唤醒，CPU处理进程的顺序由进程的优先执行(Priority)与CPU排程决定  
  
* Priority与Nice值：值越小，优先级越高（Priority值由核心动态调整，使用者无法调整，因此使用者只能修改Nice的值）  
nice值设定：  
* nice值可调整的范围为-20到19  
* root可随意调整自己或他人进程的Nice值，范围为-20到19  
* 一般使用者只能调整自己进程的Nice值，且范围为0到19  
* 一般使用者只能讲Nice值调高  
* 一开始执行一个程序就立即设定一个nice值，使用nice指令：nice [-n 數字] command  
* 调整某个已存在的PID的nice值，使用renice指令：renice [number] PID  
### 系统资源的观察  
* free：观察内存的使用情况
* uname:查看系统与核心相关的信息  
* uptime：观察系统启动时间与工作负荷  
* netstat：追踪网络或插槽档  
* dmesg：分析核心产生的信息  
* vmstat：侦测系统资源（CPU/内存/磁盘等）变化  
## SELinux  
> SELinux: Security Enhanceed Linux,就是安全强化的Linux  
  
* 自主式存取控制：Discretionary Access Control,DAC，就是依据进程的拥有者和文件资源的rwx权限来决定有无存取能力（缺点时root具有最高权限，可以做任何操作，具有读写操作权限的使用者可能执行一些误操作）  
* 委任式存取控制：Mandatory Access Control,MAC，可以根据不同的进程与特定的文件资源来进行权限的管控，使我们针对控制的主体从使用者变成了进程  
### SELinux的运行模式  
> SElinux是通过MAC方式来管控进程的，它控制的主体是进程，目标是能否读取的文件资源  
  
* 主体(Subject):要管理的进程  
* 目标(Object):控制读取的文件资源（文件系统）  
* 政策(Policy):targeted：针对网络服务限制较多，针对本机限制较少，是预设的政策；minimum:仅针对选择的进程来保护；mls：完整的SElinux限制，限制方面较为严格  
* 安全行本文(security context):类似与文件系统中的rwx  
### 安全行本文(Security Context)  
> 安全行本文是放置再文件的inode中的  
  
内容：（使用"ls -Z"指令查看secuirty context中的内容，中间以分号分割）  
* 身份识别(Identify):unconfined_u:不受限的用户，就是该文件是由不受限的进程产生；system_u:系统用户，大部分系统服务运行过程中产生的文件  
* 角色(Role):通过角色，我们可以知道这个文件是属于进程、文件资源还是代表使用者。object_r:代表文件或目录等文件资源；system_r:代表进程  
* 类型(Type):分为进程的域(domain)和文件资源的类型(type)  
### SELinux的模式  
> 通过指令"getenforce"来查看当前SELinux的模式，使用"sestatus"指令查看SELinux的相关信息  
  
* enforcing:强制模式，代表SELinux运行中，且已经正确的开始限制domain/type  
* permissive:宽容模式，代表SELinux运行中，但只会由警告信息并不会实际限制domain/type的存取  
* disable:关闭  
### SELinux政策内的规则(rules)管理  
> SELinux的三种模式会影响进程对文件的存取，除此之外，就是政策内的规则对进程的影响了  
  
* 通过指令"sestatus -b"或"getsebool -a"可以查看系统上的规则是否启动  
* 使用指令"setsebool -P [规则名称] [0|1]"关闭/开启SELinux规则  
