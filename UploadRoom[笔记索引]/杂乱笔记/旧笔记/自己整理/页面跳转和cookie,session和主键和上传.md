# 分散的技术

## 页面跳转

目的：在程序中从一个地方跳转到另一个地方

### 1 同步

#### [1] 从页面到另一个页面

```
1 a连接----<a href="html/jsp页面"></a>
2 在jsp中写--相当于a连接
	windows.location.href=""
	$("form表单").submit()
3 <form action="xxx">
```

#### [2] 从一个页面跳转到另一个servlet

```
1 a连接----<a href="html/jsp页面"></a>
2 在jsp中写--相当于a连接
	windows.location.href=""
	$("form表单").submit()
3 <form action="xxx">
```

### 2 异步

#### [1] 请求转发

```
1 请求转发是跳转路径
2 请求转发的转发器是：javax.servlet.RequestDispatcher
3 该对象负责接收请求和响应客户端的请求，将请求转发给其他的资源(HTML/servlet/jsp)
4 这个对象是servlet容器创建的
5 获得转发器对象的两种方式
	HttpServletRequest.getRequestDispatcher()
	ServletContext.getRequestDispatcher()
6 转发的动作是发生在服务器端，客户端不参与/客户端自始至终只发送一次请求/浏览器的地址栏只显示第一次请求的地址
```

##### [1] 相对路径(第一种转发器)

```
1 相对路径不是以"/"开头
2 要去的地址与当前路径的关系
	从servlet/login---->servlet/show
		没有从servlet中跳出，所以只需要写一个show就可以
		request.getRequestDispatcher("other").forward(request,response);
	从/servlet/login--->servlet/a/b/c/d/other
		也没有从servlet中出来
		request.getRequestDispatcher("/servlet/a/b/c/d/other").forward(request,response);
	从/servlet/login====>servlet1/a/b/c/d/other
		需要从servlet中跳出来
		request.getRequestDispatcher("../servlet1/a/b/c/d/other").forward(request,response);
```

##### [2] 绝对路径(第一种转发器)

```
1 绝对路径以“/”开头
2 绝对路径===web.xml中的url-pattern中的值一模一样
```

##### [3] 绝对路径(第二中转发器)

```
1 第二种转发器只能使用绝对路径
	this.getServletContext().getRequestDispatcher("/servlet1/a/b/c/d/other").forward(request, response);*/
```

#### [2] 重定向

```
1 客户端发送两次请求
2 客户端浏览器显示的是第二次请求的地址
```

##### [1] 相对路径

```
1 从/servlet/login====>servlet1/a/b/c/d/other
	response.sendRedirect("../servlet1/a/b/c/d/other");
```

##### [2] 绝对路径

```
1 从/servlet/login====>servlet1/a/b/c/d/other
	response.sendRedirect(request.getContextPath()+"/servlet1/a/b/c/d/other");
		request.getContextPath()--获得根目录
```

## Cookie和session

### [1] Cookie

#### [1] 案例

```
1 上次的访问时间
2 网页换肤
3 网页浏览记录
```

#### [2] 核心和含义 

```
1 核心:javax.servlet.http.Cookie
2 含义:Cookie是由是由服务器发送给浏览器的信息，保留在浏览器中的(内存和硬盘上)，稍后回送给服务器，由于cookie的值可以唯一定位一个客户端，所以Cookie通常被用来回话管理。
3 包含:Cookie包含名字和值，还有一些可选属性
4 servlet发送cookie给浏览器通过Response.addCookie();[底层是通过http响应头的方式发给浏览器的]
5 servlet通过Request.getCookie()获得请求中携带的上一次服务器给cookie的信息
6 域不同的时候，也是得不到的--cookie在servlet/abc下，携带的cookie也是在servlet/abc下
7 当域不同的时候，需要通过cookie.setPath()方法设置携带路径，才能够获得cookie信息，默认之后同域才能访问到。
	cookie.setPath("/");
```

### [2] Session

#### [1] 原理

```
1 当客户端第一次访问servlet，并调用Request.getsession方法时/或者第一次访问jsp页面的时候，服务器会创建javax.servlet.Http.httpSession,并且为每一个Session对象配每一个Sessionid
2 然后将Sessionid以cookie的形式返回到客户端浏览器，保存咋浏览器的内存中
3 下次访问服务器的时候，会将Sessionid携带到服务器端，然后根据Sessionid获取Session对象
```

#### [2] 创建,销毁和应用

```
1 第一次Request.getSession和第一次访问jsp页面的时候
2 关闭浏览器的时候 超过最大的有效时间的时候 调用invalidate()--》直接注销
3 应用：getAttribute和setAttribute
```
# 数据库表格的主键

## [1] 字符串

### [1] 手动[程序]生成

```
1 UUID 
	String pry = UUID.randomUUID().toString().replace("-","");--》里面有四个横杠，需要替换
2 根据某种固定的格式
	class IDGenerator{
      public String getKey(){
        return "ID"+getYear()+getName(); 
      }
      public String getYear(){}
      public String getName(){}
	}
3 sql语句： insert into category(id,name) value(?,?);
	pst.setId(1,可以写UUID(pry)/自己定义的方法->IDGenerator.getKey());
	pat.setName(2,Name);
```

### [2] 数据库(函数)生成

```
1 mysql: select replace(uuid(),"-","");
2 oracle: select sys_guid();
3 sql语句: insert into category(id,name) values(replace("-","")/sys_guid(),?);
```

## [2] 数字

### [1] 手动生成

通过程序实现

### [2] 通过数据库来实现

```
1 mysql:
	设置自动增长: auto_incement
	sql语句: insert into category(name) values(?);
2 oracle:
	1):通过序列生成: select etoak_seq.nextval from dual;
	sql语句: insert into category(id,name)(etoak_seq.nextval,?);
	2):通过设置最大id
		sql语句: 1:select max(id)+1 from category;---->返回一个变量-先执行
			     2:insert into category(id,name)(变量,?)--后执行
```

# 文件上传

## 1 普通表单

### [1] form表单

```
<form method="get/post" action="地址" enctype="application/x-WWW-form-urlencoded">
	input 
	select
	button/submit
</form>
enctype底层是添加了一个content-type：application/x-WWW-form-urlencoded
	其作用是将参数放在消息体中以key:value的形式传输
```

### [2] 处理

#### [1] 客户端浏览器

```
过程:浏览器客户端将表单数据封装成标准的http请求，发送到服务器,在这个过程中，参数被放在消息体中，并以key=value的格式
```

#### [2] 服务器

```
过程:服务器接受请求，构造httpServletRequest对象，setXXsetHeader setParmater setMethod setprotocol
```

#### [3] 我们/servlet

```
过程:调用Request.getMethod request.getParmater request.getHeader()
```

## 2 文件上传

### [1] form表单

```
<form method="get/post" action="地址" enctype="multipart/form-data">
	input 
	select
	button/submit
</form>
enctype底层是添加了一个content-type：multipart/form-data 后边会跟一个分隔符boundary--dshfjksdh
	其作用是将参数放在消息体中以分割符分割每一部分将文件数据以二进制的形式传送到服务器
form表单的要点:1.method必须是post 2.enctype:multipart/form-data
```

### [2] 处理

#### [1] 客户端浏览器

```
过程:浏览器客户端将表单数据封装成标准的http请求，发送到服务器,在这个过程中，参数被放在消息体中,并以分割符的形式
```

##### [1] 案例

```
请求头:
Accept:text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Accept-Encoding:gzip, deflate, br
Accept-Language:zh-CN,zh;q=0.8
Cache-Control:max-age=0
Connection:keep-alive
Content-Length:195722
Content-Type:multipart/form-data; boundary=----WebKitFormBoundaryT2Dr3r1XXoDcjF2y
Cookie:JSESSIONID=1D9FABCC8414C4B40D150C190637EE16; __guid=111872281.2446878758285992400.1504777520287.5203; monitor_count=1
Host:localhost
Origin:http://localhost
Referer:http://localhost/book/addbook.jsp
Upgrade-Insecure-Requests:1
User-Agent:Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/55.0.2883.87 Safari/537.36
//消息体
Request Payload
------WebKitFormBoundaryT2Dr3r1XXoDcjF2y
Content-Disposition: form-data; name="name"

111
------WebKitFormBoundaryT2Dr3r1XXoDcjF2y
Content-Disposition: form-data; name="author"

111
------WebKitFormBoundaryT2Dr3r1XXoDcjF2y
Content-Disposition: form-data; name="price"


------WebKitFormBoundaryT2Dr3r1XXoDcjF2y
Content-Disposition: form-data; name="publishdate"


------WebKitFormBoundaryT2Dr3r1XXoDcjF2y
Content-Disposition: form-data; name="status"

1
------WebKitFormBoundaryT2Dr3r1XXoDcjF2y
Content-Disposition: form-data; name="pic"; filename="004.jpg"
Content-Type: image/jpeg


------WebKitFormBoundaryT2Dr3r1XXoDcjF2y
Content-Disposition: form-data; name="fm"

0
------WebKitFormBoundaryT2Dr3r1XXoDcjF2y--
```

#### [2] 服务器

```
过程:接收请求构造httpServletRequest对象，setHeader setMethod() setProtocol()
		不会  setParameter()  setInputStream();
```

#### [3] 我们/Servlet

```
request.getMethod   request.getHeader() getProtocol()
		无法使用request.getPartameter()方法
		
		{通过getInputStream()获得请求的数据。根据header中的boundary，解析请求
		中的每一部分，手动获得参数和文件数据。}
		以上操作我们交给了第三方组件。如：smartupload\commons-fileupload\...
		
		我们 使用第三方组件，解析请求，从第三方组件中获得数据。{普通表单数据+文件数据}
		new.
		
		su.init..(ServletConfig,request,response);
		//upload的意思是第三方组件接管并解析请求，放到 组件内部的对象中。
		su.upload();
		
	    Request  req = su.getRequest() 
		su.getFiles();
```

##### 案例

```JAVA
//1 构造第三方组件
		SmartUpload su = new SmartUpload();
		//2 接管请求
		su.initialize(this,request,response);
		//3 解析请求
		try{
			su.upload();
			//4 获得请求
			Request req = su.getRequest();
			//接受 表单的值
			String name = req.getParameter("name");
			int price = Integer.parseInt(req.getParameter("price"));
			String author = req.getParameter("author");
			String pdata = req.getParameter("publishdate");
			String caid = req.getParameter("categoryid");
			String status = req.getParameter("status");
			Book book = new Book();
			//将的到的参数设置到实体类之中
			book.setName(name);book.setPrice(price);book.setAuthor(author);
			book.setPublishdate(DateUtils.parseDate(pdata,new String[]{"yyyy-MM-dd"}));book.setCategoryid(caid);
			book.setStatus(status);
			//调用service
			Categoryservice bs = new Categoryservice();
			//添加对象返回id
			String bookid = bs.regBook(book);
			//5 获得文件数据 遍历保存到服务器中的指定目录 将文件地址保存到数据库中
			//获取全部的上传文件，以files对象的形式返回
			Files fs = su.getFiles();
			//获取上传文件的书目
			int count = fs.getCount();
			//获取fm的value值
			int fm = Integer.parseInt(req.getParameter("fm"));
			for(int i =0;i<count;i++){
				//获取指定位移处的文件对象file 
				File f = fs.getFile(i);
				//获取HTML表单中对应上传文件的表单项的名字
				String fname = f.getFieldName();
				//获取文件扩展名后缀
				String fExt = f.getFileExt();
				String newName = UUID.randomUUID().toString().replaceAll("-","")+"."+fExt;
				//将文件换名另存
				f.saveAs("/myfiles/"+newName);
				Bookpic bp = new Bookpic();
				//将获得的id设置到Bookpic的ID中
				bp.setBookid(bookid);
				if(fm==i){
					bp.setFm("1");
				}else{
					bp.setFm("0");
				}
				bp.setSavepath("/myfiles/"+newName);
				bp.setShowname(fname);
				bs.regFic(bp);
			}
		}catch(Exception e){
			e.printStackTrace();
		}
```

### 第三方组件

#### [1] SmartUpload

##### [1] 好处

```
1 使用方便简单
2 能全成控制上传
3 能对上传的文件在大小，文件类型方面做出设置
4 下载灵活
5 能将文件上传到数据库中，也能从数据库中直接下载（只限于MySQL）
```

### [2] 相关类别

#### [1] file类

```
1 saveAs():将文件换名另存
2 isMIssing():判断用户是否选择了文件，也是用来判断表单项是否有值
3 getFiledName():获取HTML表单中对应此上传文件表单项的名字
4 getFileName():获取文件名
5 getFileExt():获取文件扩展名(后缀)
7 getSize():获取字节长度
```

#### [2] Files类

```
1 getCount()：获取上传的数目
2 getFile():获取指定位移处的对象File()
3 getSize():获取上传文件的总长度
```

#### [3] Request类

```
1 getParameter():获取指定的参数值
```