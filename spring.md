# Spring 框架两大核心机制（Ioc、AOP）

* loC（控制反转）/DI（依赖注入）
* AOP（面向切面编程）
* Spring是一个企业级开发框架，是软件设计层面的框架，优势在于可以将应用程序进行分层，开发者可以自主选择组件。
* MVC: Struts2、spring MVC
* ORMapping : Hibernate、MyBatis、 Spring Data

* 面向切面编程
  * ![image-20200523232934440](.\images\面向切面图解.png)



## 什么是控制反转

* 在传统的程序开发中，需要调用对象时，通常由调用者来创建被调用者的实例
  ，即对象是由调用者主动new出来的。
* 但在Spring框架中创建对象的工作不再由调用者来完成，而是交给loC容器来
  创建，再推送给调用者，整个流程完成反转，所以是控制反转。





### 如何使用IoC

* 创建Maven工程，在pom.xml添加依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.roubin</groupId>
    <artifactId>learnSpring</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.0.11.RELEASE</version>
        </dependency>
    </dependencies>

</project>
```

* 第一次使用IDEA时需要先在IDEA中安装lombok，settings->plugins
  * 如果遇到plugins加载不出来参考博客[Plugins加载不出来解决方案](https://blog.csdn.net/weixin_44945537/article/details/104436488)



* 

* 通过IoC创建对象，在配置文件中添加需要管理的对象，XML格式的配置文件，文件名可以自定义。java代码放在java包内，配置文件全部放在resouces包内。

