# JaveWeb

## 1.基本概念





## 2.Tomcat

### 1.Tomcat下载

官方网站：https://tomcat.apache.org/download-90.cgi
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201231130434823.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjMwNTY3Mg==,size_16,color_FFFFFF,t_70)
根据自己的系统下载对应的版本即可。

### 2.解压到对应目录下

一般解压后的文件里有下面的这些文件夹，他们的功能（见图）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201231130544515.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjMwNTY3Mg==,size_16,color_FFFFFF,t_70)

### 3.启动、关闭Tomcat

打开bin文件夹，在文件中找到startup.bat（启动）和shutdown.bat（关闭）两个文件，直接双击就可以了！
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201231130750360.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjMwNTY3Mg==,size_16,color_FFFFFF,t_70)
**输入http://localhost:8080/就可以成功，8080是默认的端口**
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020123113140893.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjMwNTY3Mg==,size_16,color_FFFFFF,t_70)
**可能遇到的问题：**

1. java环境变量没有配置
2. 闪退问题：需要配置兼容性
3. 乱码问题：配置文件中设置

### 4.服务器配置

可以配置启动的端口号，可以配置主机的名称
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201231131620783.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjMwNTY3Mg==,size_16,color_FFFFFF,t_70)
端口号：

```xml
    <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
```

**tomcat的默认端口号为：8080
mysql：3306
http：80
https：443**

**可以配置主机名称**

```xml
 <Host name="localhost"  appBase="webapps" unpackWARs="true" autoDeploy="true">
```

## 3.Maven

### 1.为什么要学习Maven？

  1.在javaweb开发中，需要导入很多的jar包，我们需要手动去导入。

  2.如何能够让一个东西自动帮我们导入和配置这个jar包

​     由此Maven诞生了！

Maven项目架构管理工具：方便导入jar包，Maven的核心思想：**约定大于配置**

### 2.Maven下载

https://maven.apache.org/download.cgi

![image-20210106210108017](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210106210108017.png)

### 3.解压

![image-20210106210256065](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210106210256065.png)

![image-20210106210354955](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210106210354955.png)

### 4.配置环境变量

在系统环境变量中进行如下配置：

-  M2_HOME Maven目录下的bin目录

- MAVEN_HOME maven的目录
- 在系统的Path中配置%MAVEN_HOME%\bin

在cmd下使用mvn-version查看Maven的安装情况。

![image-20210106210956618](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210106210956618.png)

### 5修改一下配置文件

conf->setting

- 镜像：mirror 作用：加速下载

- 国内建议使用阿里云的镜像

```xml
<mirror>
        <id>nexus-aliyun</id>
        <mirrorOf>*,!jeecg,!jeecg-snapshots</mirrorOf>
        <name>Nexus aliyun</name>
        <url>http://maven.aliyun.com/nexus/content/groups/public</url> 
</mirror>
```

![image-20210106210535435](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210106210535435.png)

![image-20210106211838147](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210106211838147.png)

### 6.本地仓库

本地仓库，远程仓库；

**建立一个本地仓库：localRepository**

```xml
<localRepository>D:\maven\apache-maven-3.6.3\maven-repo</localRepository>
```

![image-20210106212213970](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210106212213970.png)

## 4.Servlet

### 4.1servlet简介

Servlet就是Sun公司开发动态web的一门技术
Sun在这些API中提供一个接口叫做：Servlet，如果你想开发一个Servlet程序，只需要完成两个小步骤：
	编写一个类，实现Servlet接口
	把开发好的java类部署到web服务器中

把实现了Servlet接口的java程序叫做Servlet

### 4.2HelloServlet

Servlet接口在Sun公司有两个默认的实现类：HttPServlet，这里用到了HTTPServlet。

1.构建一个普通Maven项目，删掉里面的Src目录，以后我们在学习的时候就在Module;这个空的工程就是Maven主工程。
2.关于maven父子工程的理解：

   父项目中有一个model

```xml
<modules>
    <module>Servlet-01</module>
</modules>
```

子项目中有一个model

```xml
<parent>
    <artifactId>JavaWeb-03-servlet</artifactId>
    <groupId>org.example</groupId>
    <version>1.0-SNAPSHOT</version>
</parent>
```

父项目的jar包，子项目可以直接使用

3.Maven环境优化

   1.修改web.xml为最新的

   2.将Maven的结构搭建完整

4.编写一个Servlet程序

   1.编写一个普通类

![image-20210106182641170](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210106182641170.png)

​    2.实现Servlet接口，这里直接继承HTTPServlet，GenericServlet

```java
package com.zz.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

public class HelloWorld extends HttpServlet {
    //由于get或者post
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //super.doGet(req, resp);
        PrintWriter writer = resp.getWriter();//响应流
        writer.print("hello,servlet");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doPost(req, resp);
    }
}
```

5.编写Servlet的映射

  为什么需要映射，我们写的是java程序，但是要通过浏览器访问，而浏览器需要连接web服务器，所以我们需要在web服务中注册我们写的Servlet，还需要给他一个浏览器能够访问的路径；

6.配置Tomcat

### 4.3Servlet原理

4.4Mapping问题

1.一个Servlet可以指定一个映射路径

```xml
<servlet-mapping>
  <servlet-name>hello</servlet-name>
  <url-pattern>/hello</url-pattern>>
</servlet-mapping>
```

2.一个Servlet可以指定多个映射路径

3.一个Servlet可以指定通用映射路径

```xml
<servlet-mapping>
  <servlet-name>hello</servlet-name>
  <url-pattern>/hello/*</url-pattern>>
</servlet-mapping>
```

4.默认请求路径

```xml
<servlet-mapping>
  <servlet-name>hello</servlet-name>
  <url-pattern>/*</url-pattern>>
</servlet-mapping>
```

5.指定一些后缀或者前缀等等...



6.优先级问题

指定了固有的映射路径优先级最高，如果找不到就会走默认的处理请求；

### 4.4ServletContext

####  1.共享数据



#### 2.获取初始化参数

在对应的web.xml中添加

```xml
<context-param>
  <param-name>url</param-name>
  <param-value>jbdc:localhost//3036.com</param-value>
</context-param>
```

获取url中的数据

```java
@Override
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    ServletContext content = this.getServletContext();//获取共享区域
    
    String url = content.getInitParameter("url");//获取参数
    resp.getWriter().print(url);//输出参数
}

@Override
protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    doGet(req, resp);
}
```

#### 3.请求转发

![image-20210108193806041](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210108193806041.png)

```java
public class zf extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletContext context = this.getServletContext();

        System.out.println("进入了Servlet-04");
        /*RequestDispatcher requestDispatcher = context.getRequestDispatcher("/gp");//转发的地址
        requestDispatcher.forward(req,resp);*/
        context.getRequestDispatcher("/gp").forward(req,resp);
    }//转发的地址

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

```xml
<servlet>
  <servlet-name>zf</servlet-name>
  <servlet-class>com.zz.Helloservlet.zf</servlet-class>
</servlet>
<!--给上门的Servlet实例，提供一个外界可以访问的地址-->
<servlet-mapping>
  <servlet-name>zf</servlet-name>
  <url-pattern>/zf</url-pattern>>
</servlet-mapping>
```

#### 4.读取资源文件

Properties

- 在java目录下新建properties
- 在resource目录下新建properties

发现，都没打包到同一个路径下：classes我们俗称为classpath。

思路：需要一个文件流

```properties
username=zz123
password=123456
```

```java
public class servletdemo01 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletContext context = this.getServletContext();

        InputStream is = context.getResourceAsStream("/WEB-INF/classes/db.properties");//服务器上的相对地址，也就是到target中找
        Properties preo = new Properties();
        preo.load(is);
        String username = preo.getProperty("username");
        String password = preo.getProperty("password");
        resp.getWriter().print(username+":"+password);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

### 4.5HttpServletResponse

web服务器收到客户端的http请求，针对这个请求，分别创建代表请求的HttpServletRequest对象，代表响应的一个HttpServletResponse；

- 如果要获取客户端请求过来的参数：找HttpServletRequest
- 如果要给客户端响应一些信息：找HttpservletRequest

#### 1.简单分类

负责向浏览器发送数据的方法

```java
ServletOutputStream getOutputStream() throws IOException;

PrintWriter getWriter() throws IOException;
```

负责向浏览器发送响应头的方法

```java
void setCharacterEncoding(String var1);

void setContentLength(int var1);

void setContentLengthLong(long var1);

void setContentType(String var1);

void setDateHeader(String var1, long var2);

void addDateHeader(String var1, long var2);

void setHeader(String var1, String var2);

void addHeader(String var1, String var2);

void setIntHeader(String var1, int var2);

void addIntHeader(String var1, int var2);
```

响应的状态码

200：请求响应成功 3：重定向 4xx：错误，找不到资源404  500：服务器代码错误 502：网关错误

```java
int SC_CONTINUE = 100;
int SC_SWITCHING_PROTOCOLS = 101;
int SC_OK = 200;
int SC_CREATED = 201;
int SC_ACCEPTED = 202;
int SC_NON_AUTHORITATIVE_INFORMATION = 203;
int SC_NO_CONTENT = 204;
int SC_RESET_CONTENT = 205;
int SC_PARTIAL_CONTENT = 206;
int SC_MULTIPLE_CHOICES = 300;
int SC_MOVED_PERMANENTLY = 301;
int SC_MOVED_TEMPORARILY = 302;
int SC_FOUND = 302;
int SC_SEE_OTHER = 303;
int SC_NOT_MODIFIED = 304;
int SC_USE_PROXY = 305;
int SC_TEMPORARY_REDIRECT = 307;
int SC_BAD_REQUEST = 400;
int SC_UNAUTHORIZED = 401;
int SC_PAYMENT_REQUIRED = 402;
int SC_FORBIDDEN = 403;
int SC_NOT_FOUND = 404;
int SC_METHOD_NOT_ALLOWED = 405;
int SC_NOT_ACCEPTABLE = 406;
int SC_PROXY_AUTHENTICATION_REQUIRED = 407;
int SC_REQUEST_TIMEOUT = 408;
int SC_CONFLICT = 409;
int SC_GONE = 410;
int SC_LENGTH_REQUIRED = 411;
int SC_PRECONDITION_FAILED = 412;
int SC_REQUEST_ENTITY_TOO_LARGE = 413;
int SC_REQUEST_URI_TOO_LONG = 414;
int SC_UNSUPPORTED_MEDIA_TYPE = 415;
int SC_REQUESTED_RANGE_NOT_SATISFIABLE = 416;
int SC_EXPECTATION_FAILED = 417;
int SC_INTERNAL_SERVER_ERROR = 500;
int SC_NOT_IMPLEMENTED = 501;
int SC_BAD_GATEWAY = 502;
int SC_SERVICE_UNAVAILABLE = 503;
int SC_GATEWAY_TIMEOUT = 504;
int SC_HTTP_VERSION_NOT_SUPPORTED = 505;
```

#### 2.常见应用

1、向浏览器输出消息

2、下载文件 

​	1.获取下载文件的路径

​	2.下载文件名是啥？

​	3.想办法让浏览器能够支持下载我们需要的东西

​	4.获取下载文件的输入流

​	5.创建缓冲区

​	6.获取OutputStream对象

​	7.将FileOutputStream流写入到buffer缓冲区

​	8.使用OutputStream将缓冲流中的数据输出到客户端

3、验证码功能

验证码怎么来的？

​	前端实现

​	后端实现，需要用到java的图片类，生成一个图片

```java
package com.zz.servlet;

import javax.imageio.ImageIO;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.awt.*;
import java.awt.image.BufferedImage;
import java.io.IOException;
import java.util.Random;

public class Image extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //super.doGet(req, resp);
        //如何让浏览器5s刷新一次；
        resp.setHeader("refresh","3");

        //在内存中创建一个图片
        BufferedImage bufferedImage = new BufferedImage(80, 20,BufferedImage.TYPE_INT_RGB);

        //得到图片
       Graphics2D g = (Graphics2D) bufferedImage.getGraphics();//笔

        //设置图片的背景颜色
        g.setColor(Color.white);
        g.fillRect(0,0,80,20);

        //给图片写数据
        g.setColor(Color.BLUE);
        g.setFont(new Font(null,Font.BOLD,20));
        g.drawString(makeNum(),0,20);

        //告诉浏览器，这个请求用图片的方式打开
        resp.setContentType("image/jpg");

        //网站存在缓存，不让浏览器缓存
        resp.setDateHeader("expires",-1);
        resp.setHeader("Cache-Control","no-cache");
        resp.setHeader("Pragma","no-cache");

        //把图片写给浏览器
        boolean write = ImageIO.write(bufferedImage,"jpg", resp.getOutputStream());

    }
    //生成随机数
    private String makeNum(){
        Random random = new Random();
        String num = random.nextInt(9999999)+"";
        StringBuffer SB = new StringBuffer();
        for(int i=0;i<7-num.length();i++){
            SB.append("0");
        }
        num = SB.toString()+num;
        return  num;
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doPost(req, resp);
    }
}
```

4.实现重定向

一个web资源收到客户端请求，他会通知客户端去访问另外一个web资源C，这个过程叫重定向。

常见场景：

​	用户登录

```java
	void sendRedirect(String var1) throws IOException;
```



### 4.6HttpServletRequest

**HttpServletRequest**代表客户端的一个请求，用户通过Http访问服务器，Http请求中的所有信息会被封装到**HttpServletRequest**

1、获取前端传递的参数，请求转发

```java
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    req.setCharacterEncoding("utf-8");

    String username = req.getParameter("username");
    String password = req.getParameter("password");
    String[] hobbys = req.getParameterValues("hobbys");


    System.out.println("===================");
    System.out.println(username);
    System.out.println(password);
    System.out.println(Arrays.toString(hobbys));

    //通过请求转发
 req.getRequestDispatcher(req.getContextPath()+"/success.jsp").forward(req,resp);
    req.setCharacterEncoding("utf-8");
}
```

2请求转发和重定向的区别

面试题：请你聊聊重定向和转发的区别？

相同点

​	页面都会实现跳转

不同点

- ​	请求转发的时候，url不会产生变化 ；307
- ​	重定向的时候，url地址栏会发生变化；302

## 5.Session、Cookie

### 5.1会话

会话：用户打开一个浏览器，点击很多超链接，访问多个web资源，关闭浏览器，这个过程可以称之为会话；

有状态会话：一个同学来过教室，下次再来教室，我们会知道这个同学曾经来过教室，称为有状态会话；



客户端         服务端

1.服务端给客户一个信件，客户端下次访问服务器带上信件就可以了；Cookie

2.服务器登记你来过了，下次你来的时候我来匹配你；Seesion

### 5.2保存会话的两种技术

Cookie：

- 客户端技术（响应，请求）



Session：

服务器技术，利用这个技术，可以保存用户的会话信息，我们可以把信息或者数据放在Session中！

5.3Cookie

1.从请求中拿到Cookie信息

2.服务器响应给客户端Cookie

```java
Cookie[] cookie = req.getCookies();//这里返回数组，说明Cookie可能存在多个

cookie.getName();//获得cookie中的key

cookie.getValue();//获得cookie的值

new Cookie("lastLoginTime",System.currentTimeMillis()+"");//新建一个cookie
cookie.setMaxAge(24*60*60)//设置cookie的有效期
resp.addCookie(cookie);//响应给客户端一个cookie
```



**Cookie:一般会保存在本地，用户目录下appdata**

一个网站cookie是否存在上限！聊聊细节

- 一个cookie只能保存一个信息；
- 一个web站点可以给浏览器发送多个cookie，300个浏览器上限
- Cookie大小有限制4kb；
- 300个cookie浏览器上限

删除Cookie

- 不设置有效期，关闭浏览器，自动失效！



![image-20210118110323194](C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20210118110323194.png)

### 5.3Session（重点）

![image-20210118110805102](C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20210118110805102.png)

什么是Session：

- ​	服务器会给每一个用户（浏览器），创建一个Session对象；
-  	一个Session独占一个浏览器，只要浏览器没有关闭，这个Session就存在；
- ​	用户登录之后，整个网站都可以使用访问！-》保存用户的信息；保存购物车的信息... ...



Session与Cookie的区别

- ​	Cookie是把用户的数据写给用户的浏览器，浏览器保存
- ​	Session把用户的数据写到用户独占Session中，服务器端保存（保存重要的信息，减少服务器资源的浪费）
- ​	Session对象由服务器创建；

使用场景：

- ​	保存一个用户登录的信息；
- ​	购物车信息；
- 在整个网站中经常会使用的数据，我们将它保存在Session中；

使用Session：

```java
public class Sessiondemo01 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //super.doGet(req, resp);
        //解决乱码问题
        req.setCharacterEncoding("utf-8");
        resp.setCharacterEncoding("utf-8");
        resp.setContentType("text/html;charset=utf-8");//浏览器响应的格式

        //得到Session
        HttpSession session = req.getSession();
        //给Session中村内容
        session.setAttribute("name",new person("猪头",19));

        //获得Sessionde Id
        String id = session.getId();

        //判断Session是不是新的
        if(session.isNew()){
            resp.getWriter().write("session创建成功，Id:"+id);
        }else{
            resp.getWriter().write("session已经在服务器中存在，ID："+id);
        }
        //session创建的时候做什么：

    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}


//得到Session
HttpSession session = req.getSession();
person person =(person) session.getAttribute("name");
System.out.printf(person.toString());


//注销Session
HttpSession session = req.getSession();
session.removeAttribute("name");
session.invalidate();//注销
```



会话自动过期：web.xml

```xml
<!--设置session的存在时间-->
<session-config>
  <!--以分钟为单位-->
  <session-timeout>1</session-timeout>
</session-config>
```

![image-20210118111213929](C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20210118111213929.png)





## 6.JSP

### 6.1、什么是JSP

Java Server Pages:Java服务器端页面，也和Servlet一样

最大的特点：

- ​	写Jsp就像在写html

- ​	区别：
  - ​		html只给用户提供静态的数据
  - ​		jsp页面可以嵌入Java代码，为用户提供动态数据；

### 6.2、JSP原理

思路：JSP到底怎么执行的！

- ​	代码层面没有任何问题
- ​	服务器内部工作
  - tomcat中有一个work目录
  - 在Idea中使用Tomcat

![image-20210118180648001](C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20210118180648001.png)

我的电脑在

```
C:\Users\Administrator\AppData\Local\JetBrains\IntelliJIdea2020.3\tomcat\0be1abc6-3388-496c-90a2-7025c376fc48\work\Catalina\localhost\SessionCookie_01_war\org\apache
```

发现页面转变成了Java程序！

![image-20210118180752111](C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20210118180752111.png)

浏览器向服务器发送请求，不管访问什么资源，其实都是访问Servlet

JSP最终也会被转化为一个java类！

JSP本质上就是一个Servlet

```java
//初始化
public void _jspInit() {
  }
//销毁
  public void _jspDestroy() {
  }
//JSPService
  public void _jspService(final javax.servlet.http.HttpServletRequest request, final javax.servlet.http.HttpServletResponse response)
      throws java.io.IOException, javax.servlet.ServletException {
```

1.判断请求

2.内置了一些对象

```java
final javax.servlet.jsp.PageContext pageContext;//页面上下文
javax.servlet.http.HttpSession session = null;//Session
final javax.servlet.ServletContext application;//application
final javax.servlet.ServletConfig config;//config
javax.servlet.jsp.JspWriter out = null;//out
final java.lang.Object page = this;
javax.servlet.jsp.JspWriter _jspx_out = null;//page 当前
javax.servlet.jsp.PageContext _jspx_page_context = null;//响应
```



3.输出页面前增加的代码

```java
response.setContentType("text/html");
pageContext = _jspxFactory.getPageContext(this, request, response,null, true, 8192, true);
_jspx_page_context = pageContext;
application = pageContext.getServletContext();
config = pageContext.getServletConfig();
session = pageContext.getSession();
out = pageContext.getOut();
_jspx_out = out;
```

4.以上的这些个对象我们都可以在JSP页面中使用

在JSP页面中：

只要是Java代码就会原封不动的输出

如果是html代码，就会被转化为

```
<%
	Java代码块
%>
out.write("name")
```



### 6.3、JSP基础语法

任何语言都有自己的语法，Java中，Jsp作为java技术的一种应用，它拥有一些自己扩充的语法，java所有语法都支持！



**jsp表达式**

```jsp
<%--jsp表达式
作用：用来将程序的输出，输出到客户端
<%= 变量或者表达式%>
--%>
<%= new java.util.Date()%>
```



**jsp脚本片段**

```jsp
<%--jsp脚本片段--%>
<%
    int sum = 0;
    for(int i=1;i<=100;i++){
        sum+=i;
    }
    out.println("<h1>Sum="+sum+"</h1>");
%>
```



脚本片段的再实现

```jsp
<%--在代码嵌入html元素--%>
<%
    for(int i=0;i<5;i++){


%>
<h1>hello world <%=i%></h1>
<%
    }
%>
```

JSP声明

```jsp
<%!
    static {
        System.out.println("loding servlet");
    }
    private int globalVar = 0;
    public void zz(){
        System.out.println("进入了方法zz");
    }

%>
```

jsp声明：会被编译到jsp生成的java类中！其他的，就会被生成到_jspService方法中！

在jsp，嵌入java代码即可！



```jsp
<%%>
<%=%>
<%!%>
<%--注释--%>
```

jsp的注释不会在客户端显示，html就会！

### 6.4JSP指令

```jsp
<%@ page args...%>
<%@ include file...%>

<%--@include会将两个页面合二为一--%>
<%@include file = "common/header.jsp"%>
<h1>网页主体</h1>
<%@include file = "common/footer.jsp"%>

<hr>

<%--jsp标签
    Jsp：include:拼接页面，本质还是三个页面
--%>
<jsp:include page="/common/header.jsp"/>
<h1>网页主体</h1>
<jsp:include page="/common/footer.jsp"/>
```

### 6.5、9大内置对象

- PageContext 存东西
- Request 存东西
- Response 
- Session 存东西
- Application[ServletContext]存东西
- config[ServletConfig]
- out
- page 不了解
- exception

```java
pageContext.setAttribute("name1","zz1");//保存的数据只在一个页面中有效
request.setAttribute("name2","zz2");//保存的数据只在一次请求中有效，请求转发会携带这个数据
session.setAttribute("name3","zz3");//保存的数据只在一次会话中有效，从打开浏览器到关闭浏览器
application.setAttribute("name4","zz4");//保存的数据只在服务器中有效，从打开服务器到关闭服务器
```



request:客户端向服务器发送请求，产生的数据，用户看完就没用了，比如：新闻，用户看完没用的!

session:客户端向服务器发送请求，产生的数据，用户用完一会还有用，比如购物车

application：客户端向服务器发送请求，产生的数据，一个用户用完了，其他用户还可以使用，比如：聊天数据；

### 6.6、JSP标签、JSTL标签、EL表达式

```xml
<!--JSTL表达式-->
<dependency>
    <groupId>javax.servlet.jsp.jstl</groupId>
    <artifactId>javax.servlet.jsp.jstl-api</artifactId>
    <version>1.2.2</version>
</dependency>

<!-- https://mvnrepository.com/artifact/taglibs/standard -->
<!--standard标签库-->
<dependency>
    <groupId>taglibs</groupId>
    <artifactId>standard</artifactId>
    <version>1.1.2</version>
</dependency>
```

**EL表达式：**${}

- ​	获取数据
- ​	执行运行
- ​	获取web开发的常用对象

JSP标签

```jsp
<%--jsp:include--%>
<%	
	http://..
%>
<jsp:forward page="/jsptage2.jsp">
	<jsp:param name="name" value="kzz"><jsp:param>
	<jsp:param name="age" value="12"></jsp:param>
	</jsp:forward>
```

**JSTL表达式**

> JSTL标签库的使用就是为了弥补html标签的不足；它自定义许多标签，可以供我们使用，标签的功能和java代码一样！

核心标签

格式化标签

SQL标签

XML标签





## 7.JavaBean

实体类

JavaBean有特定的写法：

- ​	必须要有一个无参构造
- ​	属性必须私有化
- ​	必须有对应的get/set方法

一般用来和数据库的字段做映射ORM：

​	ORM：对象关系映射

- ​	表---类
- 字段-->属性
- 行记录--->对象

| id   | name | age  | address |
| ---- | ---- | ---- | ------- |
| 1    | zz1  | 18   | 西安    |
| 2    | zjj  | 20   | 北京    |
| 3    | zj   | 19   | 南京    |

```java
class person{
    private int id;
    private String name;
    private int age;
    private Straing address;
}
```

## 8.三层架构MVC

### 8.1早些年

什么是MTV：model view controller

![image-20210120174933083](C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20210120174933083.png)用户直接访问控制层，控制层就可以直接操作数据库；

```
servlet--CRUD-->数据库
弊端：程序十分臃肿，不利于维护 
Servlet的代码中：处理请求、响应、视图跳转、处理JDBC、处理业务代码、处理逻辑代码

架构：没有什么是加一层解决不了的！
程序猿调用
|
JDBC
|
MySQL Oracle ..
```

### 8.2MVC三层架构

![image-20210120181405312](C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20210120181405312.png)

Model

- ​	业务处理：业务逻辑
- ​	数据持久层：CRUD（Dao）

View

- ​	展示数据
- ​	提供链接发起servlet请求

Contrller(Servlet)

- ​	接收用户的请求：req请求参数、Session信息...
- ​	交给业务层处理对应的代码
- ​	控制视图的跳转

```java
登录--->接收用户的登录请求--->处理用户的请求（获取用户登录的参数，username，password）--->交给业务层处理登录业务（判断用户名是否正常：事务）--->Dao层查询用户名和密码是否正确-->数据库
```

## 9.过滤器Filter(重点)

步骤:

1.导包

2.编写过滤器

​	1.导包不要导错

![image-20210120190507366](C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20210120190507366.png)

​	2.实现filter接口，重写对应的方法

```java
package com.zz.Filter;

import javax.servlet.*;
import java.io.IOException;

public class SetCharacterencoding implements Filter {
    //初始化：web服务器启动，就已经初始化了，随时等待过滤对象出现
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("characterenconding初始化");
    }


    //chain：链
    /*1.过滤中的所有代码，在过滤特定请求的时候都会执行
    2.必须要让过滤器继续同行
    chain.doFilter(request,response);
    * */
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        servletRequest.setCharacterEncoding("utf-8");
        servletResponse.setCharacterEncoding("utf-8");
        servletResponse.setContentType("text/html;charset=uff-8");

        System.out.println("characterencodingfilter执行前....");
        filterChain.doFilter(servletRequest,servletResponse);//让我们的请求继续走，如果不写，程序到这就背拦截停止了
        System.out.println("characterencodingfilter执行后.....");
    }

    @Override
    //销毁：web服务器关闭的时候，过滤会销毁
    public void destroy() {
        System.out.println("characterencodingfilter销毁");
    }
}
```

​	3.在web.xml中配置

```xml
<filter>
    <filter-name>guolv</filter-name>
    <filter-class>com.zz.Filter.SetCharacterencoding</filter-class>
</filter>
<filter-mapping>
    <filter-name>guolv</filter-name>
    <url-pattern>/servlet/*</url-pattern>
</filter-mapping>
```

## 10.监听器

实现一个监听器的接口；有很多

1.编写一个监听器

  	实现监听器的接口

```java
package com.zz.listener;

import javax.servlet.ServletContext;
import javax.servlet.http.HttpSessionEvent;
import javax.servlet.http.HttpSessionListener;

public class OnlinCounterlistener implements HttpSessionListener{
    @Override
    //创建session监听 一旦创建session就会触发一次事件
    public void sessionCreated(HttpSessionEvent se) {
        ServletContext ctx = se.getSession().getServletContext();
        Integer onlineCount = (Integer)ctx.getAttribute("OnlineCount");

        if(onlineCount == null){
            onlineCount = new Integer(1);
        }else {
            int count = onlineCount.intValue();
            onlineCount = new Integer(count+1);
        }
        ctx.setAttribute("OnlineCount",onlineCount);
    }

    @Override
    //销毁session监听
    public void sessionDestroyed(HttpSessionEvent se) {
        ServletContext ctx = se.getSession().getServletContext();
        Integer onlineCount = (Integer)ctx.getAttribute("OnlineCount");

        if(onlineCount == null){
            onlineCount = new Integer(0);
        }else {
            int count = onlineCount.intValue();
            onlineCount = new Integer(count-1);
        }
        ctx.setAttribute("OnlineCount",onlineCount);
    }
}
```

2.配置监听器，在web.xml中

```xml
<listener>
    <listener-class>com.zz.listener.OnlinCounterlistener</listener-class>
</listener>
```

3.看情况是否使用！