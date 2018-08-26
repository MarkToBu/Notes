# servlet

## 代码错误解决方案

```
  	<!-- 
  		web工程错误代码解决方案
  			注意：html不允许有存在划黄线的标签
  			html修改不需要关闭Tomcat
  			Java代码和web.xml修改必须关闭Tomcat 修改好之后重新开启Tomcat
  			
  			404：
  				404：后面没有跟随路径
  					表示Tomcat开启失败
  						失败原因
  						a) web.xml配置错误
  						b）Tomcat已经开启了一个
  						c）Tomcat文件缺失
  				后面跟着路径
  					我发找到资源 action提交路径与URL-pattern 路径不匹配
  					标准路径出现错误 http：//localhost:8080/工程名/页面地址
  			
  			500：Java代码编译出错
  				或者在web.html文件中的<servrlt-class>节点下没有找到
  				servlet的是实例地址
  				can not found ** Class Exception
  			
  			405：get --doPost()
  				post --doget()
  				method属性书写错误--提交表单
  	 -->
  </body>
</html>

```

## web.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd" id="WebApp_ID" version="3.0">
  <display-name>ServletDay1_base</display-name>
  
  
  <servlet>
  	<!-- 
  		3）对应 servlet-padding节点下的servlet-name的值，由此找到servlet具体的路径开始执行
  	 -->
  	<servlet-name>suibianxie</servlet-name>
  	<!-- 
  		4) 这个节点写的是具体的路径 可以按住ctrl键跳转到servlet服务中 跳转不过去 路径错误
  	 -->
  	<servlet-class>com.etoak.servel.Login</servlet-class>
  </servlet>
  
  <servlet-mapping>
  	<!-- 
  		2）此节点对应servlet下的servlet-name的值，可以随便写，但必须一样
  	 -->
  	<servlet-name>suibianxie</servlet-name>
  	<!-- 
  		1) 这块节点对应表单或者链接提交的URL地址
  			当Tomcat开启之后，开始监听这个节点配置的URL路径，
  			如果表单提交的路径与之匹配 则开始拦截 此处多了一个/ 反斜杠的意思是代表根目录
  			拦截成功之后找到servlet-name下的值 
  	 -->
  	<url-pattern>/servlet/Login</url-pattern>
  </servlet-mapping>
  
  
  <!-- 第二对  -->
  <servlet>
  	<servlet-name>Reg</servlet-name>
  	<servlet-class>com.etoak.servel.Reg</servlet-class>
  </servlet>
  <servlet-mapping>
  	<servlet-name>Reg</servlet-name>
  	<url-pattern>/servlet/Reg</url-pattern>
  </servlet-mapping>
  
  <!-- 第三对  -->
  <servlet>
  	<servlet-name>ShowAll</servlet-name>
  	<servlet-class>com.etoak.servel.ShowAll</servlet-class>
  </servlet>
  <servlet-mapping>
  	<servlet-name>ShowAll</servlet-name>
  	<url-pattern>/servlet/ShowAll</url-pattern>
  </servlet-mapping>
  
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>default.jsp</welcome-file>
  </welcome-file-list>
</web-app>
```

## servlet的跳转

### [1]  请求转发

```Java
servlet执行请求转发之后，跳转到另一个servlet或者页面，由于是servlet转发到目的地，但是浏览器不知道，所以浏览器的地址栏依然显示的是跳转之前的路径
servlet里面的方法跳转之前是do*** 跳转之后还是***
因为是同一次请求，所以Request范围依旧有效
请求转发没有固定的路径书写格式
	如果经历过请求转发，则之后的跳转路径全部书写为绝对路径
request.getRequestDispatcher("Show").forward(request,response);
```

### [2] 重定向

```java
servlet返回响应，浏览器重新地位到新的目的地 浏览器发送了两次请求 不再是同一种请求，Request的范围失效
跳转之前的方法是do***跳转之后的是doget
浏览器的地址栏显示的最终的路径地址
重定向的跳转路径一般是绝对路径 ---/工程名/servlet/类名 || /工程名/***.html || /工程名/***.jsp
response.sendRedirect("绝对路径");
```

![两种跳转方式](D:\yitupeixun\java课程\前端\Servlet\ServletDaysOfficial\ServletDay3\attachment\两种跳转方式.jpg)

## servlet的四种范围及其局限性

### [1] PageContext

​	跳转之后立刻失效

### [2] HttpServletRequest

​	1 请求转发依然有效，因为是同一请求

​	2 重定向之后无效，因为不是同一请求

### [3] HttpSession

​	只要不超过最大的活动周期，依然有效

### [4] ServletContext

​	只要Tomcat没有关闭，依然有效

![四种范围](D:\yitupeixun\java课程\前端\Servlet\ServletDaysOfficial\ServletDay3\attachment\四种范围.jpg)

## servlet的两种跟踪机制

### [1] cookie

```
是由客户端浏览器提供的提供的一种回话跟踪机制，用来保存数据，这些数据可以用来设置用户的权限。并且，cookie存在实体，最终保存在用户的本地，我们可以找到cookie的文件，默认不支持中文
Cookie cookie  = new Cookie(String,String);
创建Cookie之后，设置生命周期，根据生命周期开始倒计时，到期cookie销毁，内部数据无法保存。在此期间用户关闭计算机，浏览器的都不会影响cookie的生命周期
```

### [2] HttpSession

```
是由服务器提供的一种会话跟踪机制，安全性较cookie高，没有实体，不存在一个文件保存在客户端或者服务器，Session通过Request.getsession() 来进行创建，创建之后存在一个最大不活动周期默认是1800s(这个是可以修改的)之后，开始倒计时，如果在这期间内，用户提交了申请则倒计时重置，因此理论上Session是可以一直有效的，但是如果，关闭浏览器或者在最大不活动周期内没有请求提交，则Session销毁Session支持中文，通过setAttribute(String,Object)来封装信息
```

### [3] 如何销毁

#### [1] cookie

```java
1）生命周期到期
2）cookie.setMaxAge(0)--设置为0时销毁
```

#### [2] Session

```java
1）超过最大不活动周期(不同的计算机存在误差)
2）Session.invalidate()--这是立即销毁当前的Session
3）关闭浏览器--但是关闭浏览器并不是严格意义上的删除Session，而是当我们再次打开浏览器的时候会再次分配一个Session，之前的Session再也无法拿取
```

### [3] 他们之间的联系

```java
Session的传输是依靠cookie来进行维持的，cookie传输了Session的一个重要的参数Sessionid(这是唯一的Session的标识)
当用户书写Request.getSession()的时候，首先web容器首先会从cookie中查看是否当前存在Sessionid。如果，没有存在Sessionid，则创建一个新的Session，并分配给这个Session一个Sessionid，跳转之前将Sessionid封装进当前的cookie中
Cookie cookie = new Cookie("jsessionid","dfddfhs3423dhufs934");
当跳转到另一个页面中，当用户书写Request.getSession() 这个方法的时候，再次从cookie中拿取Sessionid，如果可以拿取，则根据拿取的Sessionid获得绑定的Session会话
```

### [4] Cookie禁用Session还能维持吗

```
如果想维持Session，可以使用重写URL的方法来继续维持Session的跟踪机制
```

```java
String path="/工程名/servlet/类名";
String newpath =response.encodeURL(path);
response.sendRedirect(newpath);
	必须每一个Servlet都得重新写一遍
```

# Jsp

## jsp指令元素

### [1] page元素属性

```
language:页面脚本类型,书写代码的种类
	1 import:页面导包，多个包用逗号隔开不推荐使用快捷键
	2 pageEncoding:页面转换编码，如果使用默认的iso-8859-1无法保存中文
	3 session:打开页面是否创建session支持session会话跟踪机制，如果为true，则支持
	4 buffer:页面缓存 默认8kb
	5 info:用来设置页面的签名 书写时间等信息，通过getServletInfo()方法可以取出
	6 isELIgnored:是否忽略EL表达式，false不忽略
	7 contentType:对应Servlet中response.setContentType("text/html;charset=utf-8")是软编码的一部分
	8 autoFlush:页面是否支持自动刷新，默认不支持
	9 isThreadSafe:当前页面线程是否安全默认true，不安全单实例多线程。更改为false 则底层实现			          	SingleThreadModel单实例单线程线程安全效率低下
	10 isErrorPage:当前页面能否使用exception内置对象true可以使用 false无法使用
	11 errorPage：当前页面如果出现异常，自动跳转到哪个页面
```

### [2] include元素属性

```
1 file：url地址
```

### [3] taglib元素属性

```
1 prefix:设置二代标签的开头
2 uri:"http://java.sun.com/jsp/jstl/core"
```

## El表达式

凡是称之为表达式的都是只仅仅显示数据，无法进行复杂的业务逻辑

```Java 
1 格式：
	${要展示的数据}
		大括号内存在引号，则表示字符创直接输出
2 算数运算
	${"3+1"}=${3+1}
3 关系运算
	${3>1&&2<1}
4 三目运算符
	${3>1?true:false}
5 范围取值
	<%
		pageContext.setAttribute("elena","elenaPage");
		request.setAttribute("elena","elenaRequest");
		session.setAttribute("elena","elenaSession");
		application.setAttribute("elena","elenaApplication");
	%>
    格式：${范围.key} 范围 pageScope requestScope sessionScope applicationScope 在key值没有冲突的情况下，可以省略范围
    ${pageScope.elena}
	${requestScope.elena}
	${sessionScope.elena}
	${applicationScope.elena}
	如果不指定范围，则会默认输出最小现存范围
6 接收页面传递过来的值
	<%=request.getParameter("etoak") %>
	${param.etoak}
	格式：${param.etoak} 注意param就是getparameter的简写 这里永远不能简写
7 获取配置在web.xml中的全局参数
	<%=application.getInitParameter("今天是五月五号") %>
    ${initParam.今天是五月五号}--这里的initParam不能省略
8 拿取实体类的属性
	<%=((Hero)session.getAttribute("myhero")).getName() %>
    ${sessionScope.myhero.name}
9 注意EL表达式并不一定能显示任意数，必须从key值中拿取，如果没有任何数据，则页面中什么也不显示
```

## Jsp的二代标签

```java
1 先开始
	<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
2 赋值
	<c:set var="elena" value="elenaPage" scope="page"></c:set>
	格式:var:key值 value:value值  scope:范围  page request session application
3 取值
	<c:out value="直接输出字符串"></c:out>
	<c:out value="${pageScope.elena}"></c:out>
	<%String str = "我是java代码"; %>
	<c:out value="<%=str %>"></c:out>
	格式:value:要输出的值,可以使用java代码，可以直接书写字符串，可以使用EL表达式
4 删除值
	<c:remove var="elena" scope="request" />
	<c:out value="${requestScope.elena}" default="删除成功 "></c:out>
	格式:var:要删除的key,scope:要删除的范围，注意如果不指定相应范围，则默认删除.所有匹配key值的,default:如果无法取值,默认输出的值
5 流程控制
	<c:if test="${3>1}">
		<c:out value="输出这里~~~~"></c:out>
	</c:if>
	注意：test后面如果为true，则输出c：out里面的
	${empty key} 空验证 如果可以取出值来返回false 无法取值返回true
	<c:choose>
		<c:when test="${3>1}">
			<c:out value="输出这里啦！！"></c:out>
		</c:when>
		<c:otherwise>
			<c:out value="最终输出了这里！！"></c:out>
		</c:otherwise>
	</c:choose>
	注意：c:choose和c:when必须在一起
6 foreach遍历
	<c:forEach items="${requestScope.mylist}" var="etoak" step="2">
			<tr>
				<td><c:out value="${pageScope.etoak.id}"></c:out></td>
				<td><c:out value="${etoak.name}"></c:out></td>
				<td><c:out value="${etoak.pass}"></c:out></td>
			</tr>
	</c:forEach>
	格式：items中的是要遍历的数据 var中的key值 step="2"的话是隔一个遍历
```

## Jsp的一代标签

```Java
1 jsp不需要引入指令元素
2 创建一个对象
	<jsp:useBean id="u" class="com.etoak.po.User" scope="request">
	注意：ID是新建对象的key值，注意这是一个key值，通过这个key值我们就知道了对象名，因为规定二者重名
		  class表示新建对象的类
		  Scope表示新建对象的范围
	这个标签相当于 User u = new User();  request.setAttribute("u",u);
3 给属性赋值
	<jsp:setProperty property="*" name="u" value="我是设置好的用户名" param="etoakusername"/>
    property：表示给那个属性赋值  如果实体类的属性值和提交过来的key正好一一对应，可以用*表示个所有的属性赋值 name 对应usebean标签的id属性 value表示事先设置好的 param对应提交的key值--当key值与实体类的属性值不一致的时候
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
3 请求转发
	<jsp:forward page="suc.jsp">
				<jsp:param value="thisisvalue" 
				name="thisiskey" />
	</jsp:forward>
	注意:request.getRequestDispatcher("suc.jsp?thisiskey=thisisvalue").forward(request,response);
4 重定向
	<c:redirect url="err.jsp">
				<c:param name="thisiskey"
				value="thisisvalue"></c:param>
	</c:redirect>
```

### 注意：

```
1 jsp的内键动作中的usebean这个标签肯定会创建一个新的对象么？如果不？请说名理由？
	1）首先会根据id提供的key值和Scope提供的范围，从内存中拿取，如果拿取的到，那么就拿取已经存在的，如果不存在就创建一个新的的对象，并且赋值范围
	2）这里受到范围的影响
		pageContext 范围肯定新建一个对象
		request	重定向会新建一个对象
		session 超过最大不活动周期或者session销毁
		重新创建一个对象
		application 永远不会创建对象
```

### 分页

```Java
1 分页的种类
	假分页
		一次将用户的所有的数据全部取出，用户想要那些，就显示那些
	真分页
		用户需要那些数据。就从数据库中取出那些数据
2 分页的公式
	mysql语句: select * from 表名 limit x,y;
	x:起始索引值
	y：一共显示几条数据
3 分页四要素
	1）总记录数     dao层
	2） 每页记录数  自己设置
	3）总页数       （总记录数+每页记录数-1）/每页记录数
	4）当前页         默认是1 ----这个参数不是常量而是变量
4 分页四元素和分页公式
	select 字段  from 表 limit （当前页-1）*每页记录数，每页记录数
	eg:10条记录	每页3条 分4页
		page1:	1~3		0,3		(1-1)*3,3
		page2:  4~6     3,3     (2-1)*3,3
		page3:  7~9     6,3     (3-1)*3,3
	List<Computer> list = dao.getPage((当前页-1)*每页记录数,每页记录数);
```

### 上传

```
1 上传操作必须使用post没有大小限制
2 enctype：设置表单上传时的流的类型 ---multipart/form-data表示字节流    ----application/x-www-form-urlencoded
```

# ajax

## 上传的步骤

```
1 创建异步请求--注意浏览器差异性
	新的浏览器--request = new XMLHttpRequest();---level2版本
	老牌浏览器--if(window.XMLHttpRequest){
					request = new XMLHttpRequest();
				}else{
					request = new ActiveXObject("Microsoft.XMLHttp");
				}---level1版本
2 发送异步请求到指定位置
		request.open(method, url, async, username, password)
		method--发送请求的类型 get或者post 
		url--发送请求的目的地
		async--默认为true表示使用异步浏览器 false为使用同步浏览器
		username和password 表示web容器，开启安全策略，则必须填写用户名和密码
3 如果是post则需要写 request.setRequestHeader("Content-Type","application/x-www-form-urlencoded");
	使用字符流
4 request.onreadystatechange = callback;---声明回调函数 这里表示在readystate发生改变的时候执行回调函数
5 设置发送的值--get不需要从这传值 如果不需要传值填写null
	request.send("etoak="+value);
```

## 回调的步骤

```
1 首先判断返回的响应是完整的
	if(request.readyState==4)
2 判断服务器有没有出现异常
	if(request.status==200)
3 接收返回的值
	var value = request.responseText;
```

## 通过xml封装

```java
1 MIME类型需要是：text/xml
2 需要创建文档对象模型 Document doc = DocumentHelper.createDocument();
3 创建根目录 Element root = doc.addElement("root");
4 通过根目录创建一级子元素 Element child = root.addElement("firstChild");
5 设置一级子元素的属性 child.addAttribute("key","value");
6 通过一级子元素创建二级子元素 Element second  = child.addElement("secondChild");
7 设置二级子元素的值 second.setText("值");
```

## 注意

回调的步骤可以写在上传步骤的第四个步骤，这样就成了匿名函数，经常用这种方式

```javaScript
function showProvince(){
    create();
    request.open("post","servlet/ShowProvince",true);
    request.setRequestHeader("Content-Type","application/x-www-form-urlencoded");
    request.onreadystatechange = function(){
    	if(request.readyState==4){
    		if(request.status==200){
    				var doc = request.responseXML;
    				var array = doc.getElementsByTagName("province");
    				var str = "";
    				for(var i =0;i<array.length;i++){
    						var stu = array[i];
    						var name = stu.getAttribute("name");
    						var id =stu.getAttribute("id");
    						str += "<option value="+id+">"+name+"</option>";
    					}
    					var dom_op = document.getElementById("province");
    					dom_op.innerHTML += str;
    				}
    			}
    		};
    		request.send(null);
    	}
```

# JSON

含义：JSON其实就是特殊格式的字符串，可以直接打印 ，专门用来封装数据的，取代了xml

MIME：text/plain

## 两种格式

```
1 {key:value,key:value,key:value}
	key:必须是字符串  value:八种基本数据类型  字符串 自定义数据类型 数组 集合 null JSON
2 [value,value,value,value]
	value:八种基本数据类型  字符串 自定义数据类型 数组 集合 null JSON
```

## 封装方式

```java
1 可以封装任意数据类型，封装之后呈现第一种格式
		int i = 10;
		boolean flag = true;
		String str = "etoak";
		String[] strAr = {"大明湖","趵突泉","千佛山"};
		
		JSONObject jo = new JSONObject();
		jo.put("key1",i);
		jo.put("key2",flag);
		jo.put("key3",str);
		jo.put("key4",strAr);
		//{"key4":["大明湖","趵突泉","千佛山"],"key3":"etoak","key2":true,"key1":10}
		System.out.println(jo);
2 专门用来封装自定义数据类型和Map，封装之后是第一种格式
		JSONObject jo2 = new JSONObject();
		jo2.put("mygame",new Game("炉石传说",0,"暴雪"));
		//{"mygame":{"total":0,"name":"炉石传说","seller":"暴雪"}}
		System.out.println(jo2);
		Map<Integer,String> map =new HashMap<Integer,String>();
		map.put(1, "山东鲁能");
		map.put(2, "广州恒大");
		map.put(3, "上海上港");
		map.put(4, "北京国安");
		JSONObject js1 = JSONObject.fromObject(map);
		//{"3":"上海上港","2":"广州恒大","1":"山东鲁能","4":"北京国安"}
		System.out.println(js1);
		3 用来封装数组，List和Set，封装之后，呈现第二种格式
		JSONArray ja = JSONArray.fromObject(strAr);
		//["大明湖","趵突泉","千佛山"]
		System.out.println(ja);
		
		List<String> list = new ArrayList<String>();
		list.add("Java");
		list.add("C");
		list.add("C++");
		list.add("Php");
		JSONArray ja2 = JSONArray.fromObject(list);
		//["Java","C","C++","Php"]
		System.out.println(ja2);
		
		Set<String> set = new HashSet<String>();
		set.add("济南");
		set.add("青岛");
		set.add("淄博");
		set.add("济宁");
		JSONArray ja3 = JSONArray.fromObject(set);
		//["青岛","济宁","淄博","济南"]
		System.out.println(ja3);
```

# jQuery

## 四大核心函数

```
1 $(sel context)
	在括号内书写选择器，如果存在第二个参数，表示从此参数设置的范围中根据选择器选取，没有第二个参数，从全文选取
2 $(html)
	直接在括号中书写HTML标签，配合方法使用
3 $(dom)
	内部直接传输dom元素，转换成jQuery格式
4 $(document).ready(function(){
  		此函数表示全文加载完毕，之后如果有绑定事件则所有函数都放在此函数中
	});
	简略写法：
	$(function(){})；
```

### 注意

```
ready函数和onload方法有什么不同
	1 onload()-全文只执行一次-存在多个只执行最后一个
	  ready--从上到下依次执行
	2 ready()表示全文的结构加载完毕即可
	  onload() 一切要素必须加载完成
```

## 注意

```javascript 
1 js元素和jQuery元素是同一个元素么 ？如果不是请说明原因？它们之间如何相互转化？
	它们不是同一个元素 
	原因：因为jQuery会对js元素进行轻微的封装 	dom_lb != $jq_lb
		<label id="test">测试</label>
			Js:
				var dom_lb = document.getElementById("test");
							||
				<label id="test">测试</label>
			jQuery:
				var $jq_lb = $("#test");
						||
				[<label id="test">测试</label>]
      转化: jQuery~~~~~~>js
                 $jq_input = dom_input;
                 $jq_input.get(0) = dom_input;
            js~~~~~~~~~~>JQuery
                 $(dom_input) = jq_input;
```

## 选择器

### [1] 基本选择器

```
1 ID选择器---根据给定的ID匹配的一个元素
	$("#idname")
2 类别选择器 --根据给定的类匹配的元素
	$(".className")
3 标签选择器
	$("tagName")
4 交集选择器
	$("tagName.class")--两个条件缺一不可
5 并集选择器
	$("sel1,sel2,sel3")--符合一个就行
6 后代选择器
	$("sel sel sel")
7 全局选择器
	$("*")
8 获取第一个元素
	$(":first")
9 获取最后一个元素
	$(":last")
10 获取索引值是index的元素，索引值从零开始
	$(":eq(index)")
11 获取索引值大于index的元素
	$(":gt(index)")
12 获取索引值小于index的元素
	$(":lt(index)")
13 获取索引值是奇数的元素
	$(":odd")
14 获取索引值是偶数的元素
	$(":even")
15 获取标题元素
	$(":header")
16 获取无法被选取的元素
	$("not:(sel)")------$("input:not(':checked')").length
```

### [2] 层级选择器

```
1 拿取指定元素的子元素--仅仅拿取子元素
	$("sel1 > sel2")
2 向下选取 必须紧邻 互为兄弟
	$("sel1 + sel2")
3 向下选取 互为兄弟
	$("sel1 ~ sel2")
```

### [3] 表单选择器

```
1 拿取整个表单
	$(":input")
2 拿取匹配的单行文本框
	$(":text")
3 拿取匹配的密码文本框
	$(":password")
4 拿取匹配的单选框
	$(":radio")
5 拿取匹配的多选框
	$(":checkbox")
6 拿取匹配的提交按钮
	$(":submit")
7 拿取匹配的取消按钮
	$(":reset")
8 拿取匹配的按钮
	$(":button")
9 拿取匹配的图像
	$(":image")
10 拿取input所有可用的元素
	$("input:enabled")
11 拿取input中所有的不可用元素
	$("input:disabled")
12 拿取input中所有选中的元素
	$("input:checked")
13 拿取所有选中的option元素
	$("select option :selected")
```

### [4] 基本选择器

```
1 拿取包含特定文本的元素--仅仅只是包含--text():在js中为innerText，用来设置元素中的文本
	$("li:contains(text)").text()
2 拿取空元素---元素内部没有文本也没有子元素
	$(":empty").html()--可以支持超文本
3 拿取匹配选择器的元素
	$(":has(选择器)")
4 拿取隐藏的元素
	$(":hidden")
5 拿取显示的元素
	$(":visible")
```

### [5] 属性选择器

```
1 拿取含有属性名等于属性值的元素--必须完全相同
	$("[属性名=属性值]")
2 拿取含有属性名并且以属性值开头的元素
	$("[属性名^=属性值]")
3 拿取以属性值结尾的元素
	$("[属性名$=属性值]")
```

## 方法

### [1] 基本常用方法

```
1 设置元素的样式
	$().css("属性名","属性值");
2 拿取或者设置超文本
	$().text()--不传参表示拿取文本--->如果多个文本则合并为一个文本
	$().html()--不传参表示拿取超文本
	$().text(要放在元素中的文本)
	$().html(要放在元素中的超文本)-->如果元素中原本存在内容，则被覆盖
3 设置或者拿取元素属性
	$().attr("属性名","属性值");--设置修改元素属性
	$().attr("属性名");---拿取元素属性
4 向匹配的元素中追加内容
	$(A).append(B)--A为父元素，B为子元素 A内部原先的子元素不受影响，B追加在原先的子元素之后
	$(A).appendTo()--B为父元素，A为子元素 B内部原先的子元素不受影响，A追加在原先的子元素之后
5 在匹配元素之前插入内容
	$(A).before(B)--向A前面插入B
6 在匹配元素之后插入内容
	$(A).after(B)--在A的后边插入B
7 替换匹配的元素
	$(A).replaceWith(B)--A被B替换了，A不存在了
	$(A).replaceAll(B)--A替换了B，B不存在了
8 遍历元素
	$().each(function(index){
      遍历
      $()--一般拿取循环体
      index--表示索引值从0开始
	});
	案例—$("ul#myul li").each(function(index){
			if(index==3){
				/*
					this:表示已经通过选择器
					拿取的元素
					addClass()添加类
					removeClass()删除类
					toggleClass()如果存在某个类
					删除，反之添加
				*/
				$(this).addClass("red");
			}
		});
9 复制元素--被复制的元素具有相同的结构 样式和动作
	$().clone();
```

## 事件

```事件
1 为元素绑定多个事件
	$(sel).bind("事件1 事件2 事件N",function(){
      只要满足其中一个事件，就可以激发后面的匿名函数
	});
2 为元素绑定一个事件
	$(sel).事件(function(){
      
	});
案例------------------------------------------------
3 失去焦点
	$(":text").blur(function(){
		/*
			val():拿取元素的value属性
		*/
		if($(this).val().length<4){
			$("label#name_msg")
			.html("<font color='red'>用户名不能小于4位</font>");
		}
	});
4 鼠标滑过			
	$("div#mydiv").hover(
		//鼠标滑过执行此函数
		function(){
			$(this).removeClass();
			$(this).addClass("red");
		},
		//鼠标滑出执行此函数
		function(){
			$(this).removeClass();
			$(this).addClass("blue");
		}
	);
5 为元素绑定多个事件--只激发一次
	$(sel).one("事件1 事件2 事件N",function(){
		只要满足其中任意一个事件就可以激发后面的匿名函数,仅仅激发一次
	});
6 change()事件
	
```

## 破坏性操作

```javascript
1 含义：当用户执行某个方法的时候，执行之后拿取的元素与之前通过选择器拿取的元素发生了变动，那么我们称这个方法			为破坏性操作
2 $(this).addClass("highlight").children("a").slideDown().end().siblings().removeClass("highlight")
		.children("a").slideUp();
		show():显示		end()：返回破坏性操作之前的元素列表，如果没有破坏性操作返回null
		slideDown():卷帘显示		slideUp():卷帘隐藏
-------------------------------------------------------------------		
3 破坏性操作：
		1):children("元素名")：拿取元素的特定子元素
		2):siblings("元素名"):拿取元素除自己之外的兄弟元素
		3):add("元素名")：动态生成一个元素并把它添加至匹配的元素中
		4):andSelf("元素名"):选择先前所选的加入当前元素中
			$("div").find("p").andSelf().addClass("border");
		5):filter("元素名"):筛选出与指定表达式匹配的元素集合--筛选
		6):find("元素名"):搜索与指定表达式匹配的元素--查找
		7):next("元素名"):取得与匹配元素紧邻的后面同辈元素的集合--也可以是一个元素(紧邻)
		8):nextAll("元素名"):查找当前元素之后的所有同级元素
		9):parent("元素名"):取得包含着所有匹配元素的唯一父元素的集合
		10):parents("元素名"):
		11):not("元素名"):删除与指定表达式匹配的元素
		12):prev("元素名"):取得一个包含匹配的元素集合中每一个元素紧邻的前一个同辈元素的元素集合。
		13):prevAll("元素名"):取得匹配元素之前的同级元素
		14):slice("元素名"):选取匹配的子集
```

## ajax(发送异步请求[jQuery])

```
1 $.ajax(url,type,data,async,dataType,suceess:function(){},error:function(){});
	url:异步请求的目的地
	type:发送异步请求的数据类型
	data:传输的格式
	async:是否使用异步
	dataType:返回的数据类型--支持五种数据类型: text script json html xml
	success:function(){}:服务器无误时的回调函数
	error:function(){}:服务器有错时的回调函数
	注意：dataType设置为json时,jQuery将自动返回的标注json字符串转换成js对象
		原先的key值变为属性名，原先的value值变为属性值，不需要我们导入转换器进行转换
2 $.post(url,data,callback);
	url:异步请求的目的地
	data:要发送的值 post和get 都可以在此处传值 格式:{key:value,key2:value2} 此处不传值可以不写，也可以写null
	callback：回调函数，这个回调函数仅可以处理服务器无误的情况，可以支持五种数据类型 text script xml json html
3 $.get(url,data,callback,type);
	url:异步请求的目的地
	data:要发送的key和value参数
	callback:服务器无误时的回调函数
	type:返回内容的格式--xml html script json text _default
```

