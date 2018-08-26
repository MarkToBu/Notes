## AJAX

##   1.0 介绍

~Asynchronous Javascript And Xml~
~Ajax~
~异步的js和xml~

2004年发布，用来专门处理异步请求的技术。大大提高了用户
处理页面的效率。可以说是web2.0以来最大的革命性技术

同步和异步

	同步:用户发出请求，必须等待响应的返回，如果响应迟迟没有
	返回，则用户必须一直等待在那里，当响应返回整个页面被完全
	刷新
	
	异步:用户发出请求不需要等待响应的返回，当响应返回时用户
	可能正在进行其它操作，响应返回页面部分刷新并不会导致整个
	页面的刷新
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Ajax使用到的技术

	1：JavaScript			核心语法
	2：xml					封装数据
	3：dom4j					解析数据
	4：json					2006年加入ajax技术体系
							用来取代xml
	5：dhtml					动态页面技术
							囊括的很多前端技术
							css html xhtml等等
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

##   2.0   javascript  发送异步请求

![](C:\Users\Administrator\Desktop\NOTE\source\image\ajax回调函数.jpg)

#### 前端：

```html
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
    <title>ajax1：异步查重</title>
  </head>
  <body>
	<form action="#" method="post">
		用户姓名:<input type="text" name="myname" 
		id="myname" 
		onblur="checkName(this.value)" />
		<label id="name_msg"></label>
		<br>
		<input type="submit" value="提交" 
		id="sub" disabled />
	</form>
	<script type="text/javascript">
		var request;
		//A:此函数用来创建异步请求
		function create(){
			//创建异步请求
			request = new XMLHttpRequest();
			/*
				上面的代码为level2版本
				支持大部分浏览器，但是对ie 6 7 8 及
				之前的老旧浏览器兼容性较差、
				如果出现无法创建的情况
				请使用以下level2版本
				if(window.XMLHttpRequest){
					request = new XMLHttpRequest();
				}else{
					request = 
					new ActiveXObject("Microsoft.XMLHttp");
				}
			*/		
		}
		
		//B:主函数 用来发送异步请求
		function checkName(value){
			//1:创建异步请求
			create();
			//2:发送异步请求，到指定目的地
			//request.open(method, url, async, username, password)
			/*
				method:发送请求的类型
				get或者post
				url:发送请求的目的地
				注意如果为get，则可以在此处
				通过?传值
				例如:"servlet/Check?name=***"
				async:默认为true表示使用异步请求
				更改false使用同步请求，如果响应
				未返回，则浏览器锁死等待响应的返回
				username和password:如果web容器
				开启安全策略，则必须填写用户名和密码
			*/
			request.open("post","servlet/Check",true);
			//3:post专属，get不需要书写此句
			//设置使用字符流
			request.setRequestHeader("Content-Type"
			,"application/x-www-form-urlencoded");
			//4:声明回调函数
			//注意这里表示在readyState参数发生更改时执行回调函数
			request.onreadystatechange = callback;
			//5:设置要发送的值
			//get不从此处传递值，如果不需要传递值填写null
			request.send("etoak="+value);
		
		}
			
		//C:回调函数
		function callback(){
			//首先判断返回的响应是完整的
			if(request.readyState==4){
				//判断服务器没有出现异常
				if(request.status==200){
					//接受返回的值
					var value = request.responseText;
					//拿取label
					var dom_lb = 
					document.getElementById("name_msg");
					
					//拿取提交按钮
					var dom_sub = 
					document.getElementById("sub");
					
					if(value=="bingo"){
						dom_lb.innerHTML=
						"<font color='red'>用户名已经被占用</font>";
						dom_sub.disabled = true;
						return;
					}
					dom_lb.innerHTML= 
					"<font color='green'>用户名可以使用</font>";
					dom_sub.disabled = false;	
				}
			}
		}
	</script>
  </body>
</html>
```

#### 后端

```java
package com.etoak.servlet;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class Check extends HttpServlet {

	public void doPost(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
    	/*
		 * 	ajax支持三种数据的传输
		 * 	1：String			text/plain
		 * 	2：xml				text/xml
		 *  3：json				text/plain
		 * */
		response.setContentType("text/plain;charset=utf-8");
		request.setCharacterEncoding("utf-8");
		PrintWriter out = response.getWriter();
		//接受主函数提交过来的值
		String name = request.getParameter("etoak");
		//System.out.println("接受的名字是~~~》"+name);
		if(name.trim().equals("penny")){
			//发送回回调函数
			//不要使用println
			//否则字符串会添加\n
			out.print("bingo");
			out.flush();
			out.close();
			return;
		}
		out.print("suc");
		out.flush();
		out.close();
	}
}
```

###  示例一：  实用xml处理批量数据

#### 前端：

```html
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
    <title>ajax2:使用xml处理批量数据</title>
  </head>
  <body onload="show()">
	<div id="show"></div>
	<script type="text/javascript">
		var request;
		function create(){
			request = new XMLHttpRequest();
		}
		
		function show(){
			create();
			request.open("post","servlet/ShowAll",true);
			request.setRequestHeader("Content-Type"
			,"application/x-www-form-urlencoded");
			request.onreadystatechange = callback;
			request.send(null);
		}
		
		function callback(){
			if(request.readyState==4){
				if(request.status==200){
					/*
					 * <root>
					 *  	<student id="" name="">
					 *  		<email>**</email>
					 *  		<phone>**</phone>
					 *  	</student>
					 *  	<student id="" name="">
					 *  		<email>**</email>
					 *  		<phone>**</phone>
					 *  	</student>
					 *  	***
					 * </root>
					 * */
					var doc = request.responseXML;
				
					/*
							<student id="" name="">
					 *  		<email>**</email>
					 *  		<phone>**</phone>
					 *  	</student>
					 *  	<student id="" name="">
					 *  		<email>**</email>
					 *  		<phone>**</phone>
					 *  	</student>
					 *  	***
					*/
					 var array = doc
					.getElementsByTagName("student");
					
					 var table = "<table style='width:500px' border='1px'><tr><td>ID</td><td>姓名</td><td>邮箱</td><td>电话</td><td>操作</td></tr>";
					
					 for(var i = 0;i<array.length;i++){
						 var stu = array[i];
						 var id = stu.getAttribute("id");
						 var name = stu.getAttribute("name");
						 
						 var email = 
						 stu.getElementsByTagName("email")[0]
						 .firstChild.nodeValue;
						 
						 var phone = 
						 stu.getElementsByTagName("phone")[0]
						 .firstChild.nodeValue;
					 
					 	table += 
					 	"<tr><td>"+id+"</td><td>"+name+"</td><td>"
					 	+email+"</td><td>"+phone+"</td><td><label onclick='delStu("+id+")' style='cursor:hand'>删除</label></td></tr>";
					 }
					 table += "</table>";
					 document.getElementById("show").innerHTML 
					 = table;
					
				}
			}	
		}
		
		function delStu(id){
			if(confirm("您确定要删除吗")){
				create();
				request.open("get","servlet/Del?id="+id
				+"&time="+new Date(),true);
				request.onreadystatechange = function(){
					if(request.readyState==4){
						if(request.status==200){
							var value = request.responseText;
							if(value=="true"){
								show();
								return;
							}
							alert("删除失败！");
						}
					}
				};
				request.send(null);
				return;
			}
		}
	</script>
  </body>
</html> 
```

#### 展示所有数据后台

```java
package com.etoak.servlet;

import java.io.IOException;
import java.io.PrintWriter;
import java.util.List;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.dom4j.Document;
import org.dom4j.DocumentHelper;
import org.dom4j.Element;

import com.etoak.dao.StudentDaoIf;
import com.etoak.dao.StudentDaoImpl;
import com.etoak.po.Student;

public class ShowAll extends HttpServlet {

	public void doPost(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {

		response.setContentType("text/xml;charset=utf-8");
		request.setCharacterEncoding("utf-8");
		PrintWriter out = response.getWriter();
		StudentDaoIf dao = new StudentDaoImpl();
		List<Student> list = dao.getAll();
		/*
		 * 当需要封装大量数据时，我们使用xml进行封装
		 * 使用dom4j技术可以将大量的数据封装到xml文件中
		 * */
		//创建文档对象模型
		Document doc = DocumentHelper.createDocument();
		
		/*
		 * <root>
		 * 		
		 * </root>
		 * */
		Element root = doc.addElement("root");
		
		for(Student stu:list){
			/*
			 * <root>
			 *  	<student></student>
			 *  	<student></student>
			 *  	<student></student>
			 *  	<student></student>
			 *  	***
			 * </root>
			 * */
			Element child = root.addElement("student");
			/*
			 * <root>
			 *  	<student id="" name=""></student>
			 *  	<student id="" name=""></student>
			 *  	<student id="" name=""></student>
			 *  	<student id="" name=""></student>
			 *  	***
			 * </root>
			 * */
			child.addAttribute("id",stu.getId()+"");
			child.addAttribute("name",stu.getName());
			/*
			 * <root>
			 *  	<student id="" name="">
			 *  		<email>**</email>
			 *  		<phone>**</phone>
			 *  	</student>
			 *  	<student id="" name="">
			 *  		<email>**</email>
			 *  		<phone>**</phone>
			 *  	</student>
			 *  	***
			 * </root>
			 * */
			Element email = child.addElement("email");
			email.setText(stu.getEmail());
			Element phone = child.addElement("phone");
			phone.setText(stu.getPhone());
		}
		
		//将创建好的文档对象模型转换为xml
		//发送回回调函数
		out.print(doc.asXML());
		System.out.println(doc.asXML());
		out.flush();
		out.close();
	}
}
```

Dao层

```java
ublic class StudentDaoImpl implements StudentDaoIf {
	
	Connection con;
	PreparedStatement pst;
	ResultSet rs;
	
	@Override
	public List<Student> getAll() {
		try{
			String sql = "select * from student";
			con = Factory.getCon();
			pst = con.prepareStatement(sql);
			rs = pst.executeQuery();
			List<Student> list = new ArrayList<Student>();
			while(rs.next()){
				Student stu = new Student();
				stu.setId(rs.getInt(1));
				stu.setName(rs.getString(2));
				stu.setEmail(rs.getString(3));
				stu.setPhone(rs.getString(4));
				list.add(stu);
			}
			return list;
		}catch(Exception ex){
			ex.printStackTrace();
			return null;
		}finally{
			Factory.close(con, pst, rs);
		}
	}

	@Override
	public boolean delStuById(Integer id) {
		try{
			String sql = "delete from student where id = ?";
			con = Factory.getCon();
			pst = con.prepareStatement(sql);
			pst.setInt(1,id);
			return pst.executeUpdate()==1;
		}catch(Exception ex){
			ex.printStackTrace();
			return false;
		}finally{
			Factory.close(con, pst,null);
		}
	}

}
```

###  Factory

```java
package com.etoak.factory;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;

public class Factory {
	
	static{
		try{
			Class.forName("com.mysql.jdbc.Driver");
		}catch(Exception ex){
			ex.printStackTrace();
		}
	}
	
	public static Connection getCon(){
		try{
			return DriverManager
		.getConnection("jdbc:mysql://localhost:3306/et1803"
		,"root","joshuae");
		}catch(Exception ex){
			ex.printStackTrace();
			return null;
		}
	}
	
	public static void close(Connection con,PreparedStatement pst,ResultSet rs){
		try{
			if(rs!=null)
				rs.close();
		}catch(Exception ex){
			ex.printStackTrace();
		}finally{
			try{
				if(pst!=null)
					pst.close();
			}catch(Exception ex){
				ex.printStackTrace();
			}finally{
				try{
					if(con!=null)
						con.close();
				}catch(Exception ex){
					ex.printStackTrace();
				}
			}
		}
	}
}
```

  删除: Servlet层

```java
package com.etoak.servlet;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.etoak.dao.StudentDaoIf;
import com.etoak.dao.StudentDaoImpl;

public class Del extends HttpServlet {

	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {

		response.setContentType("text/plain;charset=utf-8");
		request.setCharacterEncoding("utf-8");
		PrintWriter out = response.getWriter();
		Integer id = Integer.parseInt(request.getParameter("id"));
		StudentDaoIf dao = new StudentDaoImpl();
		boolean flag = dao.delStuById(id);
		if(flag){
			out.print("true");
			return;
		}
		out.print("false");
		out.flush();
		out.close();
	}
}
```

#### 示例二 ：级联  Cascade

```html
<%@ page language="java" import="java.util.*" 
pageEncoding="UTF-8"
contentType="text/html; charset=utf-8"%>
<% request.setCharacterEncoding("utf-8"); %>
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
    <title>ajax3:使用json创建级联菜单</title>
  </head>
  <body onload="showProvince()                                    ">
	选择省份:
	<select name="province" id="province"
	onchange="showCity(this.value)">
		<option value="0">===请选择省份===</option>
	</select>
	选择城市:
	<select name="city" id="city">
		<option value="0">===请选择城市===</option>
	</select>
	<script type="text/javascript"
	src="script/json2.js"></script>
	<script type="text/javascript">
		var request;
		function create(){
			request = new XMLHttpRequest();
		}
		
		function showProvince(){
			create();
			request.open("post","servlet/ShowProvince",true);
			request.setRequestHeader("Content-Type"
			,"application/x-www-form-urlencoded");
			request.onreadystatechange = function(){
				if(request.readyState==4){
					//alert(request.status);
					if(request.status==200){
						var text = request.responseText;
						//alert(text);
						/*
							使用转换器将合法格式的json字符串
							转换为js对象（dom对象）
							转换规则如下
							1:json中的key全部转换为js对象的
							属性名
							2:json中的value全部转换为js对象的
							属性值
							对象.属性名  = 属性值
						*/
						var obj = JSON.parse(text);
						
						//拿取下拉列表控件
						var dom_select = 
						document.getElementById("province");
						//alert(obj);
						//[{"id":1,"name":"山东"},{"id":2,"name":"广东"},{"id":3,"name":"上海"},{"id":4,"name":"香港"}]}{"etoak":[{"id":1,"name":"山东"},{"id":2,"name":"广东"},{"id":3,"name":"上海"},{"id":4,"name":"香港"}]
						var array = obj.etoak;
						for(var i = 0;i<array.length;i++){
							//{"id":1,"name":"山东"}
							var province = array[i];
							var id = province.id;
							var name = province.name;
							/*
								将拿取的数据封装进下拉列表控件
								等号左边:小写o 带s [表示索引值]
								等号右边:大写O 不带s (页面显示的文本,value值)
							*/
							dom_select.options[i+1] = new Option(name,id);
						}
					}
				}
			};
			request.send(null);
		}
		
		function showCity(pid){
			create();
			request.open("post","servlet/ShowCity",true);
			request.setRequestHeader("Content-Type"
					,"application/x-www-form-urlencoded");
			request.onreadystatechange = function(){
				if(request.readyState==4){
					if(request.status==200){
						var text = request.responseText;
						//alert(text);
						//[{"id":14,"name":"黄浦区","pid":3},{"id":15,"name":"长宁区","pid":3},{"id":16,"name":"青浦区","pid":3},{"id":17,"name":"卢湾区","pid":3},{"id":18,"name":"普陀区","pid":3}]
						var array = JSON.parse(text);
						//首先清空城市下拉列表
						//拿取城市下拉控件
						var dom_select = 
						document.getElementById("city");
						while(dom_select.length>1){
							//删除索引值是1的option，0的保留
							dom_select.removeChild(dom_select.item(1));
						}
						
						for(var i = 0;i<array.length;i++){
							var city = array[i];
							var id = city.id;
							var name = city.name;
							dom_select.options[i+1] = new Option(name,id);
						}
	 		
					}
				}
			};
			request.send("pid="+pid);
		}
		
	</script>
  </body>
</html>
```

- sql

  ```sql
  drop table if exists province;
  drop table if exists city;
  drop table if exists areas;
  
  create table province(
  	id int primary key auto_increment,
  	name varchar(10)
  );
  
  create table city(
  	id int primary key auto_increment,
  	pid int,
  	name varchar(10)
  );
  
  insert into province values (null,'山东');
  insert into province values (null,'广东');
  insert into province values (null,'上海');
  insert into province values (null,'香港');
  
  insert into city values (null,1,'济南');
  insert into city values (null,1,'青岛');
  insert into city values (null,1,'淄博');
  insert into city values (null,1,'德州');
  insert into city values (null,1,'济宁');
  insert into city values (null,1,'烟台');
  insert into city values (null,1,'威海');
  
  insert into city values (null,2,'广州');
  insert into city values (null,2,'深圳');
  insert into city values (null,2,'佛山');
  insert into city values (null,2,'珠海');
  insert into city values (null,2,'东莞');
  insert into city values (null,2,'揭阳');
  
  insert into city values (null,3,'黄浦区');
  insert into city values (null,3,'长宁区');
  insert into city values (null,3,'青浦区');
  insert into city values (null,3,'卢湾区');
  insert into city values (null,3,'普陀区');
  
  insert into city values (null,4,'九龙');
  insert into city values (null,4,'新界');
  insert into city values (null,4,'大屿山');
  insert into city values (null,4,'港岛');
  ```

  - Entity 实体类

    ```java
    package com.etoak.po;
    
    public class Province {
    	private Integer id;
    	private String name;
    	public Province(){}
    	public Province(Integer id,String name){
    		this.id = id;
    		this.name = name;
    	}
    	public Integer getId() {
    		return id;
    	}
    	public void setId(Integer id) {
    		this.id = id;
    	}
    	public String getName() {
    		return name;
    	}
    	public void setName(String name) {
    		this.name = name;
    	}
    	@Override
    	public String toString() {
    		return "Province [id=" + id + ", name=" + name + "]";
    	}
    }
    //实体类2
    package com.etoak.po;
    
    public class City {
    	private Integer id;
    	private Integer pid;
    	private String name;
    	
    	public City(){}
    	
    	public City(Integer id,Integer pid,String name){
    		this.id = id;
    		this.pid = pid;
    		this.name = name;
    	}
    
    	public Integer getId() {
    		return id;
    	}
    
    	public void setId(Integer id) {
    		this.id = id;
    	}
    
    	public Integer getPid() {
    		return pid;
    	}
    
    	public void setPid(Integer pid) {
    		this.pid = pid;
    	}
    
    	public String getName() {
    		return name;
    	}
    
    	public void setName(String name) {
    		this.name = name;
    	}
    
    	@Override
    	public String toString() {
    		return "City [id=" + id + ", pid=" + pid + ", name=" + name + "]";
    	}	
    }
    
    
    ```

- jdbc原始连接

  ```java
  package com.etoak.factory;
  
  import java.sql.Connection;
  import java.sql.DriverManager;
  import java.sql.PreparedStatement;
  import java.sql.ResultSet;
  
  public class Factory {
  	
  	static{
  		try{
  			Class.forName("com.mysql.jdbc.Driver");
  		}catch(Exception ex){
  			ex.printStackTrace();
  		}
  	}
  	
  	public static Connection getCon(){
  		try{
  			return DriverManager
  		.getConnection("jdbc:mysql://localhost:3306/et1803"
  		,"root","joshuae");
  		}catch(Exception ex){
  			ex.printStackTrace();
  			return null;
  		}
  	}
  	
  	public static void close(Connection con,PreparedStatement pst,ResultSet rs){
  		try{
  			if(rs!=null)
  				rs.close();
  		}catch(Exception ex){
  			ex.printStackTrace();
  		}finally{
  			try{
  				if(pst!=null)
  					pst.close();
  			}catch(Exception ex){
  				ex.printStackTrace();
  			}finally{
  				try{
  					if(con!=null)
  						con.close();
  				}catch(Exception ex){
  					ex.printStackTrace();
  				}
  			}
  		}
  	}
  }
  ```

  - Dao层

  ```java
  package com.etoak.dao;
  
  import java.sql.Connection;
  import java.sql.PreparedStatement;
  import java.sql.ResultSet;
  import java.util.ArrayList;
  import java.util.List;
  
  import com.etoak.factory.Factory;
  import com.etoak.po.City;
  import com.etoak.po.Province;
  
  public class DaoImpl implements DaoIf {
  
  	Connection con;
  	PreparedStatement pst;
  	ResultSet rs;
  	
  	@Override
  	public List<Province> getAllProvince() {
  		try{
  			String sql = 
  			"select * from province";
  			con = Factory.getCon();
  			pst = con.prepareStatement(sql);
  			rs = pst.executeQuery();
  			List<Province> list = 
  			new ArrayList<Province>();
  			while(rs.next()){
  				Province pro = new Province(rs.getInt(1),
  						rs.getString(2));
  				list.add(pro);
  			}
  			return list;
  		}catch(Exception ex){
  			ex.printStackTrace();
  			return null;
  		}finally{
  			Factory.close(con, pst, rs);
  		}
  	}
  
  	@Override
  	public List<City> getCityByPid(Integer pid) {
  		try{
  			String sql = 
  			"select * from city where pid = ?";
  			con = Factory.getCon();
  			pst = con.prepareStatement(sql);
  			pst.setInt(1, pid);
  			rs = pst.executeQuery();
  			List<City> list = new ArrayList<City>();
  			while(rs.next()){
  				City ct = new City();
  				ct.setId(rs.getInt(1));
  				ct.setPid(rs.getInt(2));
  				ct.setName(rs.getString(3));
  				list.add(ct);
  			}
  			return list;
  		}catch(Exception ex){
  			ex.printStackTrace();
  			return null;
  		}finally{
  			Factory.close(con, pst, rs);
  		}
  	}
  }
  ```

  - web层  Servlet

    showCity.java

  ```java
  package com.etoak.servlet;
  
  import java.io.IOException;
  import java.io.PrintWriter;
  import java.util.List;
  
  import javax.servlet.ServletException;
  import javax.servlet.http.HttpServlet;
  import javax.servlet.http.HttpServletRequest;
  import javax.servlet.http.HttpServletResponse;
  
  import net.sf.json.JSONArray;
  
  import com.etoak.dao.DaoIf;
  import com.etoak.dao.DaoImpl;
  import com.etoak.po.City;
  
  public class ShowCity extends HttpServlet {
     public void doPost(HttpServletRequest request, HttpServletResponse response)
  			throws ServletException, IOException {
  
  		response.setContentType("text/plain;charset=utf-8");
  		request.setCharacterEncoding("utf-8");
  		PrintWriter out = response.getWriter();
  		DaoIf dao = new DaoImpl();
  		List<City> list = dao.getCityByPid(
  		Integer.parseInt(request.getParameter("pid")));
  		JSONArray ja = JSONArray.fromObject(list);
  		System.out.println(ja);
  		out.print(ja);
  		out.flush();
  		out.close();
  	}
  }
  ```

  showProvice.java

  ```java
  package com.etoak.servlet;
  
  import java.io.IOException;
  import java.io.PrintWriter;
  import java.util.List;
  
  import javax.servlet.ServletException;
  import javax.servlet.http.HttpServlet;
  import javax.servlet.http.HttpServletRequest;
  import javax.servlet.http.HttpServletResponse;
  
  import net.sf.json.JSONObject;
  
  import com.etoak.dao.DaoIf;
  import com.etoak.dao.DaoImpl;
  import com.etoak.po.Province;
  
  public class ShowProvince extends HttpServlet {
  
  	public void doPost(HttpServletRequest request, HttpServletResponse response)
  			throws ServletException, IOException {
  
  		response.setContentType("text/plain;charset=utf-8");
  		request.setCharacterEncoding("utf-8");
  		PrintWriter out = response.getWriter();
  		DaoIf dao = new DaoImpl();
  		List<Province> list = dao.getAllProvince();
  		JSONObject jo = new JSONObject();
  		jo.put("etoak",list);
  		System.out.print(jo);
  		out.print(jo);
  		out.flush();
  		out.close();
  	}
  
  }
  
  ```


##  3.0  Jquery ===》 Ajax 查重复用户名：

```javascript
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
    <title>jQuery6:ajax查重</title>
    <link href="css/style.css" type="text/css"
    rel="stylesheet" />
  </head>
  <body>
	<form action="#" method="post">
		用户姓名:<input type="text" name="name" />
		<label id="name_msg"></label>
		<br>
		<input type="submit" value="提交"
		disabled >
	</form>
	<script type="text/javascript"
	src="script/jquery-1.10.2.js"></script>
	<script type="text/javascript">
		$(function(){
			var $jq_input = $(":text");
			var $jq_sub = $(":submit");
			var $jq_lb = $("label#name_msg");
			
			$jq_input.blur(function(){
				/*
					$.trim():去掉字符串两边的空格
				*/
				if($.trim($jq_input.val()).length<4
						||$.trim($jq_input.val()).length>8){
					//信息提示
					$jq_lb.html(
				"<font color='red'>用户名不能小于4位或者大于8位</font>");
					//禁用按钮
					$jq_sub.attr("disabled",true);
					//联动样式
					$jq_input.addClass("myinput");
					return;
				}
				
				//向服务器发送异步请求
				/*
					$.post(url,data,callback);
				
					url:异步请求的目的地
					注意如果是get可以通过?传递值
					data:要发送的值，get 和 post
					都可以在此处传递值
					格式：{key:value,key2:value2}
					注意如果不传值此参数可以不写，也可以
					填写null
					callback:回调函数，注意仅仅可以处理
					服务器无误的情况,支持五种数据类型的传输
					text script xml json html
				*/
				$.post("servlet/Check",{etoak:$jq_input.val()},
						function(data){
					if(data=="bingo"){
						$jq_lb.html(
						"<font color='red'>用户名已经被占用</font>");
						//禁用按钮
						$jq_sub.attr("disabled",true);
						//联动样式
						$jq_input.addClass("myinput");
						return;
					}
					$jq_lb.html(
					"<font color='green'>用户名可以使用 </font>");
					//禁用按钮
					$jq_sub.attr("disabled",false);
					//联动样式
					$jq_input.removeClass("myinput");
				});			
			});
		});
	</script>
  </body>
</html>
```

######  后端

```java
package com.etoak.servlet;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class Check extends HttpServlet {

	public void doPost(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {

		//注意这里不需要进行更改了 text/html是合法的MIME类型
		response.setContentType("text/html;charset=utf-8");
		request.setCharacterEncoding("utf-8");
		PrintWriter out = response.getWriter();
		
		//接受发送过来的值
		String value = request.getParameter("etoak");
		if(value.equals("elena")){
			out.print("bingo");
			out.flush();
			out.close();
			return;
		}
		out.print("suc");
		out.flush();
		out.close();
	}
}
```

## 4.0 异步分页示例

####  前端详情：

```html
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
    <title>jQuery7:异步分页</title>
  </head>
  <body>
  	<div id="show"></div>
  	<label id="msg"></label>
  	<br>
  	<button id="first">首页</button>
  	<button id="pre">上一页</button>
  	<button id="next">下一页</button>
  	<button id="last">末页</button>
  
	<script type="text/javascript"
	src="script/jquery-1.10.2.js"></script>
	<script type="text/javascript">
		//1:总记录数
		var allRecord;
	
		//2:每页记录数
		var pageRecord = 5;
	
		//3:总页数
		var allPage;
	
		//4:当前页
		var currentPage = 1;
	
		$(function(){
			check();
		});
		
		//A:此函数用来查询数据
		function check(){
			$.ajax({
				//1:发送到的目的地
				url:"servlet/GetPage",
				//2:发送异步请求的类型
				type:"post",
				//3:传输的数据
				//格式:key=value&key2=value2&keyN=valueN
				data:"currentPage="+currentPage
				+"&pageRecord="+pageRecord,
				//4:是否使用异步
				async:true,
				//5:返回的数据类型
				//支持五种数据类型 text script json html xml
				dataType:"json",
				//6:服务器无误时回调函数
				success:function(data){
					//alert(data);
					/*
						当dataType设置为json时
						jQuery类库自动将返回的标注
						json字符串转换为js对象
						原先的key值变为属性名
						原先的value值变为属性值
						不需要我们再次导入转换器进行转换
					*/
					//拿取总记录数
					allRecord = data.allRecord;
					//拿取总页数
					allPage = data.allPage;
					//拿取封装的集合
					//[{"id":1,"author":"新华社","title":"汶川地震10周年"},{"id":2,"author":"新浪","title":"河南犯罪嫌疑人仍然未被抓获"},{"id":3,"author":"网易","title":"5月15锤子发布R1旗舰"},{"id":4,"author":"新华社","title":"美国退出伊朗核协议"},{"id":5,"author":"网易","title":"优衣库联名暴雪T恤即将上市"}]
					var array = data.list;
					
					var table = 
					"<table style='width:500px' border='1px'><tr><td>ID</td><td>标题</td><td>发布人</td></tr>";
					/*
						$.each(循环体,function(索引值,对象){
							
						});
					
					*/
					$.each(array,function(index,news){
						table += "<tr><td>"+news.id+"</td><td>"
						+news.title+"</td><td>"
						+news.author+"</td></tr>";
					});
					table += "</table>";
					$("div#show").html(table);
					$("label#msg").html("当前是第"+currentPage
					+"页，共有"+allPage
					+"页，"+allRecord+"条记录");
					if(allPage>1){
						//给所有按钮解除绑定
						$("button").unbind();
						//给按钮绑定激发事件
						if(currentPage==1){
							$("#next,#last").click(function(){
								changeCurrentPage($(this).attr("id"));
							});
						}else if(currentPage==allPage){
							$("#first,#pre").click(function(){
								changeCurrentPage($(this).attr("id"));
							});
						}else{
							$("button").click(function(){
								changeCurrentPage($(this).attr("id"));
							});
						}		
					}
					
				},
				//7:服务器出现异常时
				error:function(err){
					alert(err.status);
				}
			});
		}
		
		//B:此函数根据传入的id不同修改响应的当前页数值
		function changeCurrentPage(id){
			if(id=="first"){
				currentPage=1;
			}else if(id=="pre"){
				currentPage--;
			}else if(id=="next"){
				currentPage++;
			}else if(id=="last"){
				currentPage=allPage;
			}
			check();
		}
	</script>
  </body>
</html>
```

####  后端分页

```java
package com.etoak.servlet;

import java.io.IOException;
import java.io.PrintWriter;
import java.util.List;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import net.sf.json.JSONObject;

import com.etoak.dao.NewsDaoIf;
import com.etoak.dao.NewsDaoImpl;
import com.etoak.po.News;

public class GetPage extends HttpServlet {

	public void doPost(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		
		/*
		 * 注意这里MIME数据类型如果与dataType设置的
		 * 不统一，则此处设置的无效，以dataType为准
		 * */
		response.setContentType("text/html;charset=utf-8");
		request.setCharacterEncoding("utf-8");
		PrintWriter out = response.getWriter();
		//接受当前页
		int currentPage = Integer.parseInt(
		request.getParameter("currentPage"));
		
		//接受每页记录数
		int pageRecord = Integer.parseInt(
	    request.getParameter("pageRecord"));
		
		//拿取总记录数
		NewsDaoIf dao = new NewsDaoImpl();
		int allRecord = dao.getCount();
		
		//拿取总页数
		int allPage = 
		(allRecord+pageRecord-1)/pageRecord;
		
		//拿取当前页是1时的分页数据
		List<News> list = 
		dao.getPage((currentPage-1)*pageRecord,pageRecord);
		
		JSONObject json = new JSONObject();
		json.put("allRecord",allRecord);
		json.put("allPage",allPage);
		json.put("list", list);
		out.print(json);
		System.out.println(json);
		out.flush();
		out.close();
	}
}
```

####  老Factory

```
package com.etoak.factory;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;

public class Factory {
	
	static{
		try{
			Class.forName("com.mysql.jdbc.Driver");
		}catch(Exception ex){
			ex.printStackTrace();
		}
	}
	
	public static Connection getCon(){
		try{
			return DriverManager
		.getConnection("jdbc:mysql://localhost:3306/et1803"
		,"root","joshuae");
		}catch(Exception ex){
			ex.printStackTrace();
			return null;
		}
	}
	
	public static void close(Connection con,PreparedStatement pst,ResultSet rs){
		try{
			if(rs!=null)
				rs.close();
		}catch(Exception ex){
			ex.printStackTrace();
		}finally{
			try{
				if(pst!=null)
					pst.close();
			}catch(Exception ex){
				ex.printStackTrace();
			}finally{
				try{
					if(con!=null)
						con.close();
				}catch(Exception ex){
					ex.printStackTrace();
				}
			}
		}
	}	
}
```

####   老Dao

```java
package com.etoak.dao;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.util.ArrayList;
import java.util.List;

import com.etoak.factory.Factory;
import com.etoak.po.News;

public class NewsDaoImpl implements NewsDaoIf {

	Connection con;
	PreparedStatement pst;
	ResultSet rs;
	
	@Override
	public Integer getCount() {
		Connection con = null;
		PreparedStatement pst = null;
		String sql = "select count(*) from news";
		ResultSet rs = null;
		try {
			con = Factory.getCon();
			pst = con.prepareStatement(sql);
			rs = pst.executeQuery();
			rs.next();
			return rs.getInt(1);
		} catch (Exception ex) {
			ex.printStackTrace();
			return 0;
		} finally {
			Factory.close(con, pst, rs);
		}
	}

	@Override
	public List<News> getPage(Integer index, Integer max) {
		Connection con = null;
		PreparedStatement pst = null;
		String sql = "select * from news limit ?,?";
		ResultSet rs = null;
		List<News> list = new ArrayList<News>();
		try{
			con = Factory.getCon();
			pst = con.prepareStatement(sql);
			pst.setInt(1, index);
			pst.setInt(2, max);
			rs = pst.executeQuery();
			while(rs.next()){
				News com = new News();
				com.setId(rs.getInt(1));
				com.setTitle(rs.getString(2));;
				com.setAuthor(rs.getString(3));
				list.add(com);
			}
			return list;
		}catch(Exception ex){
			ex.printStackTrace();
			return null;
		}finally{
			Factory.close(con, pst, rs);
		}
	}
}
```

####  News实体类

```java
package com.etoak.po;

public class News {
	private Integer id;
	private String title;
	private String author;
	
	public News(){
		
	}
	public News(Integer id,String title,String author){
		this.id = id;
		this.title = title;
		this.author = author;
	}
	public Integer getId() {
		return id;
	}
	public void setId(Integer id) {
		this.id = id;
	}
	public String getTitle() {
		return title;
	}
	public void setTitle(String title) {
		this.title = title;
	}
	public String getAuthor() {
		return author;
	}
	public void setAuthor(String author) {
		this.author = author;
	}
	@Override
	public String toString() {
		return "News [id=" + id + ", title=" + title + ", author=" + author
				+ "]";
	}	
}
```

##### ## sql

```sql
drop table if exists news;

create table news(
	id int primary key auto_increment,
	title varchar(90),
	author varchar(30)	
);

insert into news values (null,'汶川地震10周年','新华社');
insert into news values (null,'河南犯罪嫌疑人仍然未被抓获','新浪');
insert into news values (null,'5月15锤子发布R1旗舰','网易');
insert into news values (null,'美国退出伊朗核协议','新华社');
insert into news values (null,'优衣库联名暴雪T恤即将上市','网易');
insert into news values (null,'中国队新队服发布','懂球帝');
insert into news values (null,'阿森纳传奇教练温格离任','懂球帝');
insert into news values (null,'2018全国剑道大赛将在两周后在上海召开','腾讯');
insert into news values (null,'汶川地震10周年','新华社');
insert into news values (null,'河南犯罪嫌疑人仍然未被抓获','新浪');
insert into news values (null,'5月15锤子发布R1旗舰','网易');
insert into news values (null,'美国退出伊朗核协议','新华社');
insert into news values (null,'优衣库联名暴雪T恤即将上市','网易');
insert into news values (null,'中国队新队服发布','懂球帝');
insert into news values (null,'阿森纳传奇教练温格离任','懂球帝');
insert into news values (null,'2018全国剑道大赛将在两周后在上海召开','腾讯');
insert into news values (null,'汶川地震10周年','新华社');
insert into news values (null,'河南犯罪嫌疑人仍然未被抓获','新浪');
insert into news values (null,'5月15锤子发布R1旗舰','网易');
insert into news values (null,'美国退出伊朗核协议','新华社');
insert into news values (null,'优衣库联名暴雪T恤即将上市','网易');
insert into news values (null,'中国队新队服发布','懂球帝');
insert into news values (null,'阿森纳传奇教练温格离任','懂球帝');
insert into news values (null,'2018全国剑道大赛将在两周后在上海召开','腾讯');
insert into news values (null,'汶川地震10周年','新华社');
insert into news values (null,'河南犯罪嫌疑人仍然未被抓获','新浪');
insert into news values (null,'5月15锤子发布R1旗舰','网易');
insert into news values (null,'美国退出伊朗核协议','新华社');
insert into news values (null,'优衣库联名暴雪T恤即将上市','网易');
insert into news values (null,'中国队新队服发布','懂球帝');
insert into news values (null,'阿森纳传奇教练温格离任','懂球帝');
insert into news values (null,'2018全国剑道大赛将在两周后在上海召开','腾讯');
insert into news values (null,'汶川地震10周年','新华社');
insert into news values (null,'河南犯罪嫌疑人仍然未被抓获','新浪');
insert into news values (null,'5月15锤子发布R1旗舰','网易');
insert into news values (null,'美国退出伊朗核协议','新华社');
insert into news values (null,'优衣库联名暴雪T恤即将上市','网易');
insert into news values (null,'中国队新队服发布','懂球帝');
insert into news values (null,'阿森纳传奇教练温格离任','懂球帝');
insert into news values (null,'2018全国剑道大赛将在两周后在上海召开','腾讯');
insert into news values (null,'汶川地震10周年','新华社');
insert into news values (null,'河南犯罪嫌疑人仍然未被抓获','新浪');
insert into news values (null,'5月15锤子发布R1旗舰','网易');
insert into news values (null,'美国退出伊朗核协议','新华社');
insert into news values (null,'优衣库联名暴雪T恤即将上市','网易');
insert into news values (null,'中国队新队服发布','懂球帝');
insert into news values (null,'阿森纳传奇教练温格离任','懂球帝');
insert into news values (null,'2018全国剑道大赛将在两周后在上海召开','腾讯');
insert into news values (null,'汶川地震10周年','新华社');
insert into news values (null,'河南犯罪嫌疑人仍然未被抓获','新浪');
insert into news values (null,'5月15锤子发布R1旗舰','网易');
insert into news values (null,'美国退出伊朗核协议','新华社');
insert into news values (null,'优衣库联名暴雪T恤即将上市','网易');
insert into news values (null,'中国队新队服发布','懂球帝');
insert into news values (null,'阿森纳传奇教练温格离任','懂球帝');
insert into news values (null,'2018全国剑道大赛将在两周后在上海召开','腾讯');
```
