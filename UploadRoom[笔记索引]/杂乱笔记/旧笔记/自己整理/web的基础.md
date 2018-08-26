# web的基础

## 1 两种架构

### (1) C/S

1 Client /Server 客户端 服务器 模式 例子:QQ 大型网络游戏

### (2) B/S

2 Browser / Server 浏览器 服务器 模式 例子:网站 网页游戏

## 2 协议

含义：网络中的计算机，进行网络通信，需要遵循的规范，就叫协议

### [1] TcpIp协议分为四层

![图片1](D:\yitupeixun\自己整理\图片1.png)

### [2] Javaweb遵循的协议

在Javaweb中，客户端和服务器交互遵循的http协议

### [3] http协议

Http协议是一种建立在tcp协议基础上的，基于请求和响应模式(一问一答)的无状态(第一次发出请求和第二次发出气球之间是没有任何关系的，从协议的角度来说--有关系-cookie和Session)的协议

## 3 请求

含义：从客户端发往服务器的信息[字符串]

```
1 请求行		请求的方法/资源/协议的版本 GET(POST)/HTTP/1.1
2 请求头		Host：那个主机 Accept：能接收到什么  Accept-Language接收的语言是什么……
3 空一格
4 消息体 		username=xx&pwd=xx
```

## 4 响应

```
1 状态行		协议版本 状态码 状态字符串 HTTP/1.1 200 ok
2 响应头		Content-Type:内容的类型 Content-length:内容的长度 cookie……
3 空一格		
4 响应正文		<html></html>
```

## 5 浏览器和服务器

含义：1 浏览器是能够发出标准的http请求，解析标准的http响应的软件

​	   2 web服务器是能够接收标准的http请求，返回标注的http响应的软件(Tomcat就是一款免费的开源的纯JAVA实现的WEB服务器.)

## 7 Servlet/jsp容器

```
Servlet/jsp 提供了Servlet的运行环境，管理Servlet的生命周期的一个组件(一对类,零件),一般存在于服务器中。
例如：Tomcat之中就存在一个Servlet容器catAlina
Servlet：Java代码(着重)和HTML标签   jsp：Java代码和jsp标签(着重)
```

##  8 Servlet的核心Api

### [1] javax.servlet.Servlet[接口]

(1) 该接口定义了所有的Servlet必须实现的方法

(2) 方法列表

```
1 init(ServletConfig)--->传递web.xml<init-param> 初始化方法
2 Service(ServletRequest，ServletResponse) 服务方法
3 destroy() 销毁方法
--------------以上统称为生命周期方法
4 getServletConfig()---->读取web.xml中的属性
5 getServletInfo()
```

(3) 含义:Servlet是运行在服务器端的一个小java程序，Servlet接收和响应来自客户端的请求[Http请求]

(4) Servlet的实现 : 可以继承javax.servlet.GenericServlet也可以继承javax.servlet.http.HttpServlet.因为接口在这些之中就已经实现了

(5) 实现的顺序:先构造Servlet，然后调用init()方法初始化，之后将所有的请求交给service()方法处理，处理完毕之后调用destroy方法销毁，等待GC回收

(6) 除了生命周期方法之外该接口还提供了一个getServletConfig方法，Servlet获得一些启动信息.

### [2] javax.setvlet.ServletConfig[接口]

(1) 构造者:容器

(2) 用法:servlet容器使用该对象传递一些启动的参数给Servlet在初始化的时候[以init()方法的参数的形式]

(3) 方法列表：

```
1 getInitParameter(String name){}--读取web.xml中的
2 getInitParameterNames()
3 getServletContext()
4 getServletName()
```

(4) 对应:Servlet 和 ServletConfig是一 一对应的，一个Servlet 都跟着 一个config 对象

### [3] javax.servlet.ServletContext[接口]

(1) 用处：Servlet所在容器通信的一些方法

(2) 方法列表:

```
（1）getRealpath:获得服务器上的绝对路径
（2）getAttrbiute/setAttribute(key,value)
（3）ServletContext与容器对应的。一个ServletContext可以对应多个Servlet.
```

### [4] javax.servlet.GenericServlet[抽象类]

1 这个类实现了Servlet和ServletConfig接口。所以这个类也就实现了这两个接口的8个方法(其中service()方法是抽象的)，还有1其自己定义的两个方法。

方法列表:

```java
1 init(ServletConfig){1 this.config = config;-接入congfig对象 2 初始化加载一些资源…… init()调用空参的}
2 abstract service(ServletRequest,ServletResponse);
3 destroy()
4 getServletConfig()
5 getServletInfo()
6 getInitParameter()
7 getinitParameterName()
8 getServletConfig()
9 getServletName()
10 init(实现初始化-我们自己)
11 log()*2--做日志的
```

### [5] javax.servlet.http.HttpServlet[类]

1 这个类继承了GenericServlet这个抽象类，实现了其全部方法

2 方法列表:

```
1 init(ServletConfig){1 this.config = config;-接入congfig对象 2 初始化加载一些资源…… init()调用空参的}
2 public service(ServletRequest req ,ServletResponse resp){
  HttpServletRequest request =()req;--将ServletRequest强转成了HttpServletRequest
  HttpServletResponse亦一样
  protected.service（）
}--分发客户端的请求给protected service
3 destroy()
4 getServletConfig()
5 getServletInfo()
6 getInitParameter()
7 getinitParameterName()
8 getServletConfig()
9 getServletName()
10 init(实现初始化-我们自己)
11 log()*2--做日志的
12 protected service( HttpServletRequest Request,HttpServletResponse Response){
		String method = request.getMethod()/GET POST--接收getmethod
		If(“get”.equals(method))doGEt---挨个进行判断
		If”POST....)doPost....		
}--接收标准的请求从service，分发给doXXX方法
13 doXXX()--我们重写doXX方法
```

### [7] Http.Servlet.http.HttpServletRequest/Http.Servlet.http.HttpServletResoponse

这是一个请求，一个响应--》3和4

## 9 API总结

```
API名字	    	实现者{}   			调用者.			构造者new
Servlet				我们					容器				容器
ServletConfig    	容器					我们				容器
ServletContext   	容器					我们				容器				
Request        		容器					我们				容器			
Respons				同上
RequestDispatcher 	容器					我们				容器
Cookie				容器					我们				我们				
HttpSession       	容器					我们				容器
Filter 				我们					容器 				容器					
XxListener			我们					容器				容器					
```

## 10 获得ServletContext

```
1．ServletConfig.getServletContext()
2．HttpSession.getServletContext();
3．FilterConfig.getServletContext();
4．ServletContextEvent.getServletContext()
```

## 11 过程

```
1 服务器启动	
	1 服务器检测自身的配置文件，如果文件缺失，则开启失败
	2 Tomcat检测webapps下的web.xml文件，有问题，报错，但正常开启
2 浏览器输入地址，回车
3 浏览器将地址信息打包成标准的http请求字符串，发送到服务器
4 服务器接受并解析请求，将信息打包到HttpServletRequest/Response中
5 到web.xml中寻找指定的url-pattern,根据Servlet-name找到Servlet-class[找到处理类]
6 容器检测内存中是否具有该Servlet对象，若有，则使用原来的，如果没有则利用反射创造新的
7 容器调用init()方法-->进行初始化
8 容器调用service(ServletRequest)--》service（HttpServletRequest）--》doXXX
10 我们重写doXXX方法，完诚业务逻辑
11 容器将Response对象转换成标准的http响应格式，通过网络返回给浏览器
12 浏览器接受响应，解析，显示
```

