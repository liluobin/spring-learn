# some centos instrucions

## 远程登录centos

* 使用Xshell5

  查看linux的ip地址：ifconfig

  setup->系统服务->勾选sshd

* Xshell新建连接

## 远程传输文件

* 使用**Linux-Xftp5**

  设置端口时需要将ftp改成**sftp**，因为centos只开放了22号端口

* 连接成功后文件名乱码解决方法：

  ![image-20200517213043932](.\images\image-20200517213043932.png)选项中勾选utf-8编码

  

# vi和vim的使用

* 正常模式

  可以使用快捷键

  ```java
  //输入
  vim hellow.java  //进入正常模式
  ```

* 插入模式

  按下**i,I,o,O,a,A,r,R**任何一个后键入编辑模式

* 命令行模式

  exc退出编辑

  :wq 保存并退出     :q!强制退出，不保存

* **vi和vim的快捷键**

  ```java
  //拷贝当前行
  在正常模式下输入yy复制，按p粘贴
  输入5yy，复制当前行一下的五行
      
  //删除当前行
  输入dd，5dd删除当前行下5行
     
  //查询关键字
  先输入/
  加关键字，进入搜索
  按n选择
   
  //添加和删除行号
  :set nu
  :set nonu
      
  //跳转到文件的最末行G，跳转到首行gg
      
  //撤销一个动作
  正常模式下输入u
      
  //跳转到指定行
  输入20（行号）
  输入shift+g  //注意，不能用小键盘输入
  ```

# 常用指令

* 关机:

  shutdown -h now  立即关机

  shutdown -h 1  一分钟后关机

  shutdown -r now   立即重启

  halt   效果等价于关机

  reboot    重启系统

  **sync**	把内存数据写入到磁盘上，***关机和重启前应先sync***



* 用户登录和注销
  * 用户注销:logout   (在远程端有效，在图形界面无效)

* 用户，组，家目录的关系

![image-20200517220907014](.\images\用户-组-家目录.png)

* 一个用户至少属于一个组
  * useradd [userid]	会自动创建[名为userid]的组	
  * useradd -d /home/tiger/  [userid]   创建到指定目录
  * passwd [userid]     为指定用户添加密码
    * pwd    可以显示当前目录
  * userdel [userid]    删除用户并保留家目录（工作中一般会保留家目录）
  * userdel -r [userid]   删除用户和家目录
  * id [userid]    查询用户和组的信息 

* **切换用户**
  * su - [userid]     从权限高到低不用输入密码
  * whoami 可以查看当前是什么用户

* 用户组
  * 类似于角色，对相同特性的用户统一管理
  * groupadd [组名]    创建一个组
  * groupdel [组名]    删除一个组
  * useradd -g [用户组]  [用户名]    将用户添加到指定组
  * usermod -g [用户组] [用户名]    修改用户的组

* /etc/passwd   用户配置信息

  ![image-20200518154641013](.\images\用户配置信息.png)

* /etc/shadow  密码配置文件（加密的形式）

* /etc/group   组配置文件

  包含组名、组口令、组标识号、组内用户信息（隐藏）



#  实用指令

* 指定运行级别

  包含七个运行级别  0：关机 ； 1：单用户（不需要密码就能root登录）； 2：多用户无网络服务

  3：多用户有网络服务（用的最多）  4：保留级别（没有启用）  5：图形界面级别   6：重启

  * **/etc/inittab 系统运行级别的配置文件**（id:5 initdefault这一行的数字）*centos7以上无效*
  * 切换运行级别的指令  **init [012356]**
  * 面试题：如何找回丢失的root密码；**进入单用户模式，再修改root密码**
    * 操作：物理关机，开机时按enter键进入bios界面（ubuntu是*shift*键），按e，进入后选kernel（第二个）继续按e，进入后修改command为1，回车键，返回后再输入b，进入引导。***passwd root修改密码***

* 帮助指令
  * man [命令或配置文件]
  * help [命令]

* 文件目录类指令

  * pwd 显示当前工作目录绝对路径
  * ls [选项] [目录或文件]    -a显示当前目录所有文件信息（隐藏文件）   -l 以列表形式显示
  * cd [参数]  **cd ~回到家目录**； cd ..回到上一级
    * 当前目录在/root 如果用绝对目录则：*cd /home/* ;如果用相对目录则：*cd ../home/* 
  * mkdir 用于创建目录   mkdir /home/dog
    * 创建多级目录   mkdir -p /home/animal/tiger
  * rmdir 删除目录  rmdir [选项] [目录]
    * rmdir /home/dog     **如果目录非空那么无法直接删除**
    * rm -rf /home/dog     **删除非空的目录**

  * touch  创建空文件
    * touch [文件名] [文件名2...]
  * **cp  拷贝指令**
    * cp [选项]  [source] [dest]      单个文件和文件夹都可以（拷贝整个文件夹时要加-r，递归拷贝）；**强制覆盖不提示的方法：\cp**

  * rm [-r |-f] [要删除的文件和目录]    -r递归删除；-f强制删除不提示

  * mv [原来的文件名] [新的文件名]     （移动或者重命名）
    * mv aaa.txt  pig.txt     （重命名,当前目录有aaa.txt则重命名）
    * mv  /home/pig.txt  /root/     （当前目录的pig.txt移动到root目录下）
  * cat  以只读的方式显示文件内容  cat [-a|-n] [文件名]   （-n显示行号）
    * cat -n /etc/prifile | more  分页显示，按空格键翻页
  * more是基于vi的文本过滤器，全屏按页显示文件，空格翻页，enter换行，ctrl+b|f上下切换
  * less 与more类似，但不是一次性加载，按需加载
    * less|more  [文件名]

## 文件目录类

* \>输出重定向
  
  * ls -l '>' [文件] 将列表内容写入到文件，文件不存在则创建，会覆盖原来的文件
* \>>追加
  
* ls -l '>>' [文件] 将列表内容追加在文件后面
  
* echo "内容" [>|>>] [文件]  把内容写入到文件中
  
* echo [选项] [输出内容]  把内容输出到控制台   echo $PATH (输出环境变量)
  
* head  显示文件开头部分
  
* head -n 5 [文件]
  
* \*tail 显示文件的尾部，用法与head类似
  
* **tail -f [文件] 实时追中文档的所有更新** （常用）
  
* **ln 软连接符号，主要存放连接其他文件的路径**，类似windows的快捷方式

  * ln -s [原文件或目录] [软连接名]  ln -s /root linkToRoot

  * cd linkToRoot/ 则进入root目录，但是如果ls显示的还是当前目录不变
  * 删除软连接  rm -rf linkToRoot  **后面一定不能加/，否则会删除目录和目录中的内容**

* history  查看已经执行过的历史指令，也可以执行历史指令

  * history [n]  显示n条指令

  * ! n   执行history编号为n的指令

    

## 时间日期类

* date 显示当前日期和时间
  * date “ + % Y|m|d|”
  * date "+%Y年%m %d  %H %M %S"
  * **设置日期** date -s 字符串时间 date -s "2018-10-10 11:55:22"

* cal 查看日历时间
  * cal 2020 显示2020的日历



## 搜索查找类

* find 从指定目录下递归查找

  * find [搜索范围] [选项]
  * find /home  -name  hello.txt    (-name 按名字查找，可以用通配符*)
  * find /opt  -user nobody   （按文件的用户进行查找）
  * find  /  -size +20M   (按文件大小来查找，+-或不写，k，M)

* locate可以快速定位文件，速度较快，在使用前需要先用updatedb创建数据库

  * updatedb    locate  hello.java

* grep 查找文件内部关键词，通常配合|管道符号使用

  * |表示将前一个命令的结果传递给后面的命令处理
  * grep [-n|-i] 查找的内容  原文件   -n输出行号，-i忽略大小写

  * cat hellow.java |grep -ni public



## 压缩和解压缩

* gzip/gunzip 压缩和解压缩文件    针对*.gz

  * gizp hello.txt   (压缩后原文件消失)  文件变为 hello.txt.gz

  * gunzip  hello.txt.gz

* zip/unzip 

  * zip [选项] [压缩的文件或目录]   -d指定解压后的目录  -r递归压缩
  * zip -r mypackage.zip  /home/   (将目录下的全部文件压缩成mypackage.zip)

  * upzip -d /opt/tmp/  mypackage.zip  (将压缩文件解压到指定目录)

* **tar 打包指令，既可以压缩也可以解压*.tar.gz**

  * tar [选项] XXX.tar.gz  打包的内容

  * -c 产生.tar打包文件  -v显示详细信息  -f指定压缩后的文件名  -z打包同时压缩  -x解压

  * tar -zcvf  **a.tar.gz**  al.txt  a2.txt  把al和a2打包压缩成a.tar.gz
  * tar -zcvf myhome.tar.gz  /home/  把home目录整体打包压缩
  * tar -zxvf  a.tar.gz  解压缩
  * tar -zxvf  a.tar.gz   -C  /opt/  解压到opt目录，目录必须存在

# linux实操

## 组管理

* 文件包含：文件的所有者，文件所在组，其他组（所在组排除的组）

* 一般文件的创建用户就是文件所有者

  * 查看所有者 ls -ahl    

    ![image-20200518180933756](.\images\tom.png)

  * 修改文件的所有者

    * chown  tom apple.txt  将root所有的apple.txt所有者改成tom，**但是所有组不变**

  * 修改文件的所在组

    * chgrp [组名] [文件名]

* 其他组，除了所在组和创建组外都是其他组

* 组的创建

  * groupadd monster/   useradd -g fox monster  /  id fox   查看所有住

* 改变用户的组(在root下)
  * usermod -g [组名] [用户名]  改变用户所在组（更改后以前创建的文件所有组不变）
  * usermod -d [目录名] [用户名]   改变用户的登录初始目录



## 权限的介绍

* **-rw-r--r--. 1 tom  police    0 5月  18 18:08 ok.txt**         ls -ahl 显示
* -表示普通文件，d 目录， l 软连接，c 字符设备（键盘鼠标），b 块文件（硬盘）
  
* rw- 表示文件所有者拥有的权限（r读，w写，x执行）
  * r-- 表示文件所在组的用户拥有的权限
  * 后面一个r--表示文件其他组用户拥有的权限
  * 1 如果是文件表示硬连接的数，如果是目录表示该目录的子目录有多少（包括.和..）
  * tom 文件的所有者
  * police文件所在组
  * 0 表示文件大小，如果是目录则是4096
  * 5月 18 18：08 文件最后的修改时间
  
* **w代表文件可写，但是不能删除，除非w作用在目录上则可以删除文件**
* **x作用在目录上则表示可以进入该目录，r作用在目录上可以ls查看**

* 权限管理，修改文件或者目录权限
  * u代表所有者  g  所有组   o其他人   a所有人（a、g、u的总和）
  * chmod u=wrx,g=rx,o=rw  ok.txt
  * chmod u -x ,g +w  ok.txt  （添加写权限和删除执行权限）
  * 规则： r=4,w=2,x=1
    * chmod u=rwx,g=rx,o=x  ok.txt   相当于chmod 751 ok.txt   (rwx可以看做二进制位)

* 修改文件的所有者-chown
  * chown newowner  file  
  * chown tom abc.txt  注意该用户是否有文件夹的写的权限
  * chown -R tom afile/  修改文件夹所有文件的所有者为tom，注意要用root用户操作

* 修改文件所在组 -chgrp
  * chgrp newgroup file 
  * chgrp -R newgroup  kkk/

---

## 定时任务调度

* crontab -e增加任务调度  -l显示 -r删除     简单的任务可以不写脚本（shell）直接在crontab中写
* crontab -e
* */1 * * * * ls -l/etc>>/tmp/to.txt  （每分钟调用一次ls -l...代码）
* crontab -r 删除任务
* 第一个* 代表分钟（0~59）  第二个*代表小时（0~23）第三个\*代表天（1-31） 第四个\*代表月（1-12）  第五个代表一周当中星期几（1-7）
* 特殊符号\* 代表任何时间   ，代表（1,2,3）中每个都取   -  （1-5）代表范围

![image-20200518213344270](F:.\images\特殊符号的作用.png)

* crond 任务调度
  * 每隔一分钟就将当前的日期信息追加到tmp/mydate
    * date>>tmp/mydate  创建/home/mytask.sh
    * 开启用户对文件的执行权限 chmod 744 /home/mytask.sh
    * crontab -e
    * */1 * * * *  /home/mytask.sh  设置定时调用脚本

* 每天凌晨两点半mysql数据库testdb备份到文件中
  * 编写文件home/mytask2.sh
    * /usr/local/mysql/bin/mysqldump -u root -proot testdb > /tmp/mydb.bak
    * 给执行权限
    * crontab -e
    * 0 2 * * * /home/mytask2.sh

* service crond restart  重启任务调度

---

## Linux磁盘分区和挂载

* 分区有主分区和逻辑分区
* mbr分区
  * 最多支持四个主分区，系统只能安装在主分区，拓展分区要占一个主分区，mbr最大只支持2TB但是兼容性最好
* gpt分区
  * 支持无限个主分区（windows最多支持128个分区），最大支持18EB的容量，windows7 64位后支持gpt

* Linux无论有多少分区，分给哪个目录，终归只有一个根目录，唯一的文件结构；Linux采用载入的处理方法，mount将磁盘挂在在文件上。

* 在Linux中，硬盘基本为scsi硬盘。标识符为hdx~的是IDE硬盘，x为盘号（a为基本盘，b基本从属盘，c辅助主盘，d辅助从属盘），~表示分区，前四个分区1-4表示。而scsi硬盘表示为sdx~。
  * lsblk -f  查看硬盘分区和挂载情况  （老师不离开 ）

* **增加一块硬盘挂载在/home/newdisk**

  1. 挂载一个硬盘，然后重启，lsblk查看硬盘

  2. fdisk /dev/sdb ,m打开指令列表，n添加一个新分区，选择区号1，w把disk写入表并离开

![image-20200518221614182](.\images\sdb分区.png)

   * 此时分区完成，但没有格式化

     3. mkfs -t ext4 /dev/sdb1     把sdb1格式化成ext4格式
     4. 挂载  先创建一个目录 mkdir /home/newdisk
        * mount /dev/sdb1  /home/newdisk    挂载硬盘到newdisk文件
        * 重启机器后挂载实效

     5. 实现永久挂载
        * vim /etc/fstab
        * 添加一行
        * mount -a  自动挂载

![image-20200518222727704](.\images\挂载修改配置文件.png)

* umont /dev/sdb1  或 umount /newdisk

* 查看磁盘 df -h
* du -h  /目录   查看指定目录的占用情况
  * -a 含文件  -h带计量单位  -s指定目录占用大小汇总  --max-depth=1子目录深度
  * 查询opt目录磁盘占用，深度为1   du -ach ---max-depth=1  /opt

## 常用的指令

* 统计home目录下文件的个数 ls -l  /home |grep "^-" | wc -l
  * ^-开头的是文件，d开头的是文件夹 ，wc -l统计

* 统计home目录下文件的个数，包括子目录的
  * ls -lR  /home |grep "^-" |wc -l     R递归查询

* 递归显示目录
  * yum install tree   安装tree指令
  * tree

---

## 网络配置-NAT模式（网络地址转换模式）环境

![image-20200522151500338](.\images\NAT网络.png)

* 修改ip地址
  * 查看ip地址，linux: ifconfig       windows: ipconfig
  * 如果修改虚拟机的ip地址，则需修改虚拟机设置中VMnet8的ip地址

* 查询连接：ping  [ip地址]

  ### centos设置自动连接网络

  系统-首选项-选定网卡-编辑-勾选自动连接

  ![image-20200522155735693](.\images\网络连接.png)

  * 缺点：每次启动自动获取动态ip，但每次的ip都不一样（不适合做为服务器）

* 使用固定的ip地址
  * 直接修改配置文件，并可以连接到外网
  * 编辑vim /etc/sysconfig/network-scripts/fcfg-eth0
    * 要求：将ip地址配置为静态，如：192.168.184.101
    * 在文件中将bootproto设置为static，onboot为yes，然后设置ip地址、网关、dns（dns和网关设置相同即可）。然后重启服务service network restart。

---

# 进程管理

* Linux每个程序都执行一个进程，每个进程都分配一个id号
* 每个进程都对应一个父进程，这个父进程可以复制多个子进程
* 每个进程都可能以两种方式存在，前台和后台，一般系统进程都是后台执行的
* **ps查看目前系统中那些进程在执行**
  * -a 所以进程信息  -u以用户格式显示进程信息  -x后台进程运行的参数

![image-20200518225445056](.\images\进程信息.png)

* ps -aux |grep sshd  过滤需要查找的进程

![image-20200518225648130](.\images\ps指令详解png)

* ps -ef  以全格式的形式查看进程的父进程（pid进程编号，ppid父进程编号）
* **终止进程**
  * kill和killall
  * kill [选项] 进程号   -9表示强制终止
    * kill 4010 ,先用 ps -aux |grep sshd 找到目标进程，然后kill
  * killall gedit
  * 强制kill一个终端 ps -aux |grep bash
    * kill -9 4090 (bash的进程号)
  * pstree 以树状形式展示进程
    * -p 显示进程的pid
    * -u 以用户的形式显示

* **服务管理**
  * service [服务名]  [start |stop | restart | reload | status]
  * 在centos7.0后不再使用service，而使用systemctl，用法类似
  * setup 可以选择自动启动各种服务
    * 或者查看etc/init.d/

![image-20200523161432459](.\images\系统服务.png)

* 服务的运行级别0~6七种

  * vim /etc/inittab（每个服务对每个运行级别都会设置一个是否自启动）

  * chkconfig 可以给每个运行级别设置自启动或关闭
    * chkconfig [服务名]  --list | grep xxx
    * chkconfig [服务名]  --list
    * chkconfig  --level  5  服务名  on/off

---

## 进程监控

### 动态监控进程

* 指令：top，与ps不同在于top在执行一段时间可以更新正在运行的进程
* top [选项]

![image-20200523163857171](.\images\top用法_u.png "u")



<img src=".\images\top用法_k.png" alt="image-20200523164019702" title="k" />

* top -d 10   每隔10秒刷新
* 按q退出 ， 按P以cpu使用率排序，M以内存使用率排序，N以PID排序

### 监控网络状态

- netstat [选项]

* netstat -anp   -an按一定顺序显示，-p显示哪个进程在调用

---

# RPM包管理和yum

* rpm：用于互联网下载包的打包及安装工具，Redhat Package Manager 类似于windows的setup.ext，（suse，redhat，centos等）都使用rpm。

* 查询已安装的rpm列表：rpm -qa|grep xx
* rpm -qi  [软件名]     查询安装rpm软件信息
* rpm -ql [软件名]      查询软件包中有哪些文件
* rpm -qf  [文件]         查询文件属于哪个rpm包
* rpm -e  [rpm包名]     删除软件包

* rpm 强制删除
  * ![image-20200523170556171](.\images\rpm强制删除.png)

* 安装rpm包
  * i 安装 ， v  提示 ， h 进度条
    * 先找到安装包，挂载上安装centos的iso文件，在/media/目录
    * 进入/media/目录，找到要安装的rpm包，用cp复制到/opt/路径
    * 再切换到/opt/路径，用rpm -ivh  [包名] 安装

### yum

* 是一个shell前端软件包管理器，基于rpm，能从指定的服务器下载rpm包并安装，可以自动处理依赖性关系，并且一次安装所有依赖的软件包。
* 查询yum服务器是否有需要安装的软件
  * yum  list|grep XXX               **没有-**

* 安装指定的yum包
  * yum install XXX

