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

  ![image-20200517213043932](F:\onedrive_data\OneDrive - zju.edu.cn\git\spring-learn\images\image-20200517213043932.png)选项中勾选utf-8编码

  

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

![image-20200517220907014](F:\onedrive_data\OneDrive - zju.edu.cn\git\spring-learn\images\用户-组-家目录.png)

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

  ![image-20200518154641013](F:\onedrive_data\OneDrive - zju.edu.cn\git\spring-learn\images\用户配置信息.png)

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
  * **had -f [文件] 实时追中文档的所有更新** （常用）

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
  * tar -zxvf  a.tar.gz  加压缩
  * tar -zxvf  a.tar.gz   -C  /opt/  解压到opt目录，目录必须存在

# linux实操

## 组管理

* 文件包含：文件的所有者，文件所在组，其他组（所在组排除的组）

* 一般文件的创建用户就是文件所有者

  * 查看所有者 ls -ahl    

    ![image-20200518180933756](F:\onedrive_data\OneDrive - zju.edu.cn\git\spring-learn\images\tom.png)

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
  * 1 如果是文件表示硬连接的数，如果是目录表示该目录的子目录有多少
  * tom 文件的所有者
  * police文件所在组
  * 0 表示文件大小，如果是目录则是4096
  * 5月 18 18：08 文件最后的修改时间

* w代表文件可写，但是不能删除，除非w作用在目录上则可以删除文件
* x作用在目录上则表示可以进入该目录，r作用在目录上可以ls查看