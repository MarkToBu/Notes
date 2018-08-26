## 乱码问题--- 中文问题和页面传值问题

```java
Servlet Jsp默认都不支持中文

	Servlet:
	
	post 软编码
			response.setContentType("text/html;charset=编码");
			request.setCharacterEncoding("编码")
		
	get 软编码
		A):在tomcat的conf文件中修改参数
		server.xml文件中添加
		useBodyEncodingForURI="true"
		B)使用硬编码
		
		public void doGet(HttpServletRequest request,
		HttpServletResponse response)throws Exception{
			软编码
			
			String name = request.getParameter("key");
			String newName = change(name);
		}
		
		public String change(String old){
			try{
				return new String(old.getBytes("iso-8859-1"),"编码");
			}catch(Exception ex){
				ex.printStackTrace();
				return null;
			}
		}
		
	Jsp:
	
		在page指令元素中添加
		contentType="text/html;charset=utf-8"
		
		在指令元素外添加Java代码
		<% request.setCharacterEncoding("utf-8") %>
		以上组成软编码
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
A):从页面通过链接或者表单传递值（get使用?传值post使用消息体）

	html jsp ~~~~~~~~~~~~~~~~~~~~~~~~~~~~>Servlet Action
	
	链接
	
	表单	?key=value&key2=value2              request.getParameter("key")
											${param.key}
											
B):从服务器当中传递值，或者从服务器发送值到页面

	Servlet ~~~~~~~~~~~~~~~~~~~~~~~~~~~~》Servlet
	Servlet ~~~~~~~~~~~~~~~~~~~~~~~~~~~~》Jsp
	
	四范围传值	
	pageContext跳转立刻失效
	request重定向失效因为不是同一次链接
	session如果销毁则失效
	application web容器不关闭永远不销毁
									
	setAttribute(String,Object)             getAttribute(String)
	
	Servlet	负责接收请求，调用数据库，进行业务逻辑
	将加工之后数据发送到页面，不适合进行页面输出工作		
	
	Jsp 接收请求，输出数据，不适合进行业务逻辑工作例如
	调用数据库等

	MVC
	Model   	JavaBean  JDBC
	View		Jsp html js css jquery ajax 
	Controll 	Servlet Action
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
四范围使用的场合：

	pageContext:一般不用来传递值跳转立刻失效
	request:仅仅存在与一次请求当中，一锤子的买卖
	A~~~》B
	session:一般用来封装权限信息，可以在多处使用
	A~~~》B~~~》C~~~》D
	application:封装全局参数，整个工程中任何
	地方都可以拿取，但是只有一份，web容器不关闭
	永远可以拿取


	A:Servlet
		List<Student> list = dao.getAll();
		request.setAttribute("mylist",list);
		request.getRequestDispatcher("../B")
		.forward(request,response);
	B:Jsp
		<c:foreach items="${mylist}" var="etoak">
			<c:out value="${etoak.属性名}"/>
		</c:foreach>
	C:Servlet
		不能从request范围拿取了
```





## 创建数据库的时候，插入中文错误

建表时，加入编码，

```
jdbc:mysql:///et1803?useUnicode=true&amp;characterEncoding=UTF-8
```



```
DROP DATABASE IF EXISTS book;
 
CREATE DATABASE  book DEFAULT CHARACTER SET utf8 COLLATE utf8_bin;
 
 
use book;

create table category(
    id  varchar(32) primary key,
    name varchar(32) 
) ENGINE=InnoDB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8 ;

 
create table book(
	id varchar(32) primary key ,
	name varchar(32),
	price int,
	author varchar(32),
	publishdate datetime,
	status varchar(5),
	categoryid varchar(32),
    foreign key(categoryid) references category(id)
)  ENGINE=InnoDB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8;


create table bookpic(
    id varchar(32) primary key,
    savepath varchar(200),
    showname varchar(100),
    bookid  varchar(32) ,
    fm  varchar(5),
    foreign key(bookid) references book(id)
) ENGINE=InnoDB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8;



 
```

设置url

```
ds.setUrl("jdbc:mysql:///book?characterEncoding=utf-8");
jdbc:mysql:///1803?useUnicod=true&characterEncoding=utf-8


```

设置 mydatabaseExplore

```
jdbc:mysql:///et1803?useUnicode=true&characterEncoding=utf-8
```

## Tomcat出错

```
netstat -ano|findstr  8080


taskkill  /pid  10860 /f
```

## hibernate錯誤

- HQL语句:  关键字不区分大小写，表名要与类名相同，首字母大写。

## a链接传值乱码问题

```
 <%=  new String(request.getParameter("messageTitle").getBytes("ISO-8859-1"),"UTF-8") %><br>
```

## DBUtils插入时间格式问题

```
	DateFormatUtils.format(date, "yyyy-MM-dd HH:mm"), messageID);
	book.setPublishdate(DateUtils.parseDate(pdate, new String[] { "yyyy-MM-dd" }));
	
```

