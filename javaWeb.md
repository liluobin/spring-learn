## Tomcat（servlet和JSP）

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



## ServletConfig

该接口是用来描述Servlet的基本信息的。

getServletName(）返回Servlet的名称，全类名（带着包名的类名）

getlnitParameter（String key）  获取init参数的值（web.xml）

getlnitParameterNames()    返回所有的initParamter的name值，一般用作遍历初始化参数

getServletContext（）返回ServletContext对象，它是Servlet的上下文，整个Servlet的管理者。

ServletConfig作用于某个Servlet实例，每个Servlet 都有对应的ServletConfig，ServletContext作用于整个Web应用，一个Web应用对应一个ServletContext，多个Servlet 实例对应一个ServletContext。

一个是局部对象，一个是全局对象。



## Servlet的层次结构
Servlet--> GenericServlet-->HttpServlet

HTTP请求有很多种类型，常用的有四种：

GET

POST

PUT

DELETE

* 方法extend httpservlet只需重写doGet和doPost两个方法

```java
package com.luobin.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
@WebServlet("/learn")
public class LearnServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("zhixng ");
        resp.setContentType("text/html;charset=UTF-8");
        resp.getWriter().write("我用这个实现了输出");

    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doPost(req, resp);
    }
}
```

GenericServlet实现Servlet接口，同时为它的子类屏蔽了不常用的方法，子类只需要重写 service方法即可。

HttpServlet继承GenericServlet，根据请求类型进行分发处理，GET进入doGET方法，POST进入doPOST方法。

开发者自定义的Servlet 类只需要继承 HttpServlet即可，重新 doGET和doPOST。



## JSP

JSP本质上就是一个Servlet，JSP主要负责与用户交互，将最终的界面呈现给用户HTML+JS+CSS+Java

的混合文件。

当服务器接收到一个后缀是jsp的请求时，将该请求交给JSP引擎去处理，每一个JSP页面第一次被访问的时候，JSP引擎会将它翻译成一个Servlet文件，再由Web 容器调用Servlet完成响应。

单纯从开发的角度看，JSP就是在HTML中嵌入Java程序。

具体的嵌入方式有3种：
1、JSP脚本，执行java逻辑代码

```java
<%java代码%>
```

2、JSP声明：定义Java方法

```java
<%!
	public static void main(){
		...
	}
	注意：只能定义不能调用。
%>
```

3、JSP表达式：把Java对象直接输出到HTML页面中

```java
<%
String str=test();
%>
<%=str%>
```

![image-20200530113948336](.\images\jsp-for循环.png)

jsp将<%%>内的部分作为java代码复制到servlet文件中，将外面的部分写作getWriter().write("xxx")，最后在servlet文件中所有的代码都整合在一起，所以for循环中间可以插入html代码块。



## JSP内置对象9个

* request : 表示一次请求，来自HttpServletRequest

* response：表示一次响应，HttpServletResponse。

* pageContext：页面上下文，获取页面信息，PageContext。

* session：表示一次会话，保存用户信息，一次会话可以有很多request和response，HttpSession。

* application：表示当前Web应用，全局对象，保存所有用户共享信息，ServletContext。

* config：当前JSP对应的Servlet的ServletConfig对象，获取当前Servlet的信息。

* out：向浏览器输出数据，JspWriter。

* page：当前JSP对应的Servlet对象，Servlet。

* exception：表示JSP页面发生的异常，Exception。

常用的是request、response、session、application、pageContext

request常用方法：

1. String getParameter（String key）获取客户端传来的参数

2. void setAttribute（String key，Object value）通过键值对的形式保存数据。
3. Object getAtribute（String key）通过key 取出value。（set/getAtribute用于在服务端页面之间传递参数）
4. RequestDispatcher getRequestDispatcher（String path）返回一个RequestDispatcher对象，该对象的forward 方法用于请求转发。
5. String[] getParameterValues）获取客户端传来的多个同名参数。
6. void setCharacterEncoding（String charset）指定每个请求的编码。



### HTTP请求状态码

200：正常

404：资源找不到

400：请求类型不匹配

500：Java程序抛出异常

## response 常用方法

1、sendRedirect（String path）重定向，页面之间的跳转。

转发getRequestDispatcher 和重定向sendRedirect的区别：

转发是将同一个请求传给下一个页面，重定向是创建一个新的请求传给下一个页面。之前的请求结束生命周期。

重定向：由客户端发送一次新的请求来访问跳转后的目标资源，地址栏改变，也叫客户端跳转。

如果两个页面之间需要通过request 来传值，则必须使用转发，不能使用重定向。



### Session

识别用户会话

服务器无法识别每一次HTTP请求的出处（不知道来自于哪个终端），它只会接受到一个请求信号，所以就存在一个问题：将用户的响应发送给其他人，必须有一种技术来让服务器知道请求来自哪，这就是会话技术。

会话：就是客户端和服务器之间发生的一系列连续的请求和响应的过程，打开浏览器进行操作到关闭浏览器的过程。

会话状态：指服务器和浏览器在会话过程中产生的状态信息，借助于会话状态，服务器能够把属于同一次会话的一系列请求和响应关联起来。

实现会话有两种方式

* session：记录在服务端，关闭浏览器，或者在其他浏览器打开后sessionID会改变：每一次会话生成一个ID
* cookie：记录在客户端



#### session常用的方法：

* String getld（）获取sessionlD
* void setMaxlnactivelnterval（int interval）设置 session的失效时间，单位为秒(网站一般保存七天登录信息)
* int getMaxlnactivelnterval）获取当前 session的失效时间
* void invalidate（）设置 session立即失效

* void setAttribute（String key，Object value）通过键值对的形式来存储数据
* Object getAttribute（String key）通过键获取对应的数据
* void removeAttribute（String key）通过键删除对应的数据



## cookie

Cookie是服务端在HTTP响应中附带传给浏览器的一个小文本文件，一旦浏览器保存了某个Cookie，在之后的请求和响应过程中，会将此Cookie来回传递，这样就可以通过Cookie这个载体完成客户端和服务端的数据交互。

* 创建Cookie

```java
Cookie cookie =new Cookie("name","tom");
response.addCookie(cookie);
```

* 读取Cookie

```java
Cookie[]  cookies=request.getCookies();
for(Cookie cookie: cookies){
	out. write(cookie. getName()+":"+cookie. getValue()+"<br/>");
}
```

Cookie 常用的方法

void setMaxAge（int age）设置Cookie的有效时间，单位为秒

int getMaxAge0   获取Cookie的有效时间,**默认值是-1，关闭浏览器则cookie消失，可以设置cookie关闭浏览器后一定时间内保留**

String getName（）获取Cookie的name

String getValue）获取Cookie的value



#### Session 和Cookie的区别

session：

* 保存在服务器
* 保存的数据是Object
* 会随着会话的结束而销毁
* 保存重要信息

cookie：

* 保存在浏览器

* 保存的数据是String

* 可以长期保存在浏览器中，与会话无关
* 保存不重要信息

session:setAttribute（name，"admin"）存

​                getAttribute（name）取

​                生命周期：服务端：只要WEB应用重启就销毁，客户端：只要浏览器关闭就销毁。

​                退出登录：session.invalidate()





cookie:response.addCookie（new Cookie（name，"admin"）存

```java
Cookie[]  cookies=request.getCookies();
for(Cookie cookie: cookies){
	out. write(cookie. getName()+":"+cookie. getValue()+"<br/>");
}
```

取

​					生命周期：不随服务端的重启而销毁，客户端：默认是只要关闭浏览器就销毁，我们通过 setMaxAge0方法设置有效期，一旦设置了有效期，则不随浏览器的关闭而销毁，而是由设置的时间来决定。

​                    退出登录：setMaxAge（0）

### JSP内置对象作用域

page、request、session、application

setAttribute、getAttribute

page作用域：对应的内置对象是pageContext。

request作用域：对应的内置对象是request。

session作用域：对应的内置对象是session。

application作用域：对应的内置对象是application。

page<request<session < application

page 只在当前页面有效。

request在一次请求内有效。

session在一次会话内有效。

application 对应整个WEB应用的。



### EL表达式

Expression Language表达式语言，替代JSP页面中数据访问时的复杂编码，可以非常便捷地取出域对象（pageContext、request、session、application）中保存的数据，前提是一定要先 setAttribute，EL就相当于在简化getAttribute

${变量名}，变量名就是setAttribute中的key值。

1. EL对于4种域对象的默认查找顺序：

   pageContext->request->session->application

   按照上述的顺序进行查找，找到立即返回，在application中也无法找到，则返回nul

2. 指定作用域进行查找

   pageContext:${pageScope.name}

   request:${requestScope.name}

   session:$(sessionScope.name}

   application:${applicationScope.name}

   * 也可以查看类的级联信息${Student.id}   **注意.id绑定的是getId方法，并不是student实例对象的属性**，等同于getAttribute("Student").getId()

EL执行表达式

&&||!<><=<===,and  or  not  eq  ne   <lt   >gt    <=le   >=ge    empty 

${num1>num2}

${empty num3}



### JSTL

JSP Standard Tag LibraryJSP标准标签库，JSP为开发者提供的一系列的标签，使用这些标签可以完成一些逻辑处理，比如循环遍历集合，让代码更加简洁，不再出现JSP脚本穿插的情况。

实际开发中EL和JSTL结合起来使用，JSTL侧重于逻辑处理，EL负责展示数据。

JSTL的使用

1、需要导入jar包（两个jstl.jar standard.jar）  存放的位置 web/WEB-INF

2、在JSP页面开始的地方导入JSTL标签库

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
```

3、在需要的地方使用

```jsp
<c:forEach items="${list}" var="user">
	<tr>
		<td>S{user.id}</td>
		<td>${user.name}</td>
		<td>S{user.score}</td>
		<td>S{user.address.value}</td>
	</tr>
</c:forEach>
```

JSTL优点：
1、提供了统一的标签
2、可以用于编写各种动态功能



常用标签：

* set、out、remove、catch

set：向域对象中添加数据

```jsp
<c:set var="name" value="tom"></c:set>
等效于：
request.setAttribute("name",tom);

<%
   User user= new User(1,"张三",66.6);
   request.setAttribute("user",user);
%>
不能存入对象，只能修改已存入对象的属性：
<c:set target="${user}" property="name" value="李四“></c:set>
```

out：输出域对象中的数据

```jsp
<c:out value="${name}" default="未定义“></c:out>
```

remove：删除域对象中的数据

```jsp
<c:remove var="name" scope="page"</c:remove>
```

catch：捕获异常

```jsp
<c:catch var="error">
	<%
		int a=10/0;
	%>
</c:catch> 
${error}
```

* 条件标签：if choose

```jsp
<c:set var="num1" value="1"></c:set>
<c:set var="num2" value=1"2"></c:set>
<c:if test="${numl>num2}">ok</c:if>
<c:if test="${numl<num2}">fail</c:if>
<hr/>
<c:choose>
	<c:when test="${numl>num2}">ok</c:when>
	<c:otherwise>fail</c:otherwise>
</c:choose>
```

* 迭代标签：foreach

```jsp
<c:forEach items="${list}" var="str" begin="2" end="3" step="2" varStatus="sta">
	${sta.count}、${str}<br/>
</c:forEach>
```

格式化标签库常用的标签：

```jsp
<%
request. setAttribute("date", new Date());
%>
<fmt: formatDate value="S{ date)" pattern="yyyy-MM-dd HH: mm: ss"></fmt: formatDate><br/>
<fmt: formatNumber value="32145.23434"maxIntegerDigits="2"maxFractionDigits="3">
</fmt: formatNumber>
```

函数标签库常用的标签：

```jsp
<%
request.setAttribute("info","Java,C");
%>
${fn:contains(info,"Python")}<br/>
${fn:startswith(info,"Java")}<br/>
${fn:endsWith(info,"C")}<br/>
${fn:indexof(info,"va")}<br/>
${fn:replace(info,"c","Python")y<br/>
${fn:substring(info,2,3)}<br/>
$(fn:split(info,",")[0]}-${fn:split(info,",")[1]}
```



## 过滤器

Filter 功能：

1、用来拦截传入的请求和传出的响应。

2、修改或以某种方式处理正在客户端和服务端之间交换的数据流。

如何使用？

与使用Servlet类似，Filter是Java WEB提供的一个接口，开发者只需要自定义一个类并且实现该接口即可。

**注意：setCharacterEncoding 只对doPost方法有效，如果是get需要使用：new String(para.getBytes("UTF-8"), "ISO-8859-1");对获取到的中文参数进行处理 **

```java
package com. southwind. filter; import javax. servlet.*;
import javax. servlet.*;
import java. io. IOException;
public class CharacterFilter implements Filter{
@override
	public void doFilter(ServletRequest servletRequest, ServletResponse 		servletResponse,FilterChain filterChain) throws IOException, ServletException{
		servletRequest. setCharacterEncoding("UTF-8");
		filterChain. doFilter(servletRequest, servletResponse);
    }
}
```

web.xml中配置 Filter

```xml
<filter>
<filter-name>charcater</filter-name>
<filter-class>com.southwind.filter.CharacterFilter</filter-class>
</filter>
<filter-mapping>
<filter-name>charcater</filter-name>
<url-pattern>/login</url-pattern>
<url-pattern>/test</url-pattern>
</filter-mapping>
```

注意：doFilter 方法中处理完业务逻辑之后，必须添加filterChain.doFilter（servletRequest，servlettResponse）；否则请求/响应无法向后传递，一直停留在过滤器中。

#### Filter的生命周期

当Tomcat启动时，通过反射机制调用Filter的无参构造函数创建实例化对象，同时调用init 方法实现初始化，doFilter 方法调用多次，当Tomcat服务关闭的时候，调用destory 来销毁Filter对象。

无参构造函数：只调用一次，当Tomcat 启动时调用（Filter一定要进行配置）

init 方法：只调用一次，当Filter的实例化对象创建完成之后调用

doFilter：调用多次，访问Filter的业务逻辑都写在Filter中

destory：只调用一次，Tomcat关闭时调用。

**同时配置多个Filter，Filter的调用顺序是由 web.xml中的配置顺序来决定的，写在上面的配置先调用，因为web.xml是从上到下顺序读取的。**

如果用@WebFilter注解来自动配置，则不能指定filter的执行顺序，会随机执行。

实际开发中Filter的使用场景：

1、统一处理中文乱码。

2、屏蔽敏感词。

3、控制资源的访问权限。



### 文件上传下载

1. jsp

   * input的 type 设置为file

   * form表单的method 设置post，get请求会将文件名传给服务端，而不是文件本身
   * form表单的 enctype 设置multipart/form-data，以二进制的形式传输数据

2. Servlet
   * fileupload 组件可以将所有的请求信息都解析成Fileltem对象，可以通过对Fileltem对象的操作完成上传，面向对象的思想。

### 文件下载

```java
import Java.io.IOException;
import java. io. InputStream;
import java. io. outputStream;
@WebServlet("/download")
public class DownloadServlet extends HttpServlet{
	@override
	protested void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException，IOException{
		String type=req.getParameter（"type"）；String fileName=""；
		switch（type）{
			case"png"：
				fileName="1.png"；
				break；
			case"txt"：
				fileName="test.txt"；
				break；
        }
		//设置响应方式
		resp.setContentType（"application/x-msdownload"）；
		//设置下载之后的文件名
		resp.setHeader（"Content-Disposition"，"attachment；			filename="+fileName）；
        //获取输出流
        OutputStream outputStream=resp.getoutputStream();
        String path=req.getServletContext().getRealPath（"file/"+fileName）；
		InputStream inputStream=new FileInputStream（path）;
        int temp=0；
		while（（temp=inputStream.read（））！=-1）{
			outputStream.write（temp）;
        }
		inputStream.close（）；
		outputStream.close（）；
    }
}
```



### Ajax

Asynchronous JavaScript And XML：异步的JavaScript和XML

AJAX不是新的编程，指的是一种交互方式，异步加载，客户端和服务器的数据交互更新在局部页面的技术，不需要刷新整个页面（局部刷新）

优点：
1、局部刷新，效率更高

2、用户体验更好





## JDBC

Java DataBase Connectivity是一个独立于特定数据库的管理系统，通用的SQL数据库存取和操作的公共接口。

定义了一组标准，为访问不同数据库提供了统一的途径。

![image-20200602230511242](.\images\JDBC.png)

#### JDBC体系结构

JDBC接口包括两个层面：

* 面向应用的API，供程序员调用
* 面向数据库的APl，供厂商开发数据库的驱动程序

JDBCAPI

提供者：Java官方
内容：供开发者调用的接口

java.sql和javax.sql

* DriverManager类
* Connection接口
* Statement接口
* ResultSet接口

DriverManager

提供者：Java官方

作用：管理不同的JDBC驱动

JDBC驱动

提供者：数据库厂商

负责连接不同的数据库

#### JDBC的使用

1、加载数据库驱动，Java程序和数据库之间的桥梁

2、获取Connection，Java程序与数据库的一次连接

3、创建 Statement对象，由Connection产生，执行SQL语句

4、如果需要接收返回值，创建ResultSet对象，保存Statement执行之后所查询到的结果。

