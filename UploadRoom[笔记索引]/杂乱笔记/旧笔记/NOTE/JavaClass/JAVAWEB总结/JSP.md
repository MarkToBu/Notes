# JSP 

##  1.0  JSP指令元素

	page指令元素用来设置页面的具体参数
	language:页面脚本类型,书写代码的种类
	import:页面导包，多个包用逗号隔开
	不推荐使用快捷键
	pageEncoding:页面转换编码，如果使用
	默认的iso-8859-1无法保存中文
	session:打开页面是否创建session支持session会话跟踪
	机制，如果为true，则支持
	buffer:页面缓存 默认8kb
	info:用来设置页面的签名 书写时间等信息，
	通过getServletInfo()方法可以取出
	isELIgnored:是否忽略EL表达式，false不忽略
	contentType:对应Servlet中
	response.setContentType("text/html;charset=utf-8")
	是软编码的一部分
	autoFlush:页面是否支持自动刷新，默认不支持
	isThreadSafe:当前页面线程是否安全
	默认true，不安全单实例多线程。更改为false
	则底层实现SingleThreadModel单实例单线程
	线程安全效率低下
	isErrorPage:当前页面能否使用exception内置对象
	true可以使用 false无法使用
	errorPage：当前页面如果出现异常，自动跳转到哪个
	页面
###  1.10.1 JSP页面脚本写法

```jsp
 <body>
  	<%--  这是jsp的注释方式可以同时注释java代码
  	和页面超文本  --%>
	<h3>~你知道五四青年节得由来吗？~</h3>
	<hr>
	<%--
		1:带!和;
			生成的源码在service()外，是类级别的
			参数
	--%>
	<%! String str1 = "五四爱国运动爆发"; %>
	<%--
		2:不带! 带;
			生成的源码在service()内，是局部变量
	--%>
	<% String str2 = "拒绝在合约上签字"; %>
	<%--
		3:表达式的输出方式
			生成的源码在service()方法内，
			直接输出，注意没有;
	--%>
	<%=str1 %>
	<%=str2 %>
	<hr>
	<%!
		int count = 10;
		
		public int getCount(){
			count++;
			return count;
		}
		
		public Object getNull(){
			return null;
		}
	
	%>
	
	<%if(count%2==0){ %>
		<font color="red"><%=count %></font>
	<%}else{ %>
		<font color="blue"><%=count %></font>
	<%} %>
	
	<% 
		int count = 100; 
		count++;
	%>
	看看这里的效果~~~~~》<%=count %>
	<br>
	再看看这里的效果呀~~~~~》<%=getCount() %>
	<br>
	最后看看这里的效果~~~~~》<%=getNull() %>
	 <% String str1 = "我是主页设置的值"; %>
	<%--
		导入一个外部页面
		表示将file设置的外部页面引入到本页面
		使之成为一个页面
	--%>
	<%@ include file="data2.jsp" %>	
	<%=str2 %> 	
  </body>
```

 

##  1.1 EL表达式

###  1.1.1  EL表达式基本用法

```jsp
<html>
  <head>
    <title>1：EL表达式(Express Language)</title>
  </head>
  <body>
	<%--
		表达式:凡是称之为表达式都是指仅仅可以显示
		数据，无法进行复杂的业务逻辑
		格式:
			${要展示的数据}
			如果大括号内存在引号，则表示字符串直接输出
	--%>
	<h3>算数运算</h3>
	${"3+1"}=${3+1}<br>
	${"2-1"}=${2-1}<br>
	${"5*3"}=${5*3}<br>
	<h3>关系运算</h3>
	${3>1}<br>
	${2<10}<br>
	<h3>逻辑运算</h3>
	${3>1&&2<100}<br>
	${1>1||100<0}<br>
	<h3>三目运算</h3>
	${3>1?true:false}<br>
	${2<10?1:0}<br>
	<h3>范围取值</h3>
	<%
		pageContext.setAttribute("elena","elenaPage");
		request.setAttribute("elena","elenaRequest");
		session.setAttribute("elena","elenaSession");
		application.setAttribute("elena","elenaApplication");
	%>
	<%=pageContext.getAttribute("elena") %>
	<%=request.getAttribute("elena") %>
	<%=session.getAttribute("elena") %>
	<%=application.getAttribute("elena") %>
	<br>
	<%--
		格式:${范围.key}
		范围:pageScope requestScope 
		sessionScope applicationScope
		在key值没有冲突的情况下，可以省略
		范围.
	--%>
	${pageScope.elena}
	${requestScope.elena}
	${sessionScope.elena}
	${applicationScope.elena}
	<%-- 如果不指定范围，则默认输出现存范围最小的 --%>
	${elena}
	<h3>接受页面通过?传递过来的值</h3>
	接受值1:<%=request.getParameter("etoak") %>
	<br>
	<%--
		格式:${param.key}
		注意param就是parameter的简写，
		这里与范围不同，永远不能省略
	--%>
	接受值2:${param.etoak}
	<h3>获取配置在web.xml中的全局参数</h3>
	拿取全局参数:<%=application
	.getInitParameter("今天是五月五号") %>
	<br>
	<%--
		注意这里initParam.不能省略
	--%>
	拿取全局参数:${initParam.今天是五月五号}
	<h3>拿取自定义数据类型的值</h3>
	<%
		Hero hero = new Hero(1,"马可波罗",13888);
		session.setAttribute("myhero",hero);
		response.sendRedirect("/JspDay2_el_jstl/show.jsp");
	%>
	
  </body>
```

## 1.2 EL表达式常见用法

```jsp
	<% String str = "damon"; %>
	<a href="index.jsp?etoak=<%=str %>">这是个链接</a>
	
		拿取实体类属性值:
	<%=((Hero)session.getAttribute("myhero")).getName() %>
  	<br>
  	<%--
  		${范围.key.属性名}
  	--%>
  	拿取实体类属性值:
  	${sessionScope.myhero.name}
  	<h3>注意</h3>
  	<% int i = 3; 
  		pageContext.setAttribute("i",i);
  	%>
  	取值1：<%=i %>
  	<%--
  		注意EL表达式并不一定能显示任意值，必须
  		根据key从范围中拿取，如果无法显示，则不显示
  		任何数据也不显示null
  	--%>
  	取值2：${i}
	
```

##  1.3  jstl 创建	常见标签库

```jsp
<%@ page language="java" import="java.util.*" 
pageEncoding="UTF-8" %>
<%@ taglib prefix="c" 
uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
    <title>jstl(jsp standard tag lib)jsp标准标签库</title>
  	<%--
  		JSP第二代标签
  	--%>
  </head>
  <body>
	<%--
		1:赋值
		var:key值
		value:value值
		scope:范围
		page request session application
	--%>	
	<c:set var="elena" value="elenaPage"
	scope="page"></c:set>
	<c:set var="elena" value="elenaRequest"
	scope="request"></c:set>
	<c:set var="elena" value="elenaSession"
	scope="session"></c:set>
	<c:set var="elena" value="elenaApplication"
	scope="application"></c:set>
	<%--
		2:取值
		value:要输出的值
		可以使用java代码，可以直接书写字符串
		，可以使用EL表达式
	--%>
	<c:out value="直接输出字符串"></c:out>
	<%String str = "我是java代码"; %>
	<c:out value="<%=str %>"></c:out>
	
	<c:out value="${pageScope.elena}"></c:out>
	<c:out value="${requestScope.elena}"></c:out>
	<c:out value="${sessionScope.elena}"></c:out>
	<c:out value="${applicationScope.elena}"></c:out>
	
	<%--
		3:删除值
		var:要删除的key
		scope:要删除的范围，
		注意如果不指定相应范围，则默认删除
		所有匹配key值的
		default:如果无法取值
		默认输出的值
	--%>
	<c:remove var="elena" scope="request" />
	<c:out value="${requestScope.elena}"
	default="删除成功 "></c:out>
	<%--
		4:流程控制
		test:后面如果为true
	--%>
	<c:if test="${3>1}">
		<c:out value="输出这里~~~~"></c:out>
	</c:if>
	
	<%--
		${empty key}
		空验证
		如果可以取出值来返回false
		无法取值返回true
		
	--%>
	<c:if test="${empty 王东}">
		<c:if test="${2>1}">
			<c:out value="最终输出了这里！！"></c:out>
		</c:if>
	</c:if>
	
	<%--
		注意c:choose必须和c:when在一起
	--%>
	<c:choose>
		<c:when test="${3>1}">
			<c:out value="输出这里啦！！"></c:out>
		</c:when>
		<c:otherwise>
			<c:out value="最终输出了这里！！"></c:out>
		</c:otherwise>
	</c:choose>
      
      
      <table style="width:500px" border="1px">
		<tr>
			<th>ID</th>
			<th>英雄名</th>
			<th>售价</th>
		</tr>
		<c:forEach items="${requestScope.mylist}" var="hero">
			<tr>
				<td><c:out value="${hero.id}"></c:out></td>
				<td><c:out value="${hero.name}"></c:out></td>
				<td><c:out value="${hero.total }"></c:out></td>
			</tr>
		</c:forEach>
	</table>
	 
  </body>
</html>
 
```

## 1.4  Jsp内建动作（jsp的第一代标签）

```jsp
<%@ page language="java" 
import="java.util.*,com.etoak.dao.*,com.etoak.po.User" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
    <title>Jsp内建动作（jsp的第一代标签）</title>
  </head>
  <body>
	<%-- <%
		String name = request.getParameter("name");
		String pass = request.getParameter("pass");
		UserDaoIf dao = new UserDaoImpl();
		User u = new User();
		u.setName(name);
		u.setPass(pass);
		dao.addUser(u);
	%> --%>
	<%--
		1:“新”建一个对象
			id:新建对象的key值，注意这是一个key值
			通过这个key值我们就知道了对象名，因为
			二者是重名的
			class:新建对象的类
			scope:新建对象的范围
			page（pageContext的简写） 
			request 
			session 
			application
			以下标签相当于
			User u = new User();
			request.setAttribute("u",u);
	--%>
	<jsp:useBean id="u" class="com.etoak.po.User"
	scope="request"></jsp:useBean>
	
	<%--
		给所有属性赋值
		property:表示给哪个属性赋值
		注意如果实体类的属性值和页面提交过来的key正好
		一一对应，则可以使用*表示给所有属性赋值
		name:对应usebean标签的id属性
		
		String name = request.getParameter("name");
		String pass = request.getParameter("pass");
		User u = (User)范围.getAttribute("key");
		u.setName(name);
		u.setPass(pass);
	
	<jsp:setProperty property="*" 
	name="u" />
		
		property:表示要设置的属性值
	
	<jsp:setProperty property="name" 
	name="u" />
	
		value:事先设置好的值
	<jsp:setProperty property="name" 
	name="u" value="我是设置好的用户名" />
	
		param:对应提交的key值
		当key值与实体类属性值不对应时使用
	--%>
	<jsp:setProperty property="name" name="u"
	param="etoakusername"/>
	<%--
		request.setAttribute("u",对应这里);
	--%>
	用户姓名:<%=u.getName() %>
	用户密码:<%=u.getPass() %>
	<hr>
	<%--
		request.setAttribute("对应这里",u);
	--%>
	用户姓名:<c:out value="${u.name}"></c:out>
	用户密码:<c:out value="${u.pass}"></c:out>
	<hr>
	<%-- 
		property:表示属性名
		name:对应usebean标签的id属性
	--%>
	用户姓名:<jsp:getProperty property="name" 
	name="u" />
	用户密码:<jsp:getProperty property="pass" 
	name="u" />
  </body>
</html>
```

##  1.5 JSP内建动作

```jsp
%@ page language="java" 
import="java.util.*,com.etoak.dao.*,com.etoak.po.User" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" 
uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
    <title>数据库测试</title>
  </head>
  <body>
	<jsp:useBean id="u" class="com.etoak.po.User"
	scope="request"></jsp:useBean>
	<jsp:useBean id="dao" class="com.etoak.dao.UserDaoImpl"
	scope="page"></jsp:useBean>
	<jsp:setProperty property="name" 
	name="u" param="etoakusername" />
	<jsp:setProperty property="pass" 
	name="u" />
	<c:choose>
		<c:when test="<%=dao.addUser(u) %>">
			<%--
				请求转发
				request.getRequestDispatcher("suc.jsp?thisiskey=thisisvalue").forward(request,response);
			--%>
			<jsp:forward page="suc.jsp">
				<jsp:param value="thisisvalue" 
				name="thisiskey" />
			</jsp:forward>
		</c:when>
		<c:otherwise>
			<%--
				重定向
			--%>
			<c:redirect url="err.jsp">
				<c:param name="thisiskey"
				value="thisisvalue"></c:param>
			</c:redirect>
		</c:otherwise>
	</c:choose>
  </body>
</html>
```

###  1.51   **jsp内建动作一定是新建一个对象吗？**

```jsp
<%---
		User u = null
		
		u = (User)范围.getAttribute("u");
		
		if(u==null){
			u = new User();
			范围.setAttribute("u",u);
		}
		
		#jsp内建动作中usebean这个标签肯定是新建一个对象吗？
		如果不是?说出为什么?
		不是
		首先根据id提供的key值和scope提供的范围
		从内存中拿取，如果可以拿取，则就拿取已经存在
		的对象，如果无法拿取，则重新创建一个新的对象
		并且赋值范围
		这里注意受到四范围的局限性影响
		pageContext 范围肯定新建一个对象
		request	重定向会新建一个对象
		session 超过最大不活动周期或者session销毁
		重新创建一个对象
		application 永远不会创建对象
	--%>
	<jsp:useBean id="u" class="com.etoak.po.User"
	scope="request"></jsp:useBean>
	用户姓名:<%=u.getName() %>
	用户密码:<%=u.getPass() %>
	<hr>
	用户姓名:<c:out value="${u.name}"></c:out>
	用户密码:<c:out value="${u.pass}"></c:out>
	<hr>
	用户姓名:<jsp:getProperty property="name" 
	name="u" />
	用户密码:<jsp:getProperty property="pass" 
	name="u" />	
	
	<%=request.getParameter("thisiskey") %>
	<c:out value="${param.thisiskey}"></c:out>
```

## 1.6   分页详解

```jsp
<%@ page language="java" 
import="java.util.*,com.etoak.po.Computer" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" 
uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
    <title>分页</title>
  </head>
  <body>
	<%--
		分页得种类
			假分页
				一次将所有数据取出，用户需要哪些数据
				就显示哪些数据
			真分页
				用户需要哪些数据，就从数据库中取出哪些
				数据
		~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
		分页公式 sql
		select 字段 from 表 limit x,y;
		x:起始索引值
		y:一共显示几条记录
		~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
		分页四要素
		1:总记录数			dao
		2:每页记录数			自己订
		3:总页数				(总记录数+每页记录数-1)/每页记录数
		4:当前页				默认是1
							此参数是不是常量而是一个
							变量
		~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
		分页四要素与分页公式
		select 字段 from 表 limit (当前页-1)*每页记录数,每页记录数
		
		eg:10条记录	每页3条 分4页
		page1:	1~3		0,3		(1-1)*3,3
		page2:  4~6     3,3     (2-1)*3,3
		page3:  7~9     6,3     (3-1)*3,3
	
		List<Computer> list = 
		dao.getPage((当前页-1)*每页记录数,每页记录数);
	~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	--%>
	<jsp:useBean id="dao"
	class="com.etoak.dao.ComputerDaoImpl"
	scope="page"></jsp:useBean>
	<%
		//1:总记录数
		int allRecord = dao.getCount();
	
		//2:每页记录数
		int pageRecord = 5;
	
		//3:总页数
		int allPage = 
		(allRecord+pageRecord-1)/pageRecord;
	
		//4:当前页
		int currentPage = 1;
	
		String value = request.getParameter("cup");
		if(value!=null){
			currentPage = Integer.parseInt(value);
		}
		
		List<Computer> list = 
		dao.getPage((currentPage-1)*pageRecord
				,pageRecord);
		pageContext.setAttribute("mylist",list);
	%>
	
	<table style="width:600px" border="1px">
		<tr>
			<th>ID</th>
			<th>品名</th>
			<th>售价</th>
			<th>归属地</th>
		</tr>
		<c:forEach items="${mylist}"
		var="etoak">
		<tr>
			<td><c:out value="${etoak.id}"></c:out></td>
			<td><c:out value="${etoak.name}"></c:out></td>
			<td><c:out value="${etoak.total}"></c:out></td>
			<td><c:out value="${etoak.location}"></c:out></td>
		</tr>
		</c:forEach>
	</table>
	
	
	<form action="index.jsp" method="get">
		
		<input type="hidden" name="cup" />
		
		<input type="button" value="首页" 
		onclick="change(1)" 
		<%=currentPage==1?"disabled":"" %>  />
		
		<input type="button" value="上一页" 
		onclick="change(<%=currentPage-1 %>)" 
		<%=currentPage==1?"disabled":"" %> />
		
		<input type="button" value="下一页" 
		onclick="change(<%=currentPage+1 %>)" 
		<%=currentPage==allPage?"disabled":"" %> />
		
		<input type="button" value="末页" 
		onclick="change(<%=allPage %>)" 
		<%=currentPage==allPage?"disabled":"" %> />
		
	</form>
	
	<script type="text/javascript">
		function change(cr){
			//将当前页的值赋值给隐藏域的值
			document.forms[0].cup.value = cr;
			//提交表单
			document.forms[0].submit();
		}
		
	</script>
  </body>
</html>
```



##  1.7 JSP语法图

![](C:\Users\Administrator\Desktop\NOTE\source\image\JSP语法图.jpg)

![](C:\Users\Administrator\Desktop\NOTE\source\image\JSP执行过程.bmp)

##  1.8 JSP九种内置对象

```java
JSP九种内置对象：request, response, session, application, page, pageContext, out, exception, config



1.request对象 
客户端的请求信息被封装在request对象中，通过它才能了解到客户的需求，然后做出响应。它是HttpServletRequest类的实例

序号 方 法 说 明 
1 object getAttribute(String name) 返回指定属性的属性值 
2 Enumeration getAttributeNames() 返回所有可用属性名的枚举 
3 String getCharacterEncoding() 返回字符编码方式 
4 int getContentLength() 返回请求体的长度（以字节数） 
5 String getContentType() 得到请求体的MIME类型 
6 ServletInputStream getInputStream() 得到请求体中一行的二进制流 
7 String getParameter(String name) 返回name指定参数的参数值 
8 Enumeration getParameterNames() 返回可用参数名的枚举 
9 String[] getParameterValues(String name) 返回包含参数name的所有值的数组 
10 String getProtocol() 返回请求用的协议类型及版本号 
11 String getScheme() 返回请求用的计划名,如:http.https及ftp等 
12 String getServerName() 返回接受请求的服务器主机名 
13 int getServerPort() 返回服务器接受此请求所用的端口号 
14 BufferedReader getReader() 返回解码过了的请求体 
15 String getRemoteAddr() 返回发送此请求的客户端IP地址 
16 String getRemoteHost() 返回发送此请求的客户端主机名 
17 void setAttribute(String key,Object obj) 设置属性的属性值 
18 String getRealPath(String path) 返回一虚拟路径的真实路径 
19 String getContextPath()
拿取/工程名



2.response对象 
response对象包含了响应客户请求的有关信息，但在JSP中很少直接用到它。它是HttpServletResponse类的实例。 

序号 方 法 说 明 
1 String getCharacterEncoding() 返回响应用的是何种字符编码 
2 ServletOutputStream getOutputStream() 返回响应的一个二进制输出流 
3 PrintWriter getWriter() 返回可以向客户端输出字符的一个对象 
4 void setContentLength(int len) 设置响应头长度 
5 void setContentType(String type) 设置响应的MIME类型 
6 sendRedirect(java.lang.String location) 重新定向客户端的请求 
　 


3.session对象 
session对象指的是客户端与服务器的一次会话，从客户连到服务器的一个WebApplication开始，直到客户端与服务器断开连接为止。它是HttpSession类的实例. 

序号 方 法 说 明 
1 long getCreationTime() 返回SESSION创建时间 
2 public String getId() 返回SESSION创建时JSP引擎为它设的惟一ID号 
3 long getLastAccessedTime() 返回此SESSION里客户端最近一次请求时间 
4 int getMaxInactiveInterval() 返回两次请求间隔多长时间此SESSION被取消(ms) 
5 String[] getValueNames() 返回一个包含此SESSION中所有可用属性的数组 
6 void invalidate() 取消SESSION，使SESSION不可用 
7 boolean isNew() 返回服务器创建的一个SESSION,客户端是否已经加入 
8 void removeValue(String name) 删除SESSION中指定的属性 
9 void setMaxInactiveInterval() 设置两次请求间隔多长时间此SESSION被取消(ms) 
　 
　 
　 
4.out对象 
out对象是JspWriter类的实例,是向客户端输出内容常用的对象 
序号 方 法 说 明 
1 void clear() 清除缓冲区的内容 
2 void clearBuffer() 清除缓冲区的当前内容 
3 void flush() 清空流 
4 int getBufferSize() 返回缓冲区以字节数的大小，如不设缓冲区则为0 
5 int getRemaining() 返回缓冲区还剩余多少可用 
6 boolean isAutoFlush() 返回缓冲区满时，是自动清空还是抛出异常 
7 void close() 关闭输出流 
　 　


5.page对象 
page对象就是指向当前JSP页面本身，有点象类中的this指针，它是java.lang.Object类的实例 

序号 方 法 说 明 
1 class getClass 返回此Object的类 
2 int hashCode() 返回此Object的hash码 
3 boolean equals(Object obj) 判断此Object是否与指定的Object对象相等 
4 void copy(Object obj) 把此Object拷贝到指定的Object对象中 
5 Object clone() 克隆此Object对象 
6 String toString() 把此Object对象转换成String类的对象 
7 void notify() 唤醒一个等待的线程 
8 void notifyAll() 唤醒所有等待的线程 
9 void wait(int timeout) 使一个线程处于等待直到timeout结束或被唤醒 
10 void wait() 使一个线程处于等待直到被唤醒 
11 void enterMonitor() 对Object加锁 
12 void exitMonitor() 对Object开锁 



6.application对象 
application对象实现了用户间数据的共享，可存放全局变量。它开始于服务器的启动，直到服务器的关闭，在此期间，此对象将一直存在；这样在用户 的前后连接或不同用户之间的连接中，可以对此对象的同一属性进行操作；在任何地方对此对象属性的操作，都将影响到其他用户对此的访问。服务器的启动和关闭 决定了application对象的生命。它是ServletContext类的实例。 

序号 方 法 说 明 
1 Object getAttribute(String name) 返回给定名的属性值 
2 Enumeration getAttributeNames() 返回所有可用属性名的枚举 
3 void setAttribute(String name,Object obj) 设定属性的属性值 
4 void removeAttribute(String name) 删除一属性及其属性值 
5 String getServerInfo() 返回JSP(SERVLET)引擎名及版本号 
6 String getRealPath(String path) 返回一虚拟路径的真实路径 
7 ServletContext getContext(String uripath) 返回指定WebApplication的
application对象 
8 int getMajorVersion() 返回服务器支持的Servlet API的最大版本号 
9 int getMinorVersion() 返回服务器支持的Servlet API的最大版本号 
10 String getMimeType(String file) 返回指定文件的MIME类型 
11 URL getResource(String path) 返回指定资源(文件及目录)的URL路径 
12 InputStream getResourceAsStream(String path) 返回指定资源的输入流 
13 RequestDispatcher getRequestDispatcher(String uripath) 返回指定资源的RequestDispatcher对象 
14 Servlet getServlet(String name) 返回指定名的Servlet 
15 Enumeration getServlets() 返回所有Servlet的枚举 
16 Enumeration getServletNames() 返回所有Servlet名的枚举 
17 void log(String msg) 把指定消息写入Servlet的日志文件 
18 void log(Exception exception,String msg) 把指定异常的栈轨迹及错误消息写入Servlet的日志文件 
19 void log(String msg,Throwable throwable) 把栈轨迹及给出的Throwable异常的说明信息 写入Servlet的日志文件 



7.exception对象 
exception对象是一个例外对象，当一个页面在运行过程中发生了例外，就产生这个对象。如果一个JSP页面要应用此对象，就必须把isErrorPage设为true，否则无法编译。他实际上是java.lang.Throwable的对象 

序号 方 法 说 明 
1 String getMessage() 返回描述异常的消息 
2 String toString() 返回关于异常的简短描述消息 
3 void printStackTrace() 显示异常及其栈轨迹 
4 Throwable FillInStackTrace() 重写异常的执行栈轨迹 

　 

8.pageContext对象 
pageContext对象提供了对JSP页面内所有的对象及名字空间的访问，也就是说他可以访问到本页所在的SESSION，也可以取本页面所在的application的某一属性值，他相当于页面中所有功能的集大成者，它的本 类名也叫pageContext。 

序号 方 法 说 明 
1 JspWriter getOut() 返回当前客户端响应被使用的JspWriter流(out) 
2 HttpSession getSession() 返回当前页中的HttpSession对象(session) 
3 Object getPage() 返回当前页的Object对象(page) 
4 ServletRequest getRequest() 返回当前页的ServletRequest对象(request) 
5 ServletResponse getResponse() 返回当前页的ServletResponse对象(response) 
6 Exception getException() 返回当前页的Exception对象(exception) 
7 ServletConfig getServletConfig() 返回当前页的ServletConfig对象(config) 
8 ServletContext getServletContext() 返回当前页的ServletContext对象(application) 
9 void setAttribute(String name,Object attribute) 设置属性及属性值 
10 void setAttribute(String name,Object obj,int scope) 在指定范围内设置属性及属性值 
11 public Object getAttribute(String name) 取属性的值 
12 Object getAttribute(String name,int scope) 在指定范围内取属性的值 
13 public Object findAttribute(String name) 寻找一属性,返回起属性值或NULL 
14 void removeAttribute(String name) 删除某属性 
15 void removeAttribute(String name,int scope) 在指定范围删除某属性 
16 int getAttributeScope(String name) 返回某属性的作用范围 
17 Enumeration getAttributeNamesInScope(int scope) 返回指定范围内可用的属性名枚举 
18 void release() 释放pageContext所占用的资源 
19 void forward(String relativeUrlPath) 使当前页面重导到另一页面 
20 void include(String relativeUrlPath) 在当前位置包含另一文件 



9.config对象 
config对象是在一个Servlet初始化时，JSP引擎向它传递信息用的，此信息包括Servlet初始化时所要用到的参数（通过属性名和属性值构成）以及服务器的有关信息（通过传递一个ServletContext对象） 

序号 方 法 说 明 
1 ServletContext getServletContext() 返回含有服务器相关信息的ServletContext对象 
2 String getInitParameter(String name) 返回初始化参数的值 
3 Enumeration getInitParameterNames() 返回Servlet初始化所需所有参数的枚举

String value= 
this.getServletConfig().getInitParameter("key");
<initParam>
	<param-name>key</param-name>
	<param-value>value</param-value>
</initParam>
```

 ## 1.9 SmartUpload使用介绍

```java
一、安装篇 

　　jspSmartUpload是由www.jspsmart.com网站开发的一个可免费使用的全功能的文件上传下载组件，适于嵌入执行上传下载操作的JSP文件中。该组件有以下几个特点： 

1、使用简单。在JSP文件中仅仅书写三五行java代码就可以搞定文件的上传或下载，方便。 

2、能全程控制上传。利用jspSmartUpload组件提供的对象及其操作方法，可以获得全部上传文件的信息（包括文件名，大小，类型，扩展名，文件数据等），方便存取。 

3、能对上传的文件在大小、类型等方面做出限制。如此可以滤掉不符合要求的文件。 

4、下载灵活。仅写两行代码，就能把Web服务器变成文件服务器。不管文件在Web服务器的目录下或在其它任何目录下，都可以利用jspSmartUpload进行下载。 

5、能将文件上传到数据库中，也能将数据库中的数据下载下来。这种功能针对的是MySQL数据库，因为不具有通用性，所以本文不准备举例介绍这种用法。 

　　jspSmartUpload组件可以从www.jspsmart.com网站上自由下载，压缩包的名字是jspSmartUpload.zip。下载后，用WinZip或WinRAR将其解压到Tomcat的webapps目录下（本文以Tomcat服务器为例进行介绍）。解压后，将webapps/jspsmartupload目录下的子目录Web-inf名字改为全大写的WEB-INF，这样一改jspSmartUpload类才能使用。因为Tomcat对文件名大小写敏感，它要求Web应用程序相关的类所在目录为WEB-INF，且必须是大写。接着重新启动Tomcat，这样就可以在JSP文件中使用jspSmartUpload组件了。 

　　注意，按上述方法安装后，只有webapps/jspsmartupload目录下的程序可以使用jspSmartUpload组件，如果想让Tomcat服务器的所有Web应用程序都能用它，必须做如下工作： 

1．进入命令行状态，将目录切换到Tomcat的webapps/jspsmartupload/WEB-INF目录下。 

2．运行JAR打包命令：jar cvf jspSmartUpload.jar com 

（也可以打开资源管理器，切换到当前目录，用WinZip将com目录下的所有文件压缩成jspSmartUpload.zip，然后将jspSmartUpload.zip换名为jspSmartUpload.jar文件即可。） 

3．将jspSmartUpload.jar拷贝到Tomcat的shared/lib目录下。 

二、相关类说明篇 

㈠ File类 

　　这个类包装了一个上传文件的所有信息。通过它，可以得到上传文件的文件名、文件大小、扩展名、文件数据等信息。 

　　File类主要提供以下方法： 

1、saveAs作用：将文件换名另存。 

原型： 

public void saveAs(java.lang.String destFilePathName) 

或 

public void saveAs(java.lang.String destFilePathName, int optionSaveAs) 

其中，destFilePathName是另存的文件名，optionSaveAs是另存的选项，该选项有三个值，分别是SAVEAS_PHYSICAL,SAVEAS_VIRTUAL，SAVEAS_AUTO。SAVEAS_PHYSICAL表明以操作系统的根目录为文件根目录另存文件，SAVEAS_VIRTUAL表明以Web应用程序的根目录为文件根目录另存文件，SAVEAS_AUTO则表示让组件决定，当Web应用程序的根目录存在另存文件的目录时，它会选择SAVEAS_VIRTUAL，否则会选择SAVEAS_PHYSICAL。 

例如，saveAs("/upload/sample.zip",SAVEAS_PHYSICAL)执行后若Web服务器安装在C盘，则另存的文件名实际是c:\upload\sample.zip。而saveAs("/upload/sample.zip",SAVEAS_VIRTUAL)执行后若Web应用程序的根目录是webapps/jspsmartupload，则另存的文件名实际是webapps/jspsmartupload/upload/sample.zip。saveAs("/upload/sample.zip",SAVEAS_AUTO)执行时若Web应用程序根目录下存在upload目录，则其效果同saveAs("/upload/sample.zip",SAVEAS_VIRTUAL)，否则同saveAs("/upload/sample.zip",SAVEAS_PHYSICAL)。 

建议：对于Web程序的开发来说，最好使用SAVEAS_VIRTUAL，以便移植。 

2、isMissing 

作用：这个方法用于判断用户是否选择了文件，也即对应的表单项是否有值。选择了文件时，它返回false。未选文件时，它返回true。 

原型：public boolean isMissing() 

3、getFieldName 

作用：取HTML表单中对应于此上传文件的表单项的名字。 

原型：public String getFieldName() 

4、getFileName 

作用：取文件名（不含目录信息） 

原型：public String getFileName() 

5、getFilePathName 

作用：取文件全名（带目录） 

原型：public String getFilePathName 

6、getFileExt 

作用：取文件扩展名（后缀） 

原型：public String getFileExt() 

7、getSize 

作用：取文件长度（以字节计） 

原型：public int getSize() 

8、getBinaryData 

作用：取文件数据中指定位移处的一个字节，用于检测文件等处理。 

原型：public byte getBinaryData(int index)。其中，index表示位移，其值在0到getSize()-1之间。 

㈡ Files类 

　　这个类表示所有上传文件的集合，通过它可以得到上传文件的数目、大小等信息。有以下方法： 

1、getCount 

作用：取得上传文件的数目。 

原型：public int getCount() 

2、getFile 

作用：取得指定位移处的文件对象File（这是com.jspsmart.upload.File，不是java.io.File，注意区分）。 

原型：public File getFile(int index)。其中，index为指定位移，其值在0到getCount()-1之间。 

3、getSize 

作用：取得上传文件的总长度，可用于限制一次性上传的数据量大小。 

原型：public long getSize() 

4、getCollection 

作用：将所有上传文件对象以Collection的形式返回，以便其它应用程序引用，浏览上传文件信息。 

原型：public Collection getCollection() 

5、getEnumeration 

作用：将所有上传文件对象以Enumeration（枚举）的形式返回，以便其它应用程序浏览上传文件信息。 

原型：public Enumeration getEnumeration() 

㈢ Request类 

　　这个类的功能等同于JSP内置的对象request。只所以提供这个类，是因为对于文件上传表单，通过request对象无法获得表单项的值，必须通过jspSmartUpload组件提供的Request对象来获取。该类提供如下方法： 

1、getParameter 

作用：获取指定参数之值。当参数不存在时，返回值为null。 

原型：public String getParameter(String name)。其中，name为参数的名字。 

2、getParameterValues 

作用：当一个参数可以有多个值时，用此方法来取其值。它返回的是一个字符串数组。当参数不存在时，返回值为null。 

原型：public String[] getParameterValues(String name)。其中，name为参数的名字。 

3、getParameterNames 

作用：取得Request对象中所有参数的名字，用于遍历所有参数。它返回的是一个枚举型的对象。 

原型：public Enumeration getParameterNames() 

㈣ SmartUpload类这个类完成上传下载工作。 

A．上传与下载共用的方法： 

只有一个：initialize。 

作用：执行上传下载的初始化工作，必须第一个执行。 

原型：有多个，主要使用下面这个： 

public final void initialize(javax.servlet.jsp.PageContext pageContext) 

其中，pageContext为JSP页面内置对象（页面上下文）。 

B．上传文件使用的方法： 

1、upload 

作用：上传文件数据。对于上传操作，第一步执行initialize方法，第二步就要执行这个方法。 

原型：public void upload() 

2、save 

作用：将全部上传文件保存到指定目录下，并返回保存的文件个数。 

原型：public int save(String destPathName) 

和public int save(String destPathName,int option) 

其中，destPathName为文件保存目录，option为保存选项，它有三个值，分别是SAVE_PHYSICAL,SAVE_VIRTUAL和SAVE_AUTO。（同File类的saveAs方法的选项之值类似）SAVE_PHYSICAL指示组件将文件保存到以操作系统根目录为文件根目录的目录下，SAVE_VIRTUAL指示组件将文件保存到以Web应用程序根目录为文件根目录的目录下，而SAVE_AUTO则表示由组件自动选择。 

注：save(destPathName)作用等同于save(destPathName,SAVE_AUTO)。 

3、getSize 

作用：取上传文件数据的总长度 

原型：public int getSize() 

4、getFiles 

作用：取全部上传文件，以Files对象形式返回，可以利用Files类的操作方法来获得上传文件的数目等信息。 

原型：public Files getFiles() 

5、getRequest 

作用：取得Request对象，以便由此对象获得上传表单参数之值。 

原型：public Request getRequest() 

6、setAllowedFilesList 

作用：设定允许上传带有指定扩展名的文件，当上传过程中有文件名不允许时，组件将抛出异常。 

原型：public void setAllowedFilesList(String allowedFilesList) 

其中，allowedFilesList为允许上传的文件扩展名列表，各个扩展名之间以逗号分隔。如果想允许上传那些没有扩展名的文件，可以用两个逗号表示。例如：setAllowedFilesList("doc,txt,,")将允许上传带doc和txt扩展名的文件以及没有扩展名的文件。 

7、setDeniedFilesList 

作用：用于限制上传那些带有指定扩展名的文件。若有文件扩展名被限制，则上传时组件将抛出异常。 

原型：public void setDeniedFilesList(String deniedFilesList) 

其中，deniedFilesList为禁止上传的文件扩展名列表，各个扩展名之间以逗号分隔。如果想禁止上传那些没有扩展名的文件，可以用两个逗号来表示。例如：setDeniedFilesList("exe,bat,,")将禁止上传带exe和bat扩展名的文件以及没有扩展名的文件。 

8、setMaxFileSize 

作用：设定每个文件允许上传的最大长度。 

原型：public void setMaxFileSize(long maxFileSize) 

其中，maxFileSize为为每个文件允许上传的最大长度，当文件超出此长度时，将不被上传。 

9、setTotalMaxFileSize 

作用：设定允许上传的文件的总长度，用于限制一次性上传的数据量大小。 

原型：public void setTotalMaxFileSize(long totalMaxFileSize) 

其中，totalMaxFileSize为允许上传的文件的总长度。 

C．下载文件常用的方法 

1、setContentDisposition 

作用：将数据追加到MIME文件头的CONTENT-DISPOSITION域。jspSmartUpload组件会在返回下载的信息时自动填写MIME文件头的CONTENT-DISPOSITION域，如果用户需要添加额外信息，请用此方法。 

原型：public void setContentDisposition(String contentDisposition) 

其中，contentDisposition为要添加的数据。如果contentDisposition为null，则组件将自动添加"attachment;"，以表明将下载的文件作为附件，结果是IE浏览器将会提示另存文件，而不是自动打开这个文件（IE浏览器一般根据下载的文件扩展名决定执行什么操作，扩展名为doc的将用Word程序打开，扩展名为pdf的将用acrobat程序打开，等等）。 

2、downloadFile 

作用：下载文件。 

原型：共有以下三个原型可用，第一个最常用，后两个用于特殊情况下的文件下载（如更改内容类型，更改另存的文件名）。 

① public void downloadFile(String sourceFilePathName) 

其中，sourceFilePathName为要下载的文件名（带目录的文件全名） 

② public void downloadFile(String sourceFilePathName,String contentType) 

其中，sourceFilePathName为要下载的文件名（带目录的文件全名）,contentType为内容类型（MIME格式的文件类型信息，可被浏览器识别）。 

③ public void downloadFile(String sourceFilePathName,String contentType,String destFileName) 

其中，sourceFilePathName为要下载的文件名（带目录的文件全名）,contentType为内容类型（MIME格式的文件类型信息，可被浏览器识别）,destFileName为下载后默认的另存文件名。 

三、文件上传篇 

㈠ 表单要求 

对于上传文件的FORM表单，有两个要求： 

1、METHOD应用POST，即METHOD="POST"。 

2、增加属性：ENCTYPE="multipart/form-data" 

下面是一个用于上传文件的FORM表单的例子： 

<FORM METHOD="POST" ENCTYPE="multipart/form-data" 
ACTION="/jspSmartUpload/upload.jsp">
<INPUT TYPE="FILE" NAME="MYFILE">
<INPUT TYPE="SUBMIT">
</FORM>
 


㈡ 上传的例子 

1、上传页面upload.html 

本页面提供表单，让用户选择要上传的文件，点击"上传"按钮执行上传操作。 

页面源码如下： 

<!--
    文件名：upload.html
	作  者：纵横软件制作中心雨亦奇(zhsoft88@sohu.com)
-->
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
<head>
<title>文件上传</title>
<meta http-equiv="Content-Type" content="text/html; charset=gb2312">
</head>

<body>
<p>&nbsp;</p>
<p align="center">上传文件选择</p>
<FORM METHOD="POST" ACTION="jsp/do_upload.jsp"
ENCTYPE="multipart/form-data">
<input type="hidden" name="TEST" value="good">
  <table width="75%" border="1" align="center">
    <tr> 
      <td><div align="center">1、 
          <input type="FILE" name="FILE1" size="30">
        </div></td>
    </tr>
    <tr> 
      <td><div align="center">2、 
          <input type="FILE" name="FILE2" size="30">
        </div></td>
    </tr>
    <tr> 
      <td><div align="center">3、 
          <input type="FILE" name="FILE3" size="30">
        </div></td>
    </tr>
    <tr> 
      <td><div align="center">4、 
          <input type="FILE" name="FILE4" size="30">
        </div></td>
    </tr>
    <tr> 
      <td><div align="center">
          <input type="submit" name="Submit" value="上传它！">
        </div></td>
    </tr>
  </table>
</FORM>
</body>
</html>
 


2、上传处理页面do_upload.jsp 

本页面执行文件上传操作。页面源码中详细介绍了上传方法的用法，在此不赘述了。 

页面源码如下： 

<%--
	文件名：do_upload.jsp
	作  者：纵横软件制作中心雨亦奇(zhsoft88@sohu.com)
--%>
<%@ page contentType="text/html; charset=gb2312" language="java" 
import="java.util.*,com.jspsmart.upload.*" errorPage="" %>
<html>
<head>
<title>文件上传处理页面</title>
<meta http-equiv="Content-Type" content="text/html; charset=gb2312">
</head>

<body>
<%
	// 新建一个SmartUpload对象
	SmartUpload su = new SmartUpload();
	// 上传初始化
	su.initialize(pageContext);
	// 设定上传限制
	// 1.限制每个上传文件的最大长度。
	// su.setMaxFileSize(10000);
	// 2.限制总上传数据的长度。
	// su.setTotalMaxFileSize(20000);
	// 3.设定允许上传的文件（通过扩展名限制）,仅允许doc,txt文件。
	// su.setAllowedFilesList("doc,txt");
	// 4.设定禁止上传的文件（通过扩展名限制）,禁止上传带有exe,bat,
	jsp,htm,html扩展名的文件和没有扩展名的文件。
	// su.setDeniedFilesList("exe,bat,jsp,htm,html,,");
	// 上传文件
	su.upload();
	// 将上传文件全部保存到指定目录
	int count = su.save("/upload");
	out.PRintln(count+"个文件上传成功！<br>");
	
	// 利用Request对象获取参数之值
	out.println("TEST="+su.getRequest().getParameter("TEST")
	+"<BR><BR>");

	// 逐一提取上传文件信息，同时可保存文件。
	for (int i=0;i<su.getFiles().getCount();i++)
	{
		com.jspsmart.upload.File file = su.getFiles().getFile(i);
		
		// 若文件不存在则继续
		if (file.isMissing()) continue;

		// 显示当前文件信息
		out.println("<TABLE BORDER=1>");
		out.println("<TR><TD>表单项名（FieldName）</TD><TD>"
		+ file.getFieldName() + "</TD></TR>");
		out.println("<TR><TD>文件长度（Size）</TD><TD>" + 
		file.getSize() + "</TD></TR>");
		out.println("<TR><TD>文件名（FileName）</TD><TD>" 
		+ file.getFileName() + "</TD></TR>");
		out.println("<TR><TD>文件扩展名（FileExt）</TD><TD>" 
		+ file.getFileExt() + "</TD></TR>");
		out.println("<TR><TD>文件全名（FilePathName）</TD><TD>"
		+ file.getFilePathName() + "</TD></TR>");
		out.println("</TABLE><BR>");

		// 将文件另存
		// file.saveAs("/upload/" + myFile.getFileName());
		// 另存到以WEB应用程序的根目录为文件根目录的目录下
		// file.saveAs("/upload/" + myFile.getFileName(), 
		su.SAVE_VIRTUAL);
		// 另存到操作系统的根目录为文件根目录的目录下
		// file.saveAs("c:\\temp\\" + myFile.getFileName(), 
		su.SAVE_PHYSICAL);

	}
%>
</body>
</html>
 


四、文件下载篇 

1、下载链接页面download.html 

页面源码如下： 

<!--
		文件名：download.html
	作  者：纵横软件制作中心雨亦奇(zhsoft88@sohu.com)
-->
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
<head>
<title>下载</title>
<meta http-equiv="Content-Type" content="text/html; charset=gb2312">
</head>
<body>
<a href="jsp/do_download.jsp">点击下载</a>
</body>
</html>
 


2、下载处理页面do_download.jsp do_download.jsp展示了如何利用jspSmartUpload组件来下载文件，从下面的源码中就可以看到，下载何其简单。 

源码如下： 

<%@ page contentType="text/html;charset=gb2312" 
import="com.jspsmart.upload.*" %><%
		// 新建一个SmartUpload对象
	SmartUpload su = new SmartUpload();
		// 初始化
	su.initialize(pageContext);
		// 设定contentDisposition为null以禁止浏览器自动打开文件，
		//保证点击链接后是下载文件。若不设定，则下载的文件扩展名为
		//doc时，浏览器将自动用word打开它。扩展名为pdf时，
		//浏览器将用acrobat打开。
	su.setContentDisposition(null);
		// 下载文件
	su.downloadFile("/upload/如何赚取我的第一桶金.doc");
%>
 


注意，执行下载的页面，在Java脚本范围外（即<% ... %>之外），不要包含HTML代码、空格、回车或换行等字符，有的话将不能正确下载。不信的话，可以在上述源码中%><%之间加入一个换行符，再下载一下，保证出错。因为它影响了返回给浏览器的数据流，导致解析出错。 

3、如何下载中文文件 

jspSmartUpload虽然能下载文件，但对中文支持不足。若下载的文件名中有汉字，则浏览器在提示另存的文件名时，显示的是一堆乱码，很扫人兴。上面的例子就是这样。（这个问题也是众多下载组件所存在的问题，很少有人解决，搜索不到相关资料，可叹！） 

为了给jspSmartUpload组件增加下载中文文件的支持，我对该组件进行了研究，发现对返回给浏览器的另存文件名进行UTF-8编码后，浏览器便能正确显示中文名字了。这是一个令人高兴的发现。于是我对jspSmartUpload组件的SmartUpload类做了升级处理，增加了toUtf8String这个方法，改动部分源码如下： 

public void downloadFile(String s, String s1, String s2, int i)
	throws ServletException, IOException, SmartUploadException
    {
	if(s == null)
	    throw new IllegalArgumentException("File '" + s +
	    "' not found (1040).");
	if(s.equals(""))
	    throw new IllegalArgumentException("File '" + s +
	    "' not found (1040).");
	if(!isVirtual(s) && m_denyPhysicalPath)
	    throw new SecurityException("Physical path is
	    denied (1035).");
	if(isVirtual(s))
	    s = m_application.getRealPath(s);
	java.io.File file = new java.io.File(s);
	FileInputStream fileinputstream = new FileInputStream(file);
	long l = file.length();
	boolean flag = false;
	int k = 0;
	byte abyte0[] = new byte[i];
	if(s1 == null)
	    m_response.setContentType("application/x-msdownload");
	else
	if(s1.length() == 0)
	    m_response.setContentType("application/x-msdownload");
	else
	    m_response.setContentType(s1);
	m_response.setContentLength((int)l);
	m_contentDisposition = m_contentDisposition != null ?
	m_contentDisposition : "attachment;";
	if(s2 == null)
	    m_response.setHeader("Content-Disposition", 
	    m_contentDisposition + " filename=" + 
	    toUtf8String(getFileName(s)));
	else
	if(s2.length() == 0)
	    m_response.setHeader("Content-Disposition", 
	    m_contentDisposition);
	else
	    m_response.setHeader("Content-Disposition", 
	    m_contentDisposition + " filename=" + toUtf8String(s2));
	while((long)k < l)
	{
	    int j = fileinputstream.read(abyte0, 0, i);
	    k += j;
	    m_response.getOutputStream().write(abyte0, 0, j);
	}
	fileinputstream.close();
    }

    /**
     * 将文件名中的汉字转为UTF8编码的串,以便下载时能正确显示另存的文件名.
     * 纵横软件制作中心雨亦奇2003.08.01
     * @param s 原文件名
     * @return 重新编码后的文件名
     */
    public static String toUtf8String(String s) {
	StringBuffer sb = new StringBuffer();
	for (int i=0;i<s.length();i++) {
	    char c = s.charAt(i);
	    if (c >= 0 && c <= 255) {
		sb.append(c);
	    } else {
		byte[] b;
		try {
		    b = Character.toString(c).getBytes("utf-8");
		} catch (Exception ex) {
		    System.out.println(ex);
		    b = new byte[0];
		}
		for (int j = 0; j < b.length; j++) {
		    int k = b[j];
		    if (k < 0) k += 256;
		    sb.append("%" + Integer.toHexString(k).
		    toUpperCase());
		}
	    }
	}
	return sb.toString();
    }

注意源码中粗体部分，原jspSmartUpload组件对返回的文件未作任何处理，现在做了编码的转换工作，将文件名转换为UTF-8形式的编码形式。UTF-8编码对英文未作任何处理，对中文则需要转换为%XX的形式。toUtf8String方法中，直接利用Java语言提供的编码转换方法获得汉字字符的UTF-8编码，之后将其转换为%XX的形式。 

将源码编译后打包成jspSmartUpload.jar，拷贝到Tomcat的shared/lib目录下（可为所有WEB应用程序所共享），然后重启Tomcat服务器就可以正常下载含有中文名字的文件了。另，toUtf8String方法也可用于转换含有中文的超级链接，以保证链接的有效，因为有的WEB服务器不支持中文链接。 

小结：jspSmartUpload组件是应用JSP进行B/S程序开发过程中经常使用的上传下载组件，它使用简单，方便。现在我又为其加上了下载中文名字的文件的支持，真个是如虎添翼，必将赢得更多开发者的青睐。 

-

资料引用:http://www.knowsky.com/3136.html
```

![](C:\Users\Administrator\Desktop\NOTE\source\image\JSP内建动作.gif)

![JSP内置对象](C:\Users\Administrator\Desktop\NOTE\source\image\JSP内置对象.gif)

![动作关联图](C:\Users\Administrator\Desktop\NOTE\source\image\动作关联图.jpg)

![四种范围](C:\Users\Administrator\Desktop\NOTE\source\image\四种范围.gif)　　

### web服务器的基本概念

```
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

### Servlet 的常见参数

![doubleS](C:\Users\Administrator\Desktop\NOTE\source\image\doubleS.gif)

## 1.10  关于servlet线程

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
15254132324

```

###   

## 1.11 Cookie和session详细解析

```

1 什么是cookie 

cookie 历来指就着牛奶一起吃的点心。然而，在因特网内，“cookie”这个字有了完全不同的意思。那么“cookie”到底是什么呢？“Cookie”是小量信息，由网络服务器发送出来以存储在网络浏览器上，从而下次这位独一无二的访客又回到该网络服务器时，可从该浏览器读回此信息。这是很有用的，让浏览器记住这位访客的特定信息，如上次访问的位置、花费的时间或用户首选项（如样式表）。Cookie 是个存储在浏览器目录的文本文件，当浏览器运行时，存储在 RAM 中。一旦阁下从该网站或网络服务器退出，Cookie 也可存储在计算机的硬驱上。I3I.net的打算是，当访客结束其浏览器对话时，即终止I3I.net的所有 cookie。 


2 Cookie 有哪些用途

Cookie 的用途之一是存储用户在特定网站上的密码和 ID。另外，也用于存储起始页的首选项。在提供个人化查看的网站上，将要求阁下的网络浏览器利用阁下计算机硬驱上的少量空间来储存这些首选项。这样，每次阁下登录该网站时，阁下的浏览器将检查阁下是否就该唯一的服务器有任何预先定义的首选项（cookie）。如果有的话，浏览器将此 cookie 随阁下对网页的请求一起发送给服务器。Microsoft 和 Netscape 使用 cookie 在其网站上创建个人起始页。各家公司利用 cookie 的一般用途包括：在线定货系统、网站个人化和网站跟踪。 

网站个人化是 cookie 最有益的用途之一。例如，当谁来到 CNN 网站，但并不想查看任何商务新闻。网站允许他将该项选为选项。从那时起（或者直到 cookie 逾期），他在访问 CNN 网页时将不会读到商务新闻。 


3 这些 Cookie 是如何起作用的

文档的 HTML 代码中的命令行告诉浏览器设置某一名称或数值的 cookie。以下是用来设置 cookie 脚本的一个普通实例。 
Set-Cookie: name = VALUE; 
expires = DATE; 
path = PATH; 
domain = DOMAIN_NAME; 


4 那么安全性如何

HTTP Cookie 不能用来从阁下的硬驱上检索个人数据、放置病毒、得到阁下的电子邮件地址或偷窃有关阁下身份的敏感信息；然而，HTTP Cookie 可用来跟踪阁下在特定网站上的所到之处。不使用 cookie 就很难进行网站跟踪。 
至于其他一切与因特网有关的事，如同阁下所希望的那样是匿名的。没有网站知道阁下是谁，除非阁下自己透露给网站。同时，cookie 只是为了更好地了解使用模式并改进网站访客的效率而采用的一个网站跟踪统计手段而已。 
如果网站设计师旨在使网页能与访客更具互动作用，或者若设计师计划让访客自定义网站的外观，则就需要使用 cookie。而且，如果阁下想要网站在某些情况下改变其外观，cookie 则提供了一条快速、容易的途径，让阁下的 HTML 页面按需要而改变。最新型的服务器使用 cookie 有助于数据库的互动性，进而改进网站的整体互动性。 


 ---------------------------------------------------------------------------------------------

Session

       在计算机中，尤其是在网络应用中，称为“会话”。

　　Session直接翻译成中文比较困难，一般都译成时域。在计算机专业术语中，Session是指一个终端用户与交互系统进行通信的时间间隔，通常指从注册进入系统到注销退出系统之间所经过的时间。 

　　具体到Web中的Session指的就是用户在浏览某个网站时，从进入网站到用户退出所经过的这段时间，也就是用户浏览这个网站所花费的时间。因此从上述的定义中我们可以看到，Session实际上是一个特定的时间概念。


指定 Cookie的有效时间，以秒为单位，MaxAge可以有三种方式设置

<1> 正数：有效时间，从当前时间开始向后推移，并且把信息写到硬盘中，超出时间表示 Cookie失效，即 Cookie过期后，key,value,domin,age,path等
相关值就得不到了，但 Cookie保存在硬盘中不会销毁

<2> 负数：当前 Cookie采用的是会话 Cookie，信息只是保存在浏览器缓存中，但片段不会写到硬盘中，HttpSesesion没有负数

<3> 0：销毁 Cookie，要立刻销毁当前应用域在客户本地硬盘中创建的 Cookie文件
```

- Cookie原理

![cookie](C:\Users\Administrator\Desktop\NOTE\source\image\cookie.gif)

![session(1)](C:\Users\Administrator\Desktop\NOTE\source\image\session(1).gif)

![session(2)](C:\Users\Administrator\Desktop\NOTE\source\image\session(2).gif)