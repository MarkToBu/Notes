# JDBC

## 1.0 简介

```
~Java DataBase Connection~
~JDBC~

SUN     ORACLE
J2SE~~~>JavaSE
J2EE~~~>JavaEE	   囊括了十三种技术
J2ME~~~>JavaME~~~>Andriod
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
JDBC:
	可是使用相同的Java代码，操纵不同品牌的数据库，
	对数据库进行增删查改等操作，jdbc是最早的
	ORM工具（Object Mapping Relation对象映射关系）
	著名的ORM工具有
		全手动:JDBC
		半自动:Mybatis
		全自动:Hibernate
	JDBC是JavaEE的十三种标准技术之一，所有的工具类都放置
	在java.sql包中
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
使用JDBC来操纵数据库的流程

	1):建立关系表
		一般在com.etoak.sql包中创建
		***.sql文件，搭建好链接之后执行sql文件
		在数据库中创建表

	2):建立对象
		一般在com.etoak.po包中创建
		使用包装类,必须创建空参构造方法，酌情创建
		带参数的构造方法，覆盖toString()方法
		此对象一般正好对应关系表
	3):创建工厂
		A)通过静态初始化块利用反射加载驱动
		B)获取链接与数据库链接成功
	4):创建测试类
		A)通过工厂获取链接
		B)通过链接拿取执行器Statement
		C)通过执行器调用
			boolean execute()
			int executeUpdate()
			Result executeQuery()
			
			Result
					.next()判断是否存在有效数据
					拿取对应的数据
					get数据类型(列数)
					get数据类型(列名)
					拿取数据
```

##  1.1 使用步骤

1> 先导包

2> 书写factory包下的  factory 工厂类

```java
package com.etoak.factory;

import java.sql.Connection;
import java.sql.DriverManager;

public class Factory {
	/*
	 *  此工厂模式用来创建Connection链接
	 *  此链接用来链接数据库
	 *  创建链接首先要加载某一种品牌数据库的
	 *  驱动
	 * 	因为某种品牌的数据库驱动仅仅需要加载一次
	 * 所以我们可以将其放置在静态初始化块中	
	 * */
	static{
		try{
			//使用反射进行类加载的方式加载驱动
			//com.mysql.jdbc.Driver:为mysql的驱动
			Class.forName("com.mysql.jdbc.Driver");
		}catch(Exception ex){
			ex.printStackTrace();
		}
	}
	
	//此静态方法用来创建链接
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
}
```

3> 书写实体类

```java
package com.etoak.po;

import java.util.Date;

/*
 * POJO类，又称之为实体类
 * 一般用来映射关系表，
 * 或者作为对象存在，是java中最为重要的
 * 类，也是JavaBean最关键的核心类
 * 通过private封装属性
 * 
 * */
public class Person {
	/*
	 * 	这里使用包装类
	 * */
	private Integer id;
	private String name;
	private String pass;
	private Integer age;
	private Double salary;
	private Date birth;
	
	public Person(){}
	
	public Person(Integer id,String name
			,String pass
			,Integer age,Double salary
			,Date birth){
		this.id = id;
		this.name = name;
		this.pass = pass;
		this.age = age;
		this.salary = salary;
		this.birth = birth;
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

	public String getPass() {
		return pass;
	}

	public void setPass(String pass) {
		this.pass = pass;
	}

	public Integer getAge() {
		return age;
	}

	public void setAge(Integer age) {
		this.age = age;
	}

	public Double getSalary() {
		return salary;
	}

	public void setSalary(Double salary) {
		this.salary = salary;
	}

	public Date getBirth() {
		return birth;
	}

	public void setBirth(Date birth) {
		this.birth = birth;
	}

	@Override
	public String toString() {
		return "Person [id=" + id + ", name=" + name + ", pass=" + pass
				+ ", age=" + age + ", salary=" + salary + ", birth=" + birth
				+ "]";
	}
}
```

4> sql包下存放.sql文件，在test包下，书写test类，来测试factory类

```java
//测试connection
package com.etoak.test;

import java.sql.Connection;

import com.etoak.factory.Factory;

public class TestConnection {

	public static void main(String[] args) {
		//使用工厂类来获取链接
		Connection con = Factory.getCon();
		if(con==null){
			System.out.println("数据库链接失败!");
			return;
		}
		System.out.println("数据库链接成功!");
		
		
		
		
	}

}
```

//测试sql语句的执行

```java
package com.etoak.test;

import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;

import com.etoak.factory.Factory;

public class TestStatement {
	public static void main(String[] args) {
		
		try{
			//1:拿取链接
			Connection con = Factory.getCon();
			//2:获取执行器
			//执行sql语句的
			Statement st = con.createStatement();
			/*
			 * 使用执行器可以执行以下三种方法
			 * 1):boolean 	execute()
			 * 		用来执行dql语句，返回true
			 * 		执行dml语句，返回false，但是语句
			 * 		依然会被执行
			 * 
			 * 2):int executeUpdate()
			 * 		返回一个数字，注意此数字表示
			 * 		更改的记录数，多用于dml语句
			 * 		如果执行dql语句立刻报错
			 * 3):ResultSet executeQuery()
			 * 		返回一个结果集，专门用来执行
			 * 		dql语句，通过解析结果集拿取
			 * 		从数据库中取出的数据
			 * 
			 * */
		String dql1 = "select * from person";
		String dml1 = 
		"insert into person values (null,'Amy','12345',40,2000.11,'1984-06-03')";
		
		/*boolean flag = st.execute(dml1);
		System.out.println(flag);*/
		
		/*int count = st.executeUpdate(dml1);
		System.out.println(count);*/
		
		ResultSet rs = st.executeQuery(dql1);
		
		/*
		 * 	在解析结果集之前首先要判断是否存在有效数据
		 * 	但是我们不能根据rs是不是为null来判断
		 * 	因为rs类似一个表格，这个表格的表头永远存在
		 * 	所以rs!=null
		 * 		结果集中.next()返回boolean，返回true
		 * 		指针下移，一条记录，返回false则表示
		 * 		指针无法移动，没有有效数据了
		 * 		
		 * 		通过get数组类型(字段列数)
		 * 		来拿取数据
		 * */
			/*while(rs.next()){
				System.out.println("ID:"+rs.getInt(1)
				+"\t用户名:"+rs.getString(2)+"\t密码:"
				+rs.getString(3)+"\t年龄:"+rs.getInt(4)
				+"\t薪资:"+rs.getDouble(5)+"\t生日:"
				+rs.getDate(6));
			}*/
		while(rs.next()){
			System.out.println("ID:"+rs.getInt("id")
			+"\t用户名:"+rs.getString("name")+"\t密码:"
			+rs.getString("pass")+"\t年龄:"+rs.getInt("age")
			+"\t薪资:"+rs.getDouble("salary")+"\t生日:"
			+rs.getDate("birth"));
		}
		 
		}catch(Exception ex){
			ex.printStackTrace();
		}
		 
	}
}
 
```

## 1.2 dom4J_jdbc

#### 1>书写配置文件,sql包下存放.sql文件，

- connection.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<database>
	<db id="mysql">
		<driver>com.mysql.jdbc.Driver</driver>
		<url>jdbc:mysql://localhost:3306/et1803</url>
		<username>root</username>
		<password>joshuae</password>
	</db>
	<db id="oracle">
		<driver>oracle.jdbc.driver.OracleDriver</driver>
		<url>jdbc:oracle:thin:@localhost:1521:orcl</url>
		<username>scott</username>
		<password>tiger</password>
	</db>
</database>
```

```sql

drop table if exists person;

create table person(
	id int primary key auto_increment,
	name varchar(10),
	pass varchar(10),
	age int,
	salary double,
	birth date
);

insert into person values (null,'elena','12345'
,20,5000.11,'1991-01-05');


```

#### 2> 书写Factory包下的FactoryByXml.java

```java
package com.etoak.factory;

import java.io.File;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.util.List;

import org.dom4j.Document;
import org.dom4j.Element;
import org.dom4j.io.SAXReader;

public class FactoryByXml {
	public static Connection getCon(String dbType){
		try{
			SAXReader sax = new SAXReader();
			Document doc = sax.read(
					new File("connection.xml"));
			Element root = doc.getRootElement();
			//拿取所有的一级子元素
			List<Element> firstChild = 
					root.elements();
			//遍历所有的一级子元素
			for(Element firstChildEle:firstChild){
				//拿取一级子元素的id属性值
				String value = 
				firstChildEle.attributeValue("id");
			
				if(value.equalsIgnoreCase(dbType)){
					//elementTextTrim(tagName):拿取特定标签内嵌套的值
					Class.forName(firstChildEle
							.elementTextTrim("driver"));
					String url = 
							firstChildEle.elementTextTrim("url");
					String name = 
							firstChildEle.elementTextTrim("username");
					String pass = 
							firstChildEle.elementTextTrim("password");
				
					return DriverManager.getConnection(url, name, pass);
				}
			
			}
			return null;
		}catch(Exception ex){
			ex.printStackTrace();
			return null;
		}
	}
	
	public static void close(ResultSet rs,
			PreparedStatement pst,Connection con){
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

- 不使用dom4j的factory；  Factory.java

```java
package com.etoak.factory;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.Statement;

public class Factory {
	/*
	 *  此工厂模式用来创建Connection链接
	 *  此链接用来链接数据库
	 *  创建链接首先要加载某一种品牌数据库的
	 *  驱动
	 * 	因为某种品牌的数据库驱动仅仅需要加载一次
	 * 所以我们可以将其放置在静态初始化块中	
	 * */
	static{
		try{
			//使用反射进行类加载的方式加载驱动
			//com.mysql.jdbc.Driver:为mysql的驱动
			Class.forName("com.mysql.jdbc.Driver");
		}catch(Exception ex){
			ex.printStackTrace();
		}
	}
	
	//此静态方法用来创建链接
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
	
	//书写一个close方法，此方法用来有序关闭所有已经开启或者占用的资源
	public static void close(ResultSet rs
			,Statement st,Connection con){
		try{
			if(rs!=null)
				rs.close();
		}catch(Exception ex){
			ex.printStackTrace();
		}finally{
			try{
				if(st!=null)
					st.close();
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

#### 3>   书写po 包下的实体类

```java
package com.etoak.po;

import java.util.Date;

/*
 * POJO类，又称之为实体类
 * 一般用来映射关系表，
 * 或者作为对象存在，是java中最为重要的
 * 类，也是JavaBean最关键的核心类
 * 通过private封装属性
 * 
 * */
public class Person {
	/*
	 * 	这里使用包装类
	 * */
	private Integer id;
	private String name;
	private String pass;
	private Integer age;
	private Double salary;
	private Date birth;
	
	public Person(){}
	
	public Person(Integer id,String name
			,String pass
			,Integer age,Double salary
			,Date birth){
		this.id = id;
		this.name = name;
		this.pass = pass;
		this.age = age;
		this.salary = salary;
		this.birth = birth;
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

	public String getPass() {
		return pass;
	}

	public void setPass(String pass) {
		this.pass = pass;
	}

	public Integer getAge() {
		return age;
	}

	public void setAge(Integer age) {
		this.age = age;
	}

	public Double getSalary() {
		return salary;
	}

	public void setSalary(Double salary) {
		this.salary = salary;
	}

	public Date getBirth() {
		return birth;
	}

	public void setBirth(Date birth) {
		this.birth = birth;
	}

	@Override
	public String toString() {
		return "Person [id=" + id + ", name=" + name + ", pass=" + pass
				+ ", age=" + age + ", salary=" + salary + ", birth=" + birth
				+ "]";
	}
}
```

#### 4>  书写dao包下的类   

- PersonDaoIf.java

```java
package com.etoak.dao;

import java.util.List;

import com.etoak.po.Person;

/*
 * DAO:Data Access Object
 * 数据实现对象，通过这些接口
 * 来实现方法，操纵数据库
 * */
public interface PersonDaoIf {
	//1:添加一个person
	public boolean addPerson(Person per);
	
	//2:根据id删除person
	public boolean delPerson(Integer id);
	
	//3:拿取全部的person
	public List<Person> getAll();
	
	//4:修改一个person
	public boolean updatePerson(Person per);
	
}
```

- 实现类   PersonDaoImpl.java

```java
package com.etoak.dao;

import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;
import java.text.SimpleDateFormat;
import java.util.ArrayList;
import java.util.List;

import com.etoak.factory.Factory;
import com.etoak.po.Person;

public class PersonDaoImpl implements PersonDaoIf {

	Connection con;
	Statement st;
	ResultSet rs;
	
	@Override
	public boolean addPerson(Person per) {
		try{
			String sql = 
			"insert into person values (null,'"
			+per.getName()+"','"+per.getPass()+"',"
			+per.getAge()+","+per.getSalary()+",'"
			+new SimpleDateFormat("yyyy-MM-dd")
			.format(per.getBirth())+"')";
			
			System.out.println(sql);
			//获得链接
			con = Factory.getCon();
			//拿取执行器
			st = con.createStatement();
			//执行sql语句
			return st.executeUpdate(sql)==1;
			
		}catch(Exception ex){
			ex.printStackTrace();
			return false;
		}finally{
			Factory.close(null, st, con);
		}
	}

	@Override
	public boolean delPerson(Integer id) {
		try{
			String sql = 
			"delete from person where id = "+id;
			con = Factory.getCon();
			st = con.createStatement();
			return st.executeUpdate(sql)==1;
		}catch(Exception ex){
			ex.printStackTrace();
			return false;
		}finally{
			Factory.close(null, st, con);
		}
	}

	@Override
	public List<Person> getAll() {
		try{
			String sql = "select * from person";
			
			//获取链接
			con = Factory.getCon();
			//获取执行器
			st = con.createStatement();
			//拿取结果集
			rs = st.executeQuery(sql);
			
			List<Person> list = new ArrayList<>();
			
			while(rs.next()){
				Person per = new Person();
				per.setId(rs.getInt(1));
				per.setName(rs.getString(2));
				per.setPass(rs.getString(3));
				per.setAge(rs.getInt(4));
				per.setSalary(rs.getDouble(5));
				per.setBirth(rs.getDate(6));
				list.add(per);
			}
			return list;
		}catch(Exception ex){
			ex.printStackTrace();
			return null;
		}finally{
			Factory.close(rs, st, con);
		}
	}

	@Override
	public boolean updatePerson(Person per) {
		try{
			/*
			 * 
			 * sql:
			 * 		update 表名 set 字段 = 字段值,
			 * 	字段 = 字段值,字段N = 字段值N where 字段 = 字段值;
			 * */
			String sql = "update person set ";
			
			if(per.getName()!=null){
				sql += " name = '"+per.getName()+"',";
			}
			
			if(per.getPass()!=null){
				sql += " pass = '"+per.getPass()+"',";
			}
			
			if(per.getAge()!=null){
				sql += " age = "+per.getAge()+",";
			}
			
			if(per.getSalary()!=null){
				sql += " salary = "+per.getSalary()+",";
			}
			
			if(per.getBirth()!=null){
				sql += " birth = '"
			+new SimpleDateFormat("yyyy-MM-dd")
				.format(per.getBirth())+"',";
			}
			//去掉最后的逗号
			sql = sql.substring(0,sql.length()-1);
			
			sql += " where id = "+per.getId();
			
			System.out.println(sql);
			
			con = Factory.getCon();
			st = con.createStatement();
			return st.executeUpdate(sql)==1;
			
			
			
		}catch(Exception ex){
			ex.printStackTrace();
			return false;
		}finally{
			Factory.close(rs, st, con);
		}
	}
}
```

- PersonDaoIf2.java

  ```java
  package com.etoak.dao;
  
  import java.util.List;
  
  import com.etoak.po.Person;
  
  public interface PersonDaoIf2 {
  	
  	//1:添加一个person
  	public boolean addPerson(Person per);
  	
  	//2:根据id删除person
  	public boolean delPersonById(Integer id);
  	
  	//3:根据name删除person
  	public boolean delPersonByName(String name);
  	
  	//4:拿取全部用户
  	public List<Person> getAll();
  	
  	//5:根据用户名查询
  	public boolean checkPerson(String name);
  	
  	//6:根据用户名和密码拿取person
  	public Person getPersonByNameAndPass(
  			String name,String pass);
  	
  	//7:拿取总记录数
  	public Integer getCount();
  	
  	//8:分页查询
  	public List<Person> getPage(Integer index
  			,Integer max);
  	
  	//9:修改用户
  	public boolean updatePerson(Person per);
  	
  }
   
  ```

  - PersonDaoImpl2

  ```java
  package com.etoak.dao;
  
  import java.sql.Connection;
  import java.sql.PreparedStatement;
  import java.sql.ResultSet;
  import java.util.ArrayList;
  import java.util.List;
  
  import com.etoak.factory.Factory;
  import com.etoak.factory.FactoryByXml;
  import com.etoak.po.Person;
  
  public class PersonDaoImpl2 implements PersonDaoIf2 {
  
  	Connection con;
  	PreparedStatement pst;
  	ResultSet rs;
  	/*
  	 * 		#Statement与PreparedStatement的不同
  	 * 		以上为两种执行器
  	 * 		
  	 * 		1):PreparedStatement具有强大缓存区，可以预编译
  	 * sql语句，非常节省资源。相同的sql语句仅仅执行一次
  	 * 		2):PreparedStatement一般不会拼接sql语句，而是
  	 * 直接使用占位符?来组装sql语句，从根本上杜绝了sql注入安全隐患
  	 * delete from *** where id = ****
  	 * 
  	 * 3 and pass = 123;drop database ***;
  	 * 		3):如果使用?过多，则Statement与数据库交互较少
  	 * 节省资源
  	 * ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  	 * */
  	
  	@Override
  	public boolean addPerson(Person per) {
  		try{
  			String sql = 
  			"insert into person values (null,?,?,?,?,?)";
  			con = FactoryByXml.getCon("mysql");
  			//此处在拿取执行器时已经将sql语句加载进来
  			pst = con.prepareStatement(sql);
  			//填充占位符,从左往右从1开始
  			pst.setString(1, per.getName());
  			pst.setString(2, per.getPass());
  			pst.setInt(3, per.getAge());
  			pst.setDouble(4, per.getSalary());
  			/*
  			 * java.sql.Date()是
  			 * java.util.Date()的子类
  			 * 只能精确到年月日，通过gettime()方法
  			 * 可以将sql.Date()转换为util.Date()
  			 * */
  			pst.setDate(5,new java.sql.Date(
  					per.getBirth().getTime()));
  			
  			return pst.executeUpdate()==1;
  			
  		}catch(Exception ex){
  			ex.printStackTrace();
  			return false;
  		}finally{
  			FactoryByXml.close(null, pst, con);
  		}
  	}
  
  	@Override
  	public boolean delPersonById(Integer id) {
  		try{
  			String sql = 
  			"delete from person where id = ?";
  			con = FactoryByXml.getCon("mysql");
  			pst = con.prepareStatement(sql);
  			pst.setInt(1,id);
  			return pst.executeUpdate()==1;
  		}catch(Exception ex){
  			ex.printStackTrace();
  			return false;
  		}finally{
  			FactoryByXml.close(null, pst, con);
  		}
  	}
  
  	@Override
  	public boolean delPersonByName(String name) {
  		try{
  			String sql = 
  			"delete from person where name = ?";
  			con = Factory.getCon();
  			pst = con.prepareStatement(sql);
  			pst.setString(1, name);
  			return pst.executeUpdate()!=0;
  		}catch(Exception ex){
  			ex.printStackTrace();
  			return false;
  		}finally{
  			FactoryByXml.close(null, pst, con);
  		}
  	}
  
  	@Override
  	public List<Person> getAll() {
  		try{
  			String sql = "select * from person";
  			con = FactoryByXml.getCon("mysql");
  			pst = con.prepareStatement(sql);
  			rs = pst.executeQuery();
  			List<Person> list = new ArrayList<>();
  			while(rs.next()){
  				Person per = new Person(
  				rs.getInt(1)
  				,rs.getString(2)
  				,rs.getString(3)
  				,rs.getInt(4)
  				,rs.getDouble(5)
  				,rs.getDate(6));
  				list.add(per);
  			}
  			return list;
  		}catch(Exception ex){
  			ex.printStackTrace();
  			return null;
  		}finally{
  			FactoryByXml.close(rs, pst, con);
  		}
  	}
  
  	@Override
  	public boolean checkPerson(String name) {
  		// TODO Auto-generated method stub
  		return false;
  	}
  
  	@Override
  	public Person getPersonByNameAndPass(String name, String pass) {
  		// TODO Auto-generated method stub
  		return null;
  	}
  
  	@Override
  	public Integer getCount() {
  		// TODO Auto-generated method stub
  		return null;
  	}
  
  	@Override
  	public List<Person> getPage(Integer index, Integer max) {
  		// TODO Auto-generated method stub
  		return null;
  	}
  
  	@Override
  	public boolean updatePerson(Person per) {
  		// TODO Auto-generated method stub
  		return false;
  	}
  
  }
  
  ```

  #### 5> test包下的  test类

  - TestStatement

  ```java
  package com.etoak.test;
  
  import java.text.SimpleDateFormat;
  
  import com.etoak.dao.PersonDaoIf;
  import com.etoak.dao.PersonDaoImpl;
  import com.etoak.po.Person;
  
  public class TestStatement {
  	public static void main(String[] args) {
  		
  		try{
  			//1:拿取链接
  			/*Connection con = Factory.getCon();
  			//2:获取执行器
  			//执行sql语句的
  			Statement st = con.createStatement();*/
  			/*
  			 * 使用执行器可以执行以下三种方法
  			 * 1):boolean 	execute()
  			 * 		用来执行dql语句，返回true
  			 * 		执行dml语句，返回false，但是语句
  			 * 		依然会被执行
  			 * 
  			 * 2):int executeUpdate()
  			 * 		返回一个数字，注意此数字表示
  			 * 		更改的记录数，多用于dml语句
  			 * 		如果执行dql语句立刻报错
  			 * 3):ResultSet executeQuery()
  			 * 		返回一个结果集，专门用来执行
  			 * 		dql语句，通过解析结果集拿取
  			 * 		从数据库中取出的数据
  			 * 
  			 * */
  		String dql1 = "select * from person";
  		String dml1 = 
  		"insert into person values (null,'Amy','12345',40,2000.11,'1984-06-03')";
  		
  		/*boolean flag = st.execute(dml1);
  		System.out.println(flag);*/
  		
  		/*int count = st.executeUpdate(dml1);
  		System.out.println(count);*/
  		
  		//ResultSet rs = st.executeQuery(dql1);
  		
  		/*
  		 * 	在解析结果集之前首先要判断是否存在有效数据
  		 * 	但是我们不能根据rs是不是为null来判断
  		 * 	因为rs类似一个表格，这个表格的表头永远存在
  		 * 	所以rs!=null
  		 * 		结果集中.next()返回boolean，返回true
  		 * 		指针下移，一条记录，返回false则表示
  		 * 		指针无法移动，没有有效数据了
  		 * 		
  		 * 		通过get数组类型(字段列数)
  		 * 		来拿取数据
  		 * */
  			/*while(rs.next()){
  				System.out.println("ID:"+rs.getInt(1)
  				+"\t用户名:"+rs.getString(2)+"\t密码:"
  				+rs.getString(3)+"\t年龄:"+rs.getInt(4)
  				+"\t薪资:"+rs.getDouble(5)+"\t生日:"
  				+rs.getDate(6));
  			}*/
  		/*while(rs.next()){
  			System.out.println("ID:"+rs.getInt("id")
  			+"\t用户名:"+rs.getString("name")+"\t密码:"
  			+rs.getString("pass")+"\t年龄:"+rs.getInt("age")
  			+"\t薪资:"+rs.getDouble("salary")+"\t生日:"
  			+rs.getDate("birth"));
  		}*/
  			PersonDaoIf dao = new PersonDaoImpl();
  		
  			Person per = new Person(4,"修改的名字"
  			,"6666666"
  			,100,1000.00
  			,new SimpleDateFormat("yyyy-MM-dd")
  			.parse("1900-05-03"));
  			
  			//System.out.println(dao.addPerson(per));
  			//System.out.println(dao.delPerson(1));
  			
  			/*for(Person pers:dao.getAll()){
  				System.out.println(pers);
  			}*/
  			System.out.println(dao.updatePerson(per));
  			 
  		}catch(Exception ex){
  			ex.printStackTrace();
  		}
  		
  		 
  	}
  }
  ```

  - TestPreparedStatement.java

  ```java
  package com.etoak.test;
  
  import java.text.SimpleDateFormat;
  
  import com.etoak.dao.PersonDaoIf2;
  import com.etoak.dao.PersonDaoImpl2;
  import com.etoak.po.Person;
  
  public class TestPreparedStatement {
  
  	public static void main(String[] args)throws Exception {
  		/*Connection con = 
  		FactoryByXml.getCon("mysql");
  		if(con==null){
  			System.out.println("链接数据库失败！");
  			return;
  		}
  		System.out.println("链接数据库成功！");*/
  	
  		Person per = new Person(null,"elena"
  		,"12345",20,3000.11
  		,new SimpleDateFormat("yyyy-MM-dd")
  		.parse("1990-03-02"));
  	
  		PersonDaoIf2 dao = new PersonDaoImpl2();
  		//System.out.println(dao.addPerson(per));
  		//System.out.println(dao.delPersonById(5));
  		//System.out.println(dao.delPersonByName("Amy"));
  	
  		for(Person pers:dao.getAll()){
  			System.out.println(pers);
  		}	
  	}
  }
   
  ```

## 1.3 批处理速度

```java
1. PreparedStatement使用批处理 executeBatch()
1.1. 不使用executeBatch()，而使用executeUpdate()
          代码如下：  
         Class.forName("com.mysql.jdbc.Driver");
         Connection conn = DriverManager.getConnection(dbUrl, user, password);
         PreparedStatement pstmt = conn.prepareStatement("update content set introtext=? where id=?");
         for(int i=0; i<10000; i++){
             pstmt.setString(1, "abc"+i);
             pstmt.setInt(2, id);
             pstmt.executeUpdate();
         }
         这样，更新10000条数据，就得访问数据库10000次
1.2. 而使用executeBatch()
          代码如下：
         Class.forName("com.mysql.jdbc.Driver");
         Connection conn = DriverManager.getConnection(dbUrl, user, password);
         PreparedStatement pstmt = conn.prepareStatement("update content set introtext=? where id=?");
         for(int i=0; i<10000; i++){
             pstmt.setString(1, "abc"+i);
             pstmt.setInt(2, id);
             pstmt.addBatch();//添加到同一个批处理中
         }
         pstmt.executeBatch();//执行批处理

 
注意：1. 如果使用了 addBatch() -> executeBatch() 还是很慢，那就得使用到这个参数了
                      rewriteBatchedStatements=true (启动批处理操作)
                      在数据库连接URL后面加上这个参数：      
                          String dbUrl =  "jdbc:mysql://localhost:3306/User? rewriteBatchedStatements=true";
                      2. 在代码中，pstmt的位置不能乱放，
                          //必须放在循环体外
                     pstmt = conn.prepareStatement("update content set introtext=? where id=?");
                     for(int i=0; i<10000; i++){
                           //放这里，批处理会执行不了，因为每次循环重新生成了pstmt，不是同一个了
                           //pstmt = conn.prepareStatement("update content set introtext=? where id=?");
                           pstmt.setString(1, "abc"+i);
                           pstmt.setInt(2, id);
                           pstmt.addBatch();//添加到同一个批处理中
                     }
                     pstmt.executeBatch();//执行批处理
2. 启用事务处理
          Class.forName("com.mysql.jdbc.Driver");
 
          Connection conn = DriverManager.getConnection(dbUrl, user, password);
          conn.setAutoCommit(false);//将自动提交关闭
          PreparedStatement pstmt = conn.prepareStatement("update content set introtext=? where id=?");
          pstmt.setString(1, tempintrotext);
          pstmt.setInt(2, id);
          pstmt.addBatch();
          pstmt.executeBatch();
          pstmt.close();

          conn.commit();//执行完后，手动提交事务
          conn.setAutoCommit(true);//在把自动提交打开
          conn.close();
3. 事务和批处理混合使用      
          Class.forName("com.mysql.jdbc.Driver");
          Connection conn = DriverManager.getConnection(dbUrl, user, password);
          conn.setAutoCommit(false);//将自动提交关闭
          PreparedStatement pstmt = conn.prepareStatement("update content set introtext=? where id=?");
          for(int i=0; i<1000000; i++){
               pstmt.setString(1, tempintrotext);
               pstmt.setInt(2, id);
               pstmt.addBatch();
               //每500条执行一次，避免内存不够的情况，可参考，Eclipse设置JVM的内存参数
               if(i>0 && i%500==0){
                    pstmt.executeBatch();
                    //如果不想出错后，完全没保留数据，则可以没执行一次提交一次，但得保证数据不会重复
                    conn.commit();
                }
         }
          pstmt.executeBatch();//执行最后剩下不够500条的
          pstmt.close();
          conn.commit();//执行完后，手动提交事务
          conn.setAutoCommit(true);//在把自动提交打开
          conn.close();
```

- 结果

  对应速度测试.png
  分别是： 不用批处理，不用事务；
          只用批处理，不用事务；
          只用事务，不用批处理；
          既用事务，也用批处理；（很明显，这个最快，所以建议在处理大批量的数据时，同时使用批处理和事务）

## 1.4 JDBC 结构图

```
1.DriverManager（类）
作用有两个：（1）管理不同数据库的驱动程序 Driver，用于注册数据库驱动
            （2）获得数据库的连接对象，DriverManger.getConnection(url,name,password)
2.Driver（接口）
Driver是连不上数据库的，只有实现 Driver接口的具体实现类才能连接上数据库，也只有 Driver是个接口才能实现使用通用的 JDBC API来访问不同的数据库
3.Connection（接口）
用于打开应用程序与数据库之间的连接通道，具体如何打开通道则由各数据库厂商提供实现，即 Connection接口的实现类来完成
4.Statement（接口）
应用程序利用 Statement向数据库发送 SQL语句，用于发送静态 SQL语句，一次性写死完整的 SQL语句
5.PreparedStatement（接口）
应用程序利用 PreparedStatement向数据库发送动态的 SQL语句，一些 SQL语句中的参数是在程序运行时动态得到的，所以先用“？”这个占位符站住动态变化的参数的位置
6.CallableStatement（接口）
在 PL/SQL中使用，把 SQL语句保存到数据库中，当需要使用这条 SQL语句时，就可以像调用方法一样来调用数据库中的这条 SQL语句
7.ResultSet（接口）
利用 ResultSet把 select查询结果返回给应用程序，并把查询结果封装到 ResultSet中
8.DatabaseMetadata（接口）
得到数据库的信息，如数据库版本号，驱动程序版本号等信息
9.ResultSetMetadata（接口）
封装数据库的元信息，元信息如：表结构的信息，列的类型、约束等信息
10.Type（接口）
封装了一些常量
11.DataSource（接口）
把创建好的连接对象放到一个池中，我们叫连接池，其实是一个集合，可以重复使用而不用释放连接对象，以提高效率，因为网络资源很宝贵

```

## 1.5  日期转换

```
Java.util.Date是在除了SQL语句的情况下面使用的。
java.sql.Date是针对SQL语句使用的，它只包含日期而没有时间部分
它们都有getTime方法返回毫秒数，自然就可以直接构建。 java.util.Date 是 java.sql.Date 的父类，前者是常用的表示时间的类，我们通常格式化或者得到当前时间都是用他，后者之后在读写数据库的时候用他，因为PreparedStament的setDate()的第2参数和ResultSet的getDate()方法的第2个参数都是java.sql.Date。
java.sql.Date转为java.util.Date
java.sql.Date date=new java.sql.Date();
java.util.Date d=new java.util.Date (date.getTime());
```

## 1.6 sql注入攻击

```
一、SQL注入介绍

SQL注入就是将原本的SQL语句的逻辑结构改变，使得SQL语句的执行结果和原本开发者的意图不一样；
方法：在表单中将命令当作用户输入提交给程序；

二、SQL注入范例

这里我们根据用户登录页面
 
<form action="" >  
用户名：<input type="text" name="username"><br/>  
密  码：<input type="password" name="password"><br/>  
 </form>  
 
预先创建一个表：
 
create table user_table(  
    id      int Primary key,  
    username    varchar(30),  
    password    varchar(30)  
);  

insert into user_table values(1,'xiazdong-1','12345');  
insert into user_table values(2,'xiazdong-2','12345');  

一般查询数据库的代码如下：
[java] view plain copy

 
public class Demo01 {  
    public static void main(String[] args) throws Exception {  
        String username = "xiazdong";  
        String password = "12345";    
        String sql = "SELECT id FROM user_table WHERE " + "username='" + username  
                + "'AND " + "password='" + password + "'";  
        Class.forName("com.mysql.jdbc.Driver");  
        Connection con = DriverManager.getConnection("jdbc:mysql://localhost:3306/db1","root","12345");  
        PreparedStatement stat = con.prepareStatement(sql);  
        System.out.println(stat.toString());  
        ResultSet rs = stat.executeQuery();  
        while(rs.next()){  
            System.out.println(rs.getString(1));  
        }  
    }  
}  

但是这里username=xiazdong,password=12345,
因此此处的SQL语句为：

SELECT id FROM user_table WHERE username='xiazdong' AND password='12345';  

如果我们把username和password的值变为：
username='  OR 1=1 -- 
password=x
会变成一个很可怕的情况：将把数据库中所有用户都列出来，为什么呢？
因为SQL语句现在为：

SELECT id FROM user_table WHERE username='' OR 1=1 -- ' AND password='12345';  

因为--表示SQL注释，因此后面语句忽略；
因为1=1恒成立，因此 username='' OR 1=1  恒成立，因此SQL语句等同于：

 
SELECT id FROM user_table;  

很奇妙吧....

三、解决方法

其实解决方法很简单，就是使用PreparedStatement即可；
```

##  1.7 JDBC驱动

```java
-------------------------------oracle--------------------------------

驱动：oracle.jdbc.driver.OracleDriver
URL：jdbc:oracle:thin:@<machine_name><:port>:dbname
     jdbc:oracle:thin:@localhost:1521:orcl
注：machine_name：数据库所在的机器的名称；
      port：端口号，默认是1521
      
-------------------------------mysql---------------------------------

驱动：  com.mysql.jdbc.Driver
URL：jdbc:mysql://<machine_name><:port>/dbname
     jdbc:mysql://localhost:3306/etoak
注：machine_name：数据库所在的机器的名称；
      port：端口号，默认3306
     
----------------------------pointbase---------------------------------

驱动：com.pointbase.jdbc.jdbcUniversalDriver
URL：jdbc:pointbase:server://<machine_name><:port>/dbname
注：machine_name：数据库所在的机器的名称；
      port：端口号，默认是9092
      
---------------------------SQL Server---------------------------------

驱动：com.microsoft.jdbc.sqlserver.SQLServerDriver
URL：jdbc:microsoft:sqlserver://<machine_name><:port>;DatabaseName=<dbname>
注：machine_name：数据库所在的机器的名称；
      port：端口号，默认是1433

--------------------------DB2---------------------------------------------

驱动：com.ibm.db2.jdbc.app.DB2Driver
URL：jdbc:db2://<machine_name><:port>/dbname
注：machine_name：数据库所在的机器的名称；
      port：端口号，默认是5000

--------------------------DB2---------------------------------------------


在开发 JBDC 中总的协议只有一个  jdbc

url定义了协议、具体数据库产品、自协议、IP地址和数据库名等信息
mysql url："jdbc:myql://IP地址:端口号(3306)/数据库名" 

oracle url："jdbc:oracle:thin:@IP地址:端口号(1521):数据库名"

jdbc：代表一个总的协议    
mysql/oracle：对应了真实的数据库产品    
第三部分：自协议，可选，如：oracle的自协议是 thin

Access:
  driver sun.jdbc.odbc.JdbcOdbcDriver
  url: jdbc:odbc:et1101
       jdbc:odbc 数据库协议 et0909数据源的名字
  
、JDBC的工作原理 

Java Database Connectivity API(JDBC)的工作原理。正如其名字揭示的，JDBC库提供了一个底层API，用来支持独立于任何特定SQL实现的基本SQL功能。提供数据库访问的基本功能。它是将各种数据库访问的公共概念抽取出来组成的类和接口。JDBC API包括两个包：java.sql(称之为JDBC内核API)和javax.sql（称之为JDBC标准扩展）。它们合在一起，包含了用Java开发数据库应用程序所需的类。这些类或接口主要有：
Java.sql.DriverManager
Java.sql.Driver
Java.sql.Connection
Java.sql.Statement
Java.sql.PreparedStatement
Java.sql.ResultSet等 

这使得从Java程序发送SQL语句到数据库变得比较容易，并且适合所有SQL方言。也就是说为一种数据库如Oracle写好了java应用程序后，没有必要再为MS SQL Server再重新写一遍。而是可以针对各种数据库系统都使用同一个java应用程序。这样表述大家可能有些难以接受，我们这里可以打一个比方：联合国开会时，联合国的成员国的与会者（相当我们这里的具体的数据库管理系统）往往都有自己的语言（方言）。大会发言人（相当于我们这里的java应用程序）不可能用各种语言来发言。你只需要使用一种语言（相当于我们这里的JDBC）来发言就行了。那么怎么保证各成员国的与会者都听懂发言呢，这就要依靠同声翻译（相当于我们这里的JDBC驱动程序）。实际上是驱动程序将java程序中的SQL语句翻译成具体的数据库能执行的语句，再交由相应的数据库管理系统去执行。因此，使用JDBC API访问数据库时，我们要针对不同的数据库采用不同的驱动程序，驱动程序实际上是适合特定的数据库JDBC接口的具体实现，它们一般具有如下三种功能：

建立一个与数据源(数据库)的连接

发送SQL语句到数据源

取回结果集 

那么，JDBC具体是如何工作的呢？ 

Java.sql.DriverManager装载驱动程序，当Java.sql.DriverManager的getConnection()方法被调用时，DriverManager试图在已经注册的驱动程序中为数据库（也可以是表格化的数据源）的URL寻找一个合适的驱动程序，并将数据库的URL传到驱动程序的acceptsURL()方法中，驱动程序确认自己有连接到该URL的能力。生成的连接Connection表示与特定的数据库的会话。Statement(包括PreparedStatement和CallableStatement)对象作为在给定Connection上执行SQL语句的容器。执行完语句后生成ResultSet结果集对象，通过结果集的一系列getter就可以访问表中各列的数据。 
```

