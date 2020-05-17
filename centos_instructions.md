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