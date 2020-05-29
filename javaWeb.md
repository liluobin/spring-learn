## Tomcat

Web应用服务器：Tomcat、Jboos、Weblogic、Jetty

* 安装Tomcat
  * bin：存放各个平台下启动和停止Tomcat服的脚本文件。
  * conf：存放各种Tomcat服务器的配置文件。
  * lib：存放Tomcat 服务器所需要的jar。
  * logs：存放Tomcar 服务运行的日志。
  * temp:Tomcat运行时的临时文件。
  * webapps：存放允许客户端访问的资源（Java程序）。
  * work：存放Tomcat将JSP转换之后的Servlet文件。

* IDEA集成Tomcat

  ![image-20200529170814589](.\images\tomcat配置.png)

  ![image-20200529170853736](.\images\关联web工程.png)



## servlet

* 什么是Servlet？

Servlet是Java Web开发的基石，与平台无关的服务器组件，它是运行在Servlet容器/Web应用服务器/Tomcat，负责与客户端进行通信。

Servlet的功能：

1. 创建并返回基于客户请求的动态HTML页面。
2. 与数据库进行通信。

* 如何使用Servlet？

Servlet本身是一组接口，自定义一个类，并且实现Servlet接口，这个类就具备了接受客户端请求以及做出响应的功能。

```java
package com.luobin.servlet;

import javax.servlet.*;
import java.io.IOException;

public class MyServlet implements Servlet {

    @Override
    public void init(ServletConfig servletConfig) throws ServletException {

    }

    @Override
    public ServletConfig getServletConfig() {
        return null;
    }

    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println("已经收到web请求");
        servletResponse.setContentType("text/html;charset=UTF-8");
        servletResponse.getWriter().write("你好，终于调试成功了");
    }

    @Override
    public String getServletInfo() {
        return null;
    }

    @Override
    public void destroy() {

    }
}

```

创建类实现Servlet接口

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <servlet>
        <servlet-name>MyServlet</servlet-name>
        <servlet-class>com.luobin.servlet.MyServlet</servlet-class>
    </servlet>

    <servlet-mapping>
        <servlet-name>MyServlet</servlet-name>
        <url-pattern>/myservlet</url-pattern>
    </servlet-mapping>
</web-app>
```

在web-info/web.xml中配置servlet

浏览器不能直接访问 Servlet文件，只能通过映射的方式来间接访问Servlet，映射需要开发者手动配置，有两种配置方式。

* 基于xml文件的配置方式
* 基于注解的方式   `@WebServlet("/myservlet")`

```java
package com.luobin.servlet;

import javax.servlet.*;
import javax.servlet.annotation.WebServlet;
import java.io.IOException;
@WebServlet("/myservlet")
public class MyServlet implements Servlet {

    @Override
    public void init(ServletConfig servletConfig) throws ServletException {

    }

    @Override
    public ServletConfig getServletConfig() {
        return null;
    }

    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println("已经收到web请求");
        servletResponse.setContentType("text/html;charset=UTF-8");
        servletResponse.getWriter().write("你好，终于调试成功了");
    }

    @Override
    public String getServletInfo() {
        return null;
    }

    @Override
    public void destroy() {

    }
}
```

上述两种配置方式结果完全一致，将 myservlet与MyServlet进行映射，即在浏览器地址栏中直接访问 myservlet就可以映射到MyServlet。



## Servlet的生命周期

1、当浏览器访问Servlet的时候，Tomcat会查询当前Servlet的实例化对象是否存在，如果不存在，则通过反射机制动态创建对象，如果存在，直接执行第3步。

2、调用init 方法完成初始化操作。

3、调用 service方法完成业务逻辑操作。

4、关闭tomcat时，会调用destory方法，释放当前对象所占用的资源。



Servlet的生命周期方法：无参构造函数、init、service、destory

1、无参构造函数只调用一次，创建对象。
2、init只调用一次，初始化对象。

3、service调用N次，执行业务方法。
4、destory只调用一次，卸载对象。