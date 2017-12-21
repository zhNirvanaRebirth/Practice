# Linux账号管理与ACL权限设定  
## Linux账号与群组(UID&GID)  
### 相关的文件  
* /etc/passwd:保存了系统中所有的账号  
* /etc/group:保存了系统中所有的群组  
* /etc/shadow:保存了系统中所有账号对应的密码（当然是加密了的）  
### 使用者账号登录过程  
当我们登入主机以取得shell的环境来工作时，当我们输入账号和密码后，系统将进行下面的工作：  
1. 在/etc/passwd中寻找是否有该账号，若没有直接跳出，若有则将该账号的UID和GID(在/etc/group中)读出来，并将该账号的家目录与shell一起读出来  
2. 核对密码，通过该账号的UID在/etc/shadow中找到对应的账号密码并进行核对  
3. 密码核对成功就进入shell控管阶段  
### /etc/passwd文档结构  
> 每一行都代表一个账号，有多少行就代表系统中有多少个账号  
  
每一行的内容，如："zh:\x:1000:1000:zh:/home/zh:/bin/bash"  每一项以分号分隔，详细解释如下：  
1. zh：账号名称，显示给我们看的  
2. x：密码，这是之前设计的，因为密码在这里的话，所有的程序都能读取，所以真正的密码在/etc/shadow文件中  
3. 1000：UID，UID分几个范围，0表示root用户，1-200用于系统发行商自用，201-999创建系统账号使用，1000-60000给一般使用者使用  
4. 1000：GID，与/etc/group中对应，表示该账号对应的群组  
5. zh：使用者的详细说明  
6. /home/zh：家目录  
7. /bin/bash：shell，账号登录成功之后会取得的一个shell用来与系统的核心贯通以进行相应指令任务  
### /etc/shadow文档结构  
> 每一行对应一个账号的密码及相关内容，该文档属于的用户与群组都是root，且操作权限是000，所以这个文件只能root用户读取或进行修改  
  
每一行的内容，如：zh:$6$DHI5tj9Yf/bJ.hAZ$nOuqpVLTqu68lnvQIEVjiKSVvDmMoITYcLHO/OeHcgjJDjbkmj502fG6CugB/fsMxcEnKMWspfgbla/X1/Pd20::0:99999:7:::  每一项以分号分隔，详细解释如下：  
1. zh：使用者账号，与/etc/passwd文件中的对应  
2. $6$DHI5tj9Yf/bJ.hAZ$nOuqpVLTqu68lnvQIEVjiKSVvDmMoITYcLHO/OeHcgjJDjbkmj502fG6CugB/fsMxcEnKMWspfgbla/X1/Pd20：加密后密码，都是采用的非可逆加密，若此栏的内容是"!"或"*",表示该账号的密码暂时失效了（失效当然就不能做登录使用咯）,可使用指令"passwd"来进行密码的修改，使用"authconfig --test | grep hashing"来进行密码加密机制的查看  
3. 第三项：密码最近一次修改的时间（注意该时间是一个整数，表示的是自1971/01/01到最后一次修改时间的天数）  
4. 第四项：密码从最后一次修改时间开始多少天不能再进行修改  
5. 第五项：密码从最后一次修改时间开始多少天之内需要再次进行修改  
6. 第六项：密码需要进行修改日期前多少天开始进行警告  
7. 第七项：密码过期后账号的宽限天数，在密码需要进行修改却没有进行密码修改，此后密码就失效，这项的意思是在密码失效之后，用户还可以使用该账号登录系统，但是每次登录，系统都会强制要求重新设置密码才能进入系统  
8. 第八项：账号过期时间，这里也是从1971/01/01开始到账号过期的天数
9. 第九项：保留项  
### /etc/group文档结构  
> 每一行对应一个群组及相关内容，/etc/passwd文件中的每个用户所属的群组必须在这里能找到  
  
每一行的内容，如："zh:\x:1000:zh"  每一项以分号分隔，详细解释如下：  
1. zh：群组名称  
2. x：群组密码，密码设定在/etc/gshadow文档中  
3. 1000：GID，/etc/passwd中每一行的第四项(群组id)和这里的GID对应可知道该用户属于哪个群组  
4. zh：此群组支援的账号名称，这里可以填多个，主要作用是什么呢，都知道Linux中每个文件都有所属的群组，并指定了相关群组可以执行的操作权限，那这里这个群组支持的账号有多个的话，这几个用户去操作该文件所能够获取的操作权限都是一样的（该群组的权限）  
### /etc/gshadow文档结构  
> 每一行记录一个群组的群组管理员的信息  
  
每一行的内容，如：zh:!!::zh  每一项以分号分隔，详细解释如下：  
1. 第一项：群组的名称  
2. 第二项：密码，若开头为"!"表示没有合法密码，即没有群组管理员  
3. 第三项：群组管理员账号  
4. 第四项：支援该群组的所有使用者账号（同/etc/group）
### 群组  
* 有效群组：用户当前创建一个文件，这个文件所属的群组即为当前用户的有效群组（用户可以支援多个群组，通过"groups"可查看用户支持的群组，使用命令"newgrp"可以进行有效群组的切换）  
* 初始群组：在/etc/passwd文档中每一行用户设定的第四项设定的GID所属的群组，即为用户的初始群组
#### 相关操作与指令  
* 可通过"id 账号名"来查看相关的用户的uid、gid与groups等  
* 使用"groupadd [-g gid] [-r] 群組名稱"新增群组  
* 使用"groupmod [-g gid] [-n group_name] 群組名"修改群组相关信息  
* 使用"groupdel [groupname]"删除群组，能删除成功的前提是没有账号使用该群组  
#### gpasswd群组管理员功能  
* "gpasswd groupname"；给群组设置密码  
* "gpasswd [-A user1,...] [-M user3,...] groupname"：设置该群组的管理员和成员（-A：设置管理员，-M：添加账号）  
* "gpasswd [-rR] groupname"：移除或设置密码失效  
* "gpasswd [-ad] user groupname"：向群组中添加或删除账号  

## 账号管理  
* useradd：新增使用者  
* passwd：密码设定  
### useradd新增使用者  
> 指令：useradd [-u UID] [-g 初始群組] [-G 次要群組] [-mM] [-c 說明欄] [-d 家目錄絕對路徑] [-s shell] 使用者帳號名  
  
我们可以直接使用"useradd 账号名"来新增使用者，系统会帮我们处理以下的几个项目：  
* 在/etc/passwd中创建一行新增使用者的账号相关的信息，包括UID/GID（这里的UID数值来源：查看当前创建的账号是什么类型，比如现在创建的是一般使用者，则查看/etc/passwd中一般使用者的最大的UID到多少了，然后新增的使用者UID在该基础上+1就行了；GID来源：通过"useradd -D"可查看新增使用者默认使用的GID应该是多少，然后再使用该值和/etc/group中对应类型的新增使用者的最大GID比较，取大值，然后再大值的基础上+1）  
* 将密码写入/etc/shadow中对应的位置，默认密码是失效的  
* 在/etc/group中加入一行群组名称和账号名称一样的信息  
* 在/home下创建一个与账号名称相同的目录作为该新增用户的家目录，且权限是700  
#### useradd相关操作  
* useradd 账号名：新增一般使用者  
* useradd -r 账号名：新增系统账号（不会主动在/home下创建家目录）  
#### useradd参考档  
> 我们使用useradd添加使用者时，系统会参照一些默认设置，这些默认设置保存在文档/etc/default/useradd中，可通过"useradd -D"查看，详细信息如下：  
  
* GROUP=100：表示新增使用者时预设的群组，这里预设的GID是100，我们通过查看/etc/group文档中，发现users这个群组的GID是100，所有默认新增使用者是应该将其初始群组设为users（公共群组机制），但是在centOS上，系统的默认设置是将群组设为和账号名相同的群组（采用的是私有群组机制）  
* HOME=/home：表示使用者家目录的基准目录(basedir)  
* INACTIVE=-1：表示密码过期后是否失效的设定值，-1表示密码过期后也不会失效，0表示密码过期后立即失效，10表示密码过期后10天后失效  
* EXPIRE=：账号失效的日期，这里直接设定在哪个日期账号失效，可用在会员制系统设定中  
* SHELL=/bin/bash：表示新增账号能使用的shell  
* SKEL=/etc/skel：新增使用者家目录下的基准目录，比如，我们在/etc/skel目录下创建一个www目录，那么我们新增出来的账号的家目录下就会有www目录，同时我们可以修改/etc/skel目录下的.bashrc文档中的变量，这样我们新增的账号就能使用里面设置的变量了  
* CREATE_MAIL_SPOOL=yes：是否建立使用者的mailbox，我们可以在新增使用者成功之后进入/var/spool/mail/目录下找到相应账号的邮箱目录  
#### /etc/login.defs参考档  
> 新增使用者时，其UID、GID和密码等值的参考，详细内容如下：  
  
* MAIL_DIR：使用者邮箱目录  
* PASS_MAX_DAYS：在修改密码之后的多少天内必须再次进行密码的修改，否则密码将过期  
* PASS_MIN_DAYS：在修改密码之后多少天内不能再次进行密码的修改  
* PASS_WARN_AGE：密码过期前警告的天数  
* UID_MIN：一般使用者的能使用最小UID  
* SYS_UID_MIN：保留给使用者自行设定的系统账号的最小UID  
* GID：一般使用者能使用的最小GID  
* SYS_GID_MIN：保留给使用者自行设定的系统账号的最小GID  
* CREATE_HOME：在不使用-M或-m时，是否会创建使用者家目录  
* UMASK：使用者家目录创建的umask，当这里时077时，家目录的权限时700  
* USERGROUPS_ENAB：使用userdel删除使用者时，是否会删除其初始群组（当该群组没有账号时）  
* ENCRYPT_METHOD SHA512：密码加密使用的机制(sha512)  
#### passwd相关操作  
> 我们使用"useradd 账号名"新增使用者时，其密码设定默认是失效的(密码以!开头)，所有我们如果想使用这个账号，需要对其进行密码设置  
> 命令：passwd [-l] [-u] [--stdin] [-S] [-n 日數] [-x 日數] [-w 日數] [-i 日數] 帳號  
  
* 查看密码的相关信息：使用"passwd -S"或"chage -l 帳號名"  
* chage [-dEImMW] 帳號名：修改密码的相关信息  
#### usermod相关操作  
> 使用"usermod [-cdegGlsuLU] username"命令进行账户相关信息的修改  
  

#### userdel相关操作  
> userdel：删除使用者资料，当账号的初始群组没有其他用户属于该群组时，该群组也会被删除  
> 命令："userdel [-r] username"，能删除的资料如下：  
  
* 使用者账号与密码：/etc/passwd,/etc/shadow  
* 使用者群组：/etc/group,/etc/gshadow  
* 使用者个人资料：/home/username,/var/spool/mail/username  
### 使用者功能  
> useradd,usermod,userdel等命令都是root用户才能使用，一般使用者能使用的功能如下：  
  
* "id [username]"：查看username的UID，GID和groups  
* "finger [-s] username"：查看使用者的信息  
* "chfn [-foph] [帳號名]"：修改使用者的信息  
* "chsh [-ls]"：查看系统上能使用的shell或修改使用者的shell  
## ACL  
> ACL:Access Control List,是提供user,group,other管理权限之外的细部权限设定，ACL可针对单一用户，单一档案或目录进行权限设定  
  
ACL可以针对的权限设定：  
* 可以针对使用者来设定权限  
* 可以针对群组来设定权限  
* 可以针对在某个目录下新建文档/目录时设置权限  
### ACL设定技巧  
* getfacl:获取某个档案/目录的ACL设定  
* setfacl:设置某个档案/目录的ACL设定，如："setfacl -m u:vbird1:rx acl_test1"  
## 使用者身份切换  
* "su"：使用non-login shell方式变成root用户，这种方式切换，很多变量不会改变，比如原来的邮箱(/var/spool/mail/username)等  
* "su -"：使用login shell方式变成root用户  
* "su - -c 指令"：使用一次root用户身份执行指令，完成之后返回当前用户  
* "sudo"：在切换使用者时只需要输入自己的密码，且只有在/etc/sudoers中的用户才能使用该命令  
* 使用visudo来修改/etc/sudoers中的内容  
## 使用者的特殊shell和PAM  
> 如果我们想让某个账号只能访问系统的mail资源，当我们不给某个账号设置密码，该账号是无法登陆Linux主机的，自然就不能使用mail资源，但是我们给该账号设置了密码，该账号能登录主机，就能访问一般使用者能访问的资源  
> /sbin/nologin：特殊的shell，如系统账号使用的shell是/sbin/nologin，所以系统账号是不需要登录的，就算给了其密码，它也无法登录，当我们使用该账号（其使用shell为/etc/nologin）时，会提示/etc/nologin.txt中的内容  
> PAM：Pluggable Authentication Modules, 嵌入式模組，提供了一连串的验证机制，使用者将验证需求告知PAM，PAM输入验证结果  

