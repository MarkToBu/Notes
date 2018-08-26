# Servlet

## 前言

```\
Servlet:

是一种独立于平台和协议的服务器端的Java应用程序，可以生成动态的Web页面。 它担当Web浏览器或其他HTTP客户程序发出请求，与HTTP服务器上的数据库或应用程序之间的中间层。

　　Servlet是位于Web 服务器内部的服务器端的Java应用程序，与传统的从命令行启动的Java应用程序不同，Servlet由Web服务器进行加载，该Web服务器必须包含支持Servlet的Java虚拟机。

servlet由来
　　servlet是在服务器上运行
的小程序。这个词是在Java applet的环境中创造的，Java applet是一种当作单独文件跟网页一起发送的小程序，它通常用于在客户端运行，结果得到为用户进行运算或者根据用户互作用定位图形等服务。　
　　服务器上需要一些程序，常常是根据用户输入访问数据库的程序。这些通常是使用公共网关接口（CGI）应用程序完成的。然而，在服务器上运行Java，这种程序可使用Java编程语言实现。在通信量大的服务器上，Java servlet的优点在于它们的执行速度更快于CGI程序。各个用户请求被激活成单个程序中的一个线程，而创建单独的程序，这意味着各个请求的系统开销比较小。


WEB服务器:
	也称为WWW(WORLD WIDE WEB)服务器，主要功能是提供网上信息浏览服务。 
(1)应用层使用HTTP协议。 (2)HTML文档格式。 (3)浏览器统一资源定位器(URL)。  
　
　　使用最多的 web server 服务器软件 有两个：微软的信息服务器（iis），和Apache。


　　通俗的讲，Web服务器传送(serves)页面使浏览器可以浏览，然而应用程序服务器提供的是客户端应用程序可以调用(call)的方法(methods)。确切一点，你可以说：Web服务器专门处理HTTP请求(request)，但是应用程序服务器是通过很多协议来为应用程序提供(serves)商业逻辑(business logic)。


　　Web服务器可以解析(handles)HTTP协议。当Web服务器接收到一个HTTP请求(request)，会返回一个HTTP响应(response)，例如送回一个HTML页面。为了处理一个请求(request)，Web服务器可以响应(response)一个静态页面或图片，进行页面跳转(redirect)，或者把动态响应(dynamic response)的产生委托(delegate)给一些其它的程序例如CGI脚本，JSP(JavaServer Pages)脚本，servlets，ASP(Active Server Pages)脚本，服务器端(server-side)JavaScript，或者一些其它的服务器端(server-side)技术。无论它们(译者注：脚本)的目的如何，这些服务器端(server-side)的程序通常产生一个HTML的响应(response)来让浏览器可以浏览。


　　要知道，Web服务器的代理模型(delegation model)非常简单。当一个请求(request)被送到Web服务器里来时，它只单纯的把请求(request)传递给可以很好的处理请求(request)的程序(译者注：服务器端脚本)。Web服务器仅仅提供一个可以执行服务器端(server-side)程序和返回(程序所产生的)响应(response)的环境，而不会超出职能范围。服务器端(server-side)程序通常具有事务处理(transaction processing)，数据库连接(database connectivity)和消息(messaging)等功能。


　　虽然Web服务器不支持事务处理或数据库连接池，但它可以配置(employ)各种策略(strategies)来实现容错性(fault tolerance)和可扩展性(scalability)，例如负载平衡(load balancing)，缓冲(caching)。集群特征(clustering—features)经常被误认为仅仅是应用程序服务器专有的特征。


web容器
   容器：充当中间件的角色(所谓容器就是指符合一定的规范能提供一系列服务的管理器，方便别人使用它来完成一系列的功能)

   WEB容器：给处于其中的应用程序组件（JSP，SERVLET）提供一个环境，使JSP、SERVLET直接更容器中的环境变量接口交互，不必关注其它系统问题。主要有WEB服务器来实现。例如：TOMCAT,WEBLOGIC,WEBSPHERE等。该容器提供的接口严格遵守J2EE规范中的WEB APPLICATION 标准。我们把遵守以上标准的WEB服务器就叫做J2EE中的WEB容器。

	例如tomcat，使用tomcat可以为我们提供servlet、jsp等服务，我们俗称叫servlet服务器，在服务器中会有相关的容器，servlet容器可以调用servlet和jsp动态的为我们生成html

对于刚刚接触的人来说，可以把服务器就理解成一个容器也可以，不过两者的确不是一回事，是服务器为我们提供一个容器使我们的程序能够在容器里运行，使用服务器提供的一系列功能 
```



##  1.0 Server+Applet 简介

~Servlet~
~一个运行在服务器端的小程序~一个普通的类，如果继承了HttpServlet抽象类。则这个类我们称之为Servlet类。



CS开发模式和BS开发模式

client-server
	客户端服务器模式

CS:用户需要在本地安装一个客户端，不依赖网络，客户端
自带数据库等，一切都安装在本地，通过功能丰富的客户端来
实现用户的需求，这种开发模式比较常见，我们平时使用的
很多软件例如qq 迅雷 office 以及电脑游戏都属于CS架构
因为安装了客户端功能较为丰富，能够充分发挥用户计算机的
能力，但是这种开发模式后期维护困难，需要经常更新补丁
这种开发模式并不是Java语言擅长的

browser-server
浏览器服务器模式

用户不需要安装任何客户端，本地的浏览器就是客户端，与我们上网浏览网页非常类似，输入url地址，键入密钥，打开相应的
网页获得相应的权限，这种开发模式数据库等都放置在服务器端
用户不需要进行任何维护和更新等，缺点依赖网络，功能较为单一
因为其跨平台性，以及网络的不断发展，Java在BS架构上
具有垄断地位，并且BS架构在很多领域已经处于垄断地位
 静态网页和动态网页

##  1.1  WEB工程错误常见解决方案

	<!-- 
		web工程错误代码解决方案
		
		注意:html页面不允许存在划黄线的标记
		html页面修改时不需要关闭tomcat
		java代码和web.xml文件如果进行修改，必须
		关闭tomcat，修改好之后重新开启tomcat进行测试
		
		404：
			1):404后面没有跟随路径
				表示tomcat开启失败
					失败原因
					a):web.xml配置错误
					b):tomcat已经开启了一个
					c):tomcat文件缺失
			2):404后面存在一个路径
				表示根据此路径无法找到资源
				例如action提交的路径与
				url-pattern节点下的值不匹配
				页面标准路径出现错误等
				标准路径
				http://localhost:8080/工程名/页面地址
		500：
				Java代码编译出错
				或者在web.xml文件中
				的<servlet-class>节点下没有找到
				servlet的实例地址
				can not found ** Class Exception
		405：
				get~~>doPost()
				post~~>doGet()导致
				method属性书写错误
	-->
##  1.2 Sevelt常见语句解释

```java
	//首先书写软编码
		//text/html:MIME信息表示使用流输出的页面并不是标准的
		//html页面，这里表示告诉浏览器按照html语法解析
		//如果书写错误，则浏览器要求用户下载页面
		response.setContentType("text/html;charset=utf-8");
		request.setCharacterEncoding("utf-8");protected void doPost(HttpServletRequest request,
			HttpServletResponse response)
			throws ServletException, IOException {
		//首先书写软编码
		//text/html:MIME信息表示使用流输出的页面并不是标准的
		//html页面，这里表示告诉浏览器按照html语法解析
		//如果书写错误，则浏览器要求用户下载页面
		response.setContentType("text/html;charset=utf-8");
		request.setCharacterEncoding("utf-8");
		
		//拿取字符流
		PrintWriter out = response.getWriter();
		
		//接受页面提交过来的值
		//括号内的为提交键值对的key
		//<input type="text" name="对应这里" />
		String name = request.getParameter("myname");
		String pass = request.getParameter("mypass");
		
		UserDaoIf dao = new UserDaoImpl();
		
		User u = dao.getUserByNameAndPass(name, pass);
		
		//开始动态输出页面
		out.println("<html>");
		out.print("<head>");
		if(u!=null){
			out.print("<title>登录成功</title>");
			out.println("</head>");
			out.println("<body>");
			out.print("欢迎您回来"+u.getName()+"<hr />");
			/*
			 * 	点击此链接之后，到达另外一个Servlet实例
			 * 在此Servlet中动态输出页面
			 * 此页面中存在一个表格，此表格显示所有用户的资料
			 * 
			 * */
			out.print("<a href='servlet/ShowAll'>查看全部用户资料</a>");
			out.println("</body>");
			out.println("</html>");
			out.flush();
			out.close();
			return;
		}
		out.print("<title>登录失败</title>");
		out.println("</head>");
		out.println("<body>");
		out.print("数据库查无此人，请您先<a href='/ServletDay1_base/register.html'>登录</a><hr />");
		out.println("</body>");
		out.println("</html>");
		out.flush();
		out.close();
	}
```

##  1.3 Sevlet 四种范围及其局限性

```
 Servlet四种范围及其局限性:
		 * 
		 * 		所谓范围的有效和失效是指
		 * 		在特定的跳转之后
		 * 		如果setAttribute()封装的参数还可以
		 * 		通过getAttribute()来拿取，则称之为有效
		 * 		否则无效
		 * 
		 * 		PageContext		
		 * 						跳转立刻失效
		 * 		HttpServletRequest
		 * 						重定向立刻失效，因为不再是
		 * 						同一次请求
		 *		HttpSession		在最大不活动周期之内
		 *						可以拿取
		 *		ServletContext	只要tomcat不关闭
		 *						永远有效
		 * */
```

## 1.4 请求转发和重定向

###  **请求转发**

```java
	//请求转发
			/*
			 * Servlet执行请求转发之后，
			 * 跳转到另外一个Servlet或者页面
			 * 由于是Servlet转发到目的地，浏览器并不知道
			 * 最终跳转到了另一个Servlet，所以浏览器
			 * 地址栏依然显示跳转之前的路径
			 * 跳转之前是do***，跳转之后还是do***
			 * 因为是同一次请求，所以request范围依然有效
			 * 请求转发没有固定的路径书写方式
			 * 
			 * 如果经历过请求转发，则之后的跳转路径
			 * 全部书写为绝对路径
			 * 
			 * */
request.getRequestDispatcher("Show")
			.forward(request, response);
```

### 重定向

```java
//重定向
		/*
		 * 	由Servlet实例返回响应浏览器重新定位到目的地
		 * 浏览器发送了两次请求，最终定位到目的地，不再是同一次
		 * 请求，request范围失效，跳转之前是do***
		 * 跳转之后肯定是doGet
		 * 浏览器地址栏显示的就是最终的路径
		 * 重定向跳转的路径一般是
		 * /工程名/servlet/类名
		 * /工程名/***.html
		 * /工程名/***.jsp
		 * 因为是从根目录发送请求，一般采用绝对路径书写方式  
		 * */
response.sendRedirect(
		"/ServletDay2_scope/servlet/Show");
```

## 1.5 Servlet 生命周期

```java
public class TestLife extends HttpServlet {
	
	private String mycode;
	
	/*
	 * 	Servlet的生命周期
	 *  
	 *  构造方法(创建出Servlet实例仅仅执行1次)~~~~>
	 *  初始化方法(一般进行一些初步的初始化工作仅仅执行1次)~~~~>
	 *  service()(此方法一般不被覆盖，它可以根据用户提交方式的不同
	 *  调用doGet或者doPost,执行多次)~~~~>
	 *  doGet()||doPost()(执行多次，专门用来处理业务逻辑)
	 *  destroy()(当tomcat关闭，最终执行此方法，
	 *  一般用来释放资源，仅仅执行1次)
	 *  
	 * 	Servlet是一个单实例多线程的Java程序
	 *  仅仅创建一次，当请求过来分配多个线程给每个请求，
	 *  彼此不受影响，存在线程安全隐患
	 * 
	 * 
	 * */
	/**
	 * 构造方法
	 */
	public TestLife() {
		System.out.println("我是Servlet的构造方法~~~");
	}

	/**
	 * 销毁方法
	 */
	public void destroy() {
		System.out.println("我是Servlet的销毁方法~~~");
	}

	/**
		doGet执行业务逻辑的方法
	 */
	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {

		response.setContentType("text/html;charset="+mycode);
		request.setCharacterEncoding(mycode);
		System.out.println("我是Servlet的doGet~~~");
		PrintWriter out = response.getWriter();
		out.println("<!DOCTYPE HTML PUBLIC \"-//W3C//DTD HTML 4.01 Transitional//EN\">");
		out.println("<HTML>");
		out.println("  <HEAD><TITLE>A Servlet</TITLE></HEAD>");
		out.println("  <BODY>");
		out.print("    This is ");
		out.print(this.getClass());
		out.println(", using the GET method");
		out.println("  </BODY>");
		out.println("</HTML>");
		out.flush();
		out.close();
	}

	/**
		初始化方法
	 */
	public void init() throws ServletException {
		System.out.println("我是Servlet的初始化方法~~~");
		
		/*
		 * ServletConfig:
		 * 		接口，此接口可以拿取某个特定Servlet的
		 * 私有参数，独有的配置信息
		 * */
		/*
		 * 
		 * TestLife tl = new TestLife();
		 * ServletConfig config = 
		 * tl.getServletConfig();
		 * 
		 * */
		ServletConfig config = 
				this.getServletConfig();
	
		//拿取配置的私有参数
		/*String value1 = 
		config.getInitParameter("五月三日");
	
		System.out.println("私有参数是~~>"+value1);
	
		mycode = config.getInitParameter("mycode");*/
	
		/*
		 * ServletContext:
		 * 		接口，专门用来封装整个工程中所有的
		 * 共享参数等，并不局限于某一个Servlet实例
		 * */
		ServletContext application = 
				this.getServletContext();
		
		mycode = application.getInitParameter("etoak");
		
		/*
		 * 方法测试:
		 * 		ServletConfig:
		 * 			getServletName 		  
					getInitParameter
					拿取私有参数的值
				ServletContext:
					getContextPath
					getInitParameter
					拿取共享参数的值
					getRealPath
				HttpServletRequest:
					getParameter
					getContextPath
					getMethod
					getParameterValues
				HttpServletResponse:
					getOutputStream
					getWriter	
		 * 
		 * */
	}
}

```

## 1.6 Servlet会话跟踪

```java
	在Servlet中存在两种会话跟踪机制
  		Cookie
  			由客户端浏览器提供的一种会话跟踪机制，
  			用来保存数据，这些数据可以用来设置用户的
  			权限。cookie存在实体，最终保存在用户的本地
  			我们可以找到cookie的文件，默认不支持中文
  			通过
  				Cookie cookie = new Cookie(String,String);
  			创建cookie，之后设置生命周期，根据设置的生命周期
  			开始倒计时，到期cookie销毁，内部数据无法保存。
  			在此期间用户关闭计算机 浏览器等都不会影响cookie
  			的生命周期
  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  		HttpSession
  		
  			由服务器提供一种会话跟踪机制，安全性较
  			cookie高，没有实体，不存在一个文件保存在
  			客户端或者服务器，session通过
  			request.getSession()来创建，创建之后存在
  			一个最大不活动周期默认1800s(可以修改)
  			之后开始倒计时，如果在此期间用户提交了请求，则
  			倒计时重置，因此理论上session可以一直有效，
  			但是如果关闭浏览器或者在最大不活动周期之内没有
  			请求提交，则session销毁
  			session支持中文，通过setAttribute(String,Object)
  			来封装信息
  	~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  	#cookie与session如何销毁
  	cookie:
  		1)生命周期到期
  		2)cookie.setMaxAge(0)
  	session
  		1)超过最大不活动周期(不同的计算机存在误差)
  		2)session.invalidate()
  			立刻销毁当前session
  		3)关闭浏览器
  			关闭浏览器并不是严格意义上的删除session
  			而是当我们再次打开浏览器时会再次分配一个
  			session，之前的session再也无法拿取了
  	~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  	#cookie与session两种会话跟踪机制有什么联系吗？
  	如果存在联系说出它们的关联?
  	
  	session是依靠Cookie来进行维持的，cookie传输了
  	session重要的一个参数sessionid （唯一的session标识）
  	当用户书写request.getSession()方法时，首先web容器
  	从cookie中查看是否当前存在sessionid，如果没有sessionid
  	则创建一个新的session，并分配给这个session一个sessionid
  	跳转之前将sessionid封装进当前的cookie
  	Cookie cookie = new Cookie("jsessionid","48539579454958495");
  	当跳转到另外一个页面之后，当用户书写
  	request.getSession()这个方法时，再次到cookie中拿取
  	sessionid,如果可以拿取，则根据拿取的sessionid名
  	获得绑定的session会话。
  	
  	#Cookie如果禁用，session还能维持吗？
  	如果想维持session应该使用什么方法?
  	默认情况下session不能维持了，但是我们可以使用
  	重写url的方式来继续维持session会话跟踪机制
```

##    1.7 读写Cookie

### 读

```java
Cookie[] cookies = request.getCookies();
		
		if(cookies!=null){
			for(Cookie co:cookies){
				System.out.println("cookie保存的键是~~》"
						+co.getName());
				System.out.println("cookie保存的值是~~》"
						+co.getValue());
			}
```

### 写 &Cookie的设置

```java
response.setContentType("text/html;charset=utf-8");
		request.setCharacterEncoding("utf-8");
		PrintWriter out = response.getWriter();
		
		String name = request.getParameter("name");
		String pass = request.getParameter("pass");
		
		if(name.trim().equals("elena")&&
				pass.trim().equals("12345")){
			//接受cookie的生命周期
			//number format Exception null
			Integer life = Integer.parseInt(
					request.getParameter("life"));
			//创建一个Cookie
			Cookie cookie = new Cookie("n",name);
			//设置cookie的生命周期
			/*
			 * 	注意这里
			 * 	1)正数：表示倒计时的秒数
			 * 	2)0：立刻销毁
			 *  3)负数：类似session，cookie保存在
			 *  浏览器缓存中，当用户关闭浏览器等立刻销毁
			 * 
			 * */
			cookie.setMaxAge(life);
			//发送走cookie
			response.addCookie(cookie);
			
			cookie = new Cookie("p",pass);
			cookie.setMaxAge(life);
			response.addCookie(cookie);
			//重定向跳转到另外一个页面
			/*
			 * getContextPath():
			 * 	拿取/工程名
			 * request.getContextPath()
			 * application.getContextPath()
			 * 
			 * */
			response.sendRedirect(
			"/ServletDay3_cookie_session/servlet/ReadCookie");
```

##  1.8 Session拿取和设置

```java
if(name.trim().equals("elena")&&
				pass.trim().equals("12345")){
			/*
			 * request.getSession():
			 * 此方法表示拿取已经存在的session或者
			 * 创建新的session
			 * 
			 * */
			//创建新的session
			HttpSession session = request.getSession();
			//设置最大不活动周期
			session.setMaxInactiveInterval(1000);
			//封装信息
			session.setAttribute("n",name);
			session.setAttribute("p",pass);
			//打印session的id这个id类似于我们身份证号，
			//是session的唯一标识
			System.out.println("跳转前sessionid是~~~~~》"
			+session.getId());
			
			String path = "/ServletDay3_cookie_session/servlet/TestSession2";
			//重写url使用浏览器地址栏分号传值
			String newPath = response.encodeURL(path);
			
			response.sendRedirect(newPath);
```

从Session中拿取数据

```java
//获取已经存在的session
		HttpSession session = request.getSession();
		
		System.out.println("跳转后的sessionid是~~~~~》"
		+session.getId());
		String name = (String)session.getAttribute("n");
		String pass = (String)session.getAttribute("p");
		
		if(name!=null&&pass!=null){  }
```

##  1.9 HTTP请求方法

```java
GET
要求得到所请求URL上的东西。

POST
要求服务器接受附加到请求的信息内容，并提供请求URL上的东西。

HEAD
只要求得到GET返回结果的头部信息。

TRACE
要求请求信息回送，这样客户端可以看到服务器端接收到什么，以便测试或排错。

PUT
指出要把所包含的信息内容放到请求的URL上。

DELETE
指出要删除所请求URL上的东西。

OPTIONS
要求得到一个HTTP方法列表，表明所请求的URL上的东西可以对这些方法做出响应。

```

##  1.10 Servlet线程

```
servlet采用单实例多线程模式开发，减少产生servlet实例的开销。servlet容器维护一个线程池，里面放着工作者线程来相应请求，同时还有一个调度线程来管理工作者线程。当容器收到一个servlet请求，调度线程就从线程池中取出一个工作者线程，该工作者线程将处理这个请求，做法是执行servlet的service方法；当这个线程执行时，收到另一个请求，调度线程就在线程池中取出另一个工作者线程来响应新的请求。容器不关心请求是否访问的是同一个servlet,当多个请求同时访问同一个servlet时，这个servlet的service方法将在多线程中并发执行。并发执行必然会出现同步问题，也就是线程的安全问题，下面是网上找到的原本版本。绕来绕去，总也绕不开几门基础课，终于明白为什么考研要考数据结构，操作系统，组成原理和网络原理了，这一个线程安全问题就涉及到数据结构和操作系统，数据结构帮助选择线程安全的数据类型，操作系统帮助找到线程安全的操作方法。

一. Servlet容器如何同时来处理多个请求 
     工作者线程Work Thread:执行代码的一组线程 
     调度线程Dispatcher Thread：每个线程都具有分配给它的线程优先级,线程是根据优先级调度执行的 
     Servlet采用多线程来处理多个请求同时访问。servlet依赖于一个线程池来服务请求。线程池实际上是一系列的工作者线程集合。Servlet使用一个调度线程来管理工作者线程. 
     当容器收到一个Servlet请求，调度线程从线程池中选出一个工作者线程,将请求传递给该工作者线程，然后由该线程来执行Servlet的service方法。当这个线程正在执行的时候,容器收到另外一个请求,调度线程同样从线程池中选出另一个工作者线程来服务新的请求,容器并不关心这个请求是否访问的是同一个Servlet.当容器同时收到对同一个Servlet的多个请求的时候，那么这个Servlet的service()方法将在多线程中并发执行。 
      Servlet容器默认采用单实例多线程的方式来处理请求，这样减少产生Servlet实例的开销，提升了对请求的响应时间，对于Tomcat可以在server.xml中通过<Connector>元素设置线程池中线程的数目。 
     就实现来说： 
      调度者线程类所担负的责任是线程的调度，只需要利用自己的属性完成自己的责任。所以该类是承担了责任的，并且该类的责任又集中到唯一的单体对象中。 
而其他对象又依赖于该特定对象所承担的责任，我们就需要得到该特定对象。那该类就是一个单例模式的实现了。 
    
二 如何开发线程安全的Servlet 
    1，变量的线程安全：这里的变量指字段和共享数据(如表单参数值)。 

      a，将 参数变量本地化。多线程并不共享局部变量.所以我们要尽可能的在servlet中使用局部变量。 
      例如：String user = ""; 
         user = request.getParameter("user"); 

      b，使用同步块Synchronized，防止可能异步调用的代码块。这意味着线程需要排队处理。 
在使用同步块的时候要尽可能的缩小同步代码的范围，不要直接在sevice方法和响应方法上使用同步，这样会严重影响性能。 

   2,属性的线程安全：ServletContext，HttpSession，ServletRequest对象中属性 
     ServletContext：（线程是不安全的） 
    ServletContext是可以多线程同时读/写属性的，线程是不安全的。要对属性的读写进行同步处理或者进行深度Clone()。 
     所以在Servlet上下文中尽可能少量保存会被修改（写）的数据，可以采取其他方式在多个Servlet中共享，比方我们可以使用单例模式来处理共享数据。 
     HttpSession：（线程是不安全的） 
     HttpSession对象在用户会话期间存在，只能在处理属于同一个Session的请求的线程中被访问，因此Session对象的属性访问理论上是线程安全的。 
     当用户打开多个同属于一个进程的浏览器窗口，在这些窗口的访问属于同一个Session，会出现多次请求，需要多个工作线程来处理请求，可能造成同时多线程读写属性。 
     这时我们需要对属性的读写进行同步处理：使用同步块Synchronized和使用读/写器来解决。 

     ServletRequest：（线程是安全的） 
     对于每一个请求，由一个工作线程来执行，都会创建有一个新的ServletRequest对象，所以ServletRequest对象只能在一个线程中被访问。ServletRequest是线程安全的。 
    注意：ServletRequest对象在service方法的范围内是有效的，不要试图在service方法结束后仍然保存请求对象的引用。 

3，使用同步的集合类： 
    使用Vector代替ArrayList，使用Hashtable代替HashMap。 

4，不要在Servlet中创建自己的线程来完成某个功能。 
    Servlet本身就是多线程的，在Servlet中再创建线程，将导致执行情况复杂化，出现多线程安全问题。 

5，在多个servlet中对外部对象(比方文件)进行修改操作一定要加锁，做到互斥的访问。 
   
6，javax.servlet.SingleThreadModel接口是一个标识接口，如果一个Servlet实现了这个接口，那Servlet容器将保证在一个时刻仅有一个线程可以在给定的servlet实例的service方法中执行。将其他所有请求进行排队。 
   服务器可以使用多个实例来处理请求，代替单个实例的请求排队带来的效益问题。服务器创建一个Servlet类的多个Servlet实例组成的实例池，对于每个请求分配Servlet实例进行响应处理，之后放回到实例池中等待下此请求。这样就造成并发访问的问题。 
此时,局部变量(字段)也是安全的，但对于全局变量和共享数据是不安全的，需要进行同步处理。而对于这样多实例的情况SingleThreadModel接口并不能解决并发访问问题。 

SingleThreadModel接口在servlet规范中已经被废弃了。
```



##  1.11 Servlet 各个参数的方法

```

1>ServletConfig  
	封装了所有servlet私有的参数设置
	--- getServletName 			
	--- getServletContext  
	--- getInitParameter
	--- getInitParameterNames

====================================================
2>ServletContext
	封装了所有本工程所有servlet的参数设置
	*--- setAttribute
	*--- getAttribute
	*--- getAttributeNames
	--- getInitParameter
	--- getInitParameterNames
	--- getRealPath
====================================================
3>HttpServletRequest
	封装了所有 请求过来的方法
	
	与路径有关
	--- getMethod
	--- getQueryString
	--- getContextPath
	--- getRequestURI
	--- getRequestURL
	--- getServletPath
	
	
	与字符和MIME类型有关
	--- setCharacterEncoding
	--- getContentType	
	--- getRequestedSessionId
	
	与属性值有关
	*--- setAttribute
	*--- removeAttribute
	*--- getAttribute
	*--- getAttributeNames
	
	与主机信息有关
	--- getLocalPort
	
	
	与参数有关
	--- getParameterNames
	--- getParameter
	--- getParameterValues

	http://localhost:8080/工程名/首页
	拿取字符串
==================================================
4>HttpServletResponse
	封装了所有响应过去的方法


	与输出有关
	--- getOutputStream
	--- getWriter				

	与字符和MIME了下有关
	--- setCharacterEncoding
	--- setContentType
```

## 1.12 Servlet原理

![两种跳转方式](C:\Users\Administrator\Desktop\NOTE\source\image\两种跳转方式.jpg)

![四种范围](C:\Users\Administrator\Desktop\NOTE\source\image\四种范围.jpg)

###  过滤器

![Filter流程](C:\Users\Administrator\Desktop\NOTE\source\image\Filter流程.jpg)

![多层过滤器](C:\Users\Administrator\Desktop\NOTE\source\image\多层过滤器.gif)

