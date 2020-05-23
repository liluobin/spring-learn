# 搭建javaEE环境

![image-20200523173458953](.\images\javaEE结构示意图.png)

* 安装JDK1.7

  * ![image-20200523175835301](.\images\javaee安装步骤.png)

  * vim /etc/profile  按G到文件末尾，插入
    * JAVA_HOME=/opt/jdk1.7.0-0.79
    * PATH=/opt/jdk1.7.0_79/bin:$PATH
    * export JAVA_HOME  PATH
    * 注销用户重新登录后生效

## 安装tomcat  web服务器

* ![image-20200523180943995](.\images\tomcat安装.png)

* linux本地服务器访问tomcat，http:// localhost:8080  （tomcat的端口号是8080）
* 在windows访问linux的tomcat
  * service iptables status 查询开放的端口，或在windows用telnet [ip]查询
  * 放行： etc/sysconfig/iptables,这样外网才能访问
  * centos7： **firewall -cmd --permanent --add -port =8080/tcp**
    * ![image-20200523181718337](.\images\开放8080端口.png)

## 安装eclopse

* ![image-20200523182137100](.\images\eclipse安装png)