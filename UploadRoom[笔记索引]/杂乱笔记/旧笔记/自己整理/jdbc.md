# Jdbc

## [1] 含义

```
可是使用相同的Java代码，操纵不同品牌的数据库，
	对数据库进行增删查改等操作，jdbc是最早的
	ORM工具（Object Mapping Relation对象映射关系）
	著名的ORM工具有
		全手动:JDBC
		半自动:Mybatis
		全自动:Hibernate
	JDBC是JavaEE的十三种标准技术之一，所有的工具类都放置
	在java.sql包中
```

## [2] 理解

```
1 jdbc的用处就是用java来连接数据库的
2 Java是如何连接数据库的
	Java官方制定了Java连接数据库的标准[接口集Java.sql.*],数据库厂商实现Java连接各自数据库[厂商要实现的标准]，程序员需要链接数据库的时候需要到各自的官网，下载相应的应用[Java链接数据库的应用--->驱动]
3 Java官方制定标准，厂商实现标准，程序员学习标准，使用驱动
```

## [3] 流程

```
1 建立关系表 
	一般在com.etoak.sql包中创建 ***.sql	文件 搭建好连接之后执行sql文件 ---其	中的sql语句
2 建立对象-实体类
	一般在com.etoak.po中创建 使用包装类，必须创建空参的构造方法，也可以创建有参的构造方法，覆盖toString()方法 此对象正好对应关系表
3 先导入MySQL.jar包--用来连接MySQL的
  创建工厂
  	1）通过静态初始化块利用反射加载驱动--- 此工厂模式用来创建Connection链接，此链接用来链接数据库，创建链接首先要加载某一种品牌数据库的驱动，所以我们可以将其放置在静态初始化块中，因为某种品牌的数据库驱动仅仅需要加载一次
	2）通过获取连接与数据库连接成功--通过创建静态方法来创建连接--静态方法可以在别的包中被调用
	3) 创建一个xml文件，用来存储MySQL的连接驱动 和连接地址，用户名和密码
	   通过利用dom4J进行对这个xml文件的读取来加载驱动和创建连接
	   还可以用来对结果集（ResultSet）和执行器（PreparedStatement）和 连接（Connection）的关闭节约资源
 4 创建dao层
 4 创建测试类
	1）通过工厂获取连接--调用工厂包中的工厂类的连接数据库的方法（Connection）
	2）通过连接拿取执行器Statment--用来执行SQL语句的
		1) boolean execute() -用来执行dql语句 返回true 执行dml语句 返回false 但语句依旧被执行
		2）int executeUpdate() 只能执行dml语句 返回的是修改的次数 每次只能执行一条语句
		3) Result executeQuery() 返回的是结果集 只能执行dql语句 
			(1) 先判断其中是否存在数据 但是我们不能根据其是否为空来判断 因为其中的格式是类似于一个表格的形						式存在的，表头是永远存在的
				.next() 判断
			(2) 拿去对应的数据--两种方式
				1）get数据类型(列数)
				2）get数据类型(列名)
```

### [1] 用代码连接MySQL

#### [1] 第一种-固定格式

```
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
```

#### [2] 第二种-读取xml文件

```
		try{
			SAXReader sax = new SAXReader();获取解析器
			Document doc = sax.read(new File("Connection.xml"));通过解析器解析xml文件
			Element root =doc.getRootElement();获取根元素
			List<Element> firstChild =root.elements();获取第一级元素
			for(Element firstChildEle: firstChild){
				String value = firstChildEle.attributeValue("id");获取第一级元素的值
				if(value.equalsIgnoreCase(dbType)){
					Class.forName(firstChildEle.elementTextTrim("driver"));通过获取值来加载驱动
					String url =firstChildEle.elementTextTrim("url");通过一级元素获取值
					String name = firstChildEle.elementTextTrim("username");通过一级元素获取值
					String pass = firstChildEle.elementTextTrim("password");通过一级元素获取值
					return DriverManager.getConnection(url,name,pass);创建连接
				}
			}
			return null;
		}catch(Exception e){
			e.printStackTrace();
			return null;
		}
```

xml 文件

```
<?xml version="1.0" encoding="UTF-8"?>
<database>
	<db id="mysql">
		<driver>com.mysql.jdbc.Driver</driver>
		<url>jdbc:mysql://localhost:3306/et1803</url>
		<username>root</username>
		<password>etoak</password>
	</db>
	<db id="oracle">
		<driver>oracle.jdbc.driver.OracleDriverr</driver>
		<url>jdbc:oracle:thin:@localhost:1521:orcl</url>
		<username>scott</username>
		<possword>etoak</possword>
	</db>
</database>
```

### [2] Dao层

#### [1] 接口

​	创建方法(增删改查)与包装类有关系

```java
package com.etoak.dao;
import java.util.List;
import com.etoak.po.Person;
public interface PersonDaoIf {
		1 //添加一个person
		public boolean addPerson(Person per);
		2 //根据id删除person
		public boolean delPerson(Integer id);
		3 //根据name删除person
		pulic boolean delPerson (String name);
		4 //拿取全部person
		public List<Person> getAll();
		5 //根据用户名查询
		punlic boolean checkPerson(String name);
        6 //根据用户名和密码进行查询
        public Person getPersonByNameAndPass(String name,String pass);
        7 //拿取总记录数
        public Integer getCount();
        8 //分页查询
        public List<Person> getPage(integer index,Integer max);
		9 //修改一个person
		public boolean updatePerson(Person per);
}

```

#### [2] 实现类

​	实现接口

```java
package com.etoak.dao;
import java.util.List;
import com.etoak.po.Person;
public class Td implements PersonDaoIf2 {
  	Connection con;
  	PraparementStatement pst;
  	ResultSet rs;
  	//添加一个person
	@Override
	public boolean addPerson(Person per) {
      try{
        String sql = "insert into Person values(null,?,?,?,?,?)";
        con = Factory.con();
        pst = con.prepareStatement(sql);
        pst.setString(1,per.getName());
		pst.setString(2,per.getPass());
		pst.setInt(3,per.getAge());
		pst.setDouble(4,per.getSalary());
		pst.setDate(5,new java.sql.Date(per.getBirth().getTime()));
		return pst.executeUpdate()==1;
      }catch(Exception e){
        e.printStackTrace();
        return false;
      }fianlly{
        Factory.close(null,pst,con);
      }
	}
  	//根据ID删除Person
	@Override
	public boolean delPersonById(Integer id) {
      try{
        String sql="delete from person where id=?";
        con = Factory.getCon();
        pst = con.prepareStatement(sql);
        pst.setInt(1,id);
        return pst.executeUpdate()==1;
      }catch(Exception e){
        e.printStackTrace();
        return false;
      }fianlly{
        Factory.close(null,pst,con);
      }
	}
  	//根据name删除Person
	@Override
	public boolean delPersonByName(String name) {
      try{
        String sql = "delete from person where name=?";
        con = Factory.getCon();
        pst = con.prepareStatement(sql);
        pst.setString(1,name);
        return pst.executeUpdate()==1;
      }catch(Exception e){
        e.printStackTrace();
      }fianlly{
        Factory.close(null,pst,con);
      }
	}
  	//查询全部
	@Override
	public List<Person> getAll() {
      try{
        String sql = "select * from person";
        con = Factory.getCon();
        pst = con.prepareStatement(sql);
        rs = pst.executeQuery();
        List<Person> list = mew ArrayList<Person>();
        while(rs.next()){
          Person per = new Person(rs.getInt(1),rs.getString(2),rs.getString(3),rs.getInt(4),rs.getDouble(5),rs.getDate(6));
          list.add(per);
        }
        return list;
      }catch(Exception e){
        e.printStackTrace();
      }finally{
        Factory.close(rs,pst,con);
      }
	}
  	//根据用户名查询
	@Override
	public boolean checkPerson(String name) {
      try{
        String sql = "select * from person where name=?";
        con = Factory.getCon();
        pst = con.prepareStatement(sql);
        pst.setString(1,name);
        return pst.executeUpdate()==1;
      }catch(Exception e){
        e.printStackTrace()
      }finally{
        Factory.close(rs,pst,con);
      }
	}
  	//根据用户名和密码拿取person
	@Override
	public Person getPersonByNameAndPass(String name, String pass) {
      try{
        String sql = "select * from person where name=? and pass=?";
        con = Factory.getCon();
        pst = con.prepareStatement(sql);
        pst.setString(1,name);
        pst.SetString(2,pass);
        rs = pst.executeQuery();
        List<Person> list = new ArrayList<Person>();
        while(rs.next()){
          Person per = new Person(rs.getInt(1),rs.getString(2),rs.getString(3),rs.getInt(4),rs.getDouble(5),rs.getDate(6));
          list.add(per);
        }
        return list;
      }catch(Exception e){
        e.printStackTrace();
        return null;
      }finally{
        Factory.close(rs,pst,con);
      }
	}
  	//拿取总数
	@Override
	public Integer getCount() {
      try{
        String  sql = "select count(*) from person";
        con = Factory.getCon();
        pst = con.prepareStatement(sql);
        rs = pst.executeQuery();
        rs.next();
        return rs.getInt(1);
      }catch(Exception e){
        e.printStackTrace();
        return null;
      }finally{
        Factory.close(rs,pst,con);
      }
	}
  	//分页
	@Override
	public List<Person> getPage(Integer index, Integer max) {
      try{
        String sql = "select * from person limit?,?";
        con = Factory.getCon();
        pst = con.prepareStatement();
        pst.setInt(1,index);
        pst.setInt(2,max);
        rs = pst.executeQuery();
        List<Person> list = new ArrayList<Person>();
        while(rs.next()){
          Person per = new Person(rs.getInt(1),rs.getString(2),rs.getString(3),rs.getInt(4),rs.getDouble(5),rs.getDate(6));
          list.add(per);
        }
        return list;
      }catch(Exception e){
        e.printStackTrace();
        return null;
      }finally{
        Factory.close(rs,pst,con);
      }
	}
  	//修改用户
	@Override
	public boolean updatePerson(Person per) {
      try{
        String sql = "update person set ";
        if(per.getName() !=null){
          sql +=" name = '"+per.getName()+"',";
        }
        if(per.getPass()!=null){
          sql+=" pass = '"+per.getPass()+"',";
        }
        if(per.getAge()!=null){
			sql +=" age = "+per.getAge()+",";
		}
		if(per.getSalary()!=null){
			sql +=" salary = "+per.getSalary()+",";
		}
		if(per.getBirth()!=null){
			sql +=" birth = '"+new SimpleDateFormat("yyyy-MM-dd").format(per.getBirth())+"',";
		}
		sql = sql.substring(0,sql.length()-1);//去掉最后的逗号
			
		sql +=" where id ="+per.getId();
			
		System.out.println(sql);
			
		con =Factory.getCon();//获取连接
		st = con.createStatement();//获取执行器
		return st.executeUpdate(sql)==1;
      }catch(Exception e){
        e.printStackTrace();
        return false;
      }finally{
 		Factory.close(null,pst,con);       
      }
	}
}
```

### [3] 两种执行器的不同

```
#Statement与PreparedStatement的不同以上为两种执行器
		
		1):PreparedStatement具有强大缓存区，可以预编译
sql语句，非常节省资源。相同的sql语句仅仅执行一次
		2):PreparedStatement一般不会拼接sql语句，而是
直接使用占位符?来组装sql语句，从根本上杜绝了sql注入安全隐患
delete from *** where id = ****

3 and pass = 123;drop database ***;
		3):如果使用?过多，则Statement与数据库交互较少
节省资源

```

### [4] 元数据[MetaData]数据的数据

#### [1] 数据库的元数据[java.sql.DatabaseMetadata]

```
1 数据库的基本信息
```

#### [2] 结果集的元数据[ResultSetMetadata]

```
1 先获取结果集
	ResultSet rs = 执行器.executeQuery();
2 通过结果集获取元数据
	ResultSetMetadata rsmd = rs.getMetaData();
3 获取列数
	int count = rsmd.getColumnCount();
4 通过while(rs.next())控制行数
5 通过for循环控制列数
6 获取列的名字 
	String name = rsmd.getColumnName();
7 获取列的长度
	int length = rsmd.getColumnzDisplaySize();
8 获取列的类型
	String Type = rsmd.getColumnTypeName();
9 获取列的内容
	String value = rs.getString(name);
```

## [4] 事务

### [1] 含义

```
数据库之中的一组相关的sql语句，作为一个执行的单元，全部执行或者全部不执行，不可拆分的这种sql组合的执行方式事务
```

### [2] 案例[转账]

```
1 update ka1 set money = money-1;
2 update ka2 set money = money+1;
```

### [3] 注意

jdbc中的事务是不开启的，因为jdbc中的都是自动提交的

### [4] jdbc控制事务的方式

```
1 关闭自动提交
	Connection.setAutoCommit(false);
2 执行sql语句
	sql1 sql2
3 提交和回滚
	Connection.Commit(); 
	Connection.rollback();
```

### [5] 事务的特征

```
1 原子性：多条语句不可再分
2 一致性：数据库事务执行前后，数据的状态是一致的
3 隔离性：事务与实务之间影响的程序
4 持久性：事务执行完之后需要真正影响数据库
```

### [6] 事务的隔离级别

```
1 读未提交
2 读已提交
3 可重复读
4 串行化
```

### [7] 隔离级别用来解决什么

```
1 数据库中的脏读
2 数据库中的不可重复读
3 幻读
```

## [5] 数据源[DataSource]

### [1] 含义

```
1 数据源是用来获得连接的工厂，是用来顶替DriverManager
2 DataSource:是一个接口(标准),有很多的实现方案，最常用的一种是:采用连接池(DBCP)的方式实现数据源
```

### [2] 数据源和连接池和DBCP的关系

```
1 数据源[DataSource]是标准
2 连接池是解决方案，不是jar包
3 DBCP是采用了连接池方案的具体的实现
```

### [3] 常见的连接池组件

```
1 dbcp
2 c3p0
3 DRIUD
4 mybatis中的POOLED
```

### [4] DBCP的工作原理[重复使用]

```
1 当服务器启动的时候，创建一些链接放在链接池中，等待客户端请求
2 当客户端请求的时候，先判断链池中是否有空闲的连接，如果有直接返回，如果没有,则判断连接数是否超过最大可用连接数，没有则创建新的连接返回，超过了则抛出异常
3 当客户端使用完连接时，将连接放回连接池中，从而实现连接的重复使用
```

### [5] DBCP的组件

```
1 DBCP是Apache-commons提供的一套采用数据库连接池的方案的javax.sql.DataSource接口的具体实现组件
2 DBCP的作用是--获得连接
```

### [6] DBCP的案例

```java
//创建对象
BasicDataSource ds = new BasicDataSource();
//给属性赋值--Driver URL  Username  password 必须的
ds.setDriverClassName("com.mysql.jdbc.Driver");
ds.setUrl("jdbc:mysql://localhost:3306/et1803");
ds.setUsername("root");
ds.setPassword("etoak");
//可选项
ds.setMaxActive(20);//设置最大的可用空闲连接数
ds.setMaxWite(3000);//设置最大的等待时间
ds.setMaxIdle(10);//设置最大的空闲连接数
//获得连接
Connection con = ds.getConnection();
```

## [6] 封装jdbc

```
1 将可变的的内容以参数的的内容传递下来
2 将不变的内容固定下来
```

```Java
	BasicDataSource bs = new BasicDataSource();
		bs.setDriverClassName("com.mysql.jdbc.Driver");
		bs.setUrl("jdbc:mysql:///et1803");
		bs.setUsername("root");
		bs.setPassword("etoak");
——————————————————	上边这些可以写在工厂类中————————————————————————————————————————————————
		String sql = "select * from News";
		QueryRunner qr = new QueryRunner(bs);
		List<News> stus = (List<News>)qr.query(sql,new BeanListHandler(News.class));
		for(News news:stus){
			System.out.println(news.getId()+"\t"+news.getTitle()+"\t"+news.getAuthor());
		}
通过到如dbUtil包，可以newQueryRanner对象，可以调用query方法(很多Query方法)，里边可以传三个参数，connection连接，sql语句，BeanLIstHandler(实体类.class)
这样可以免去写dao层，这样就可以对jdbc进行封装
```

## [7] java处理Excel

注意：需要导入jxl.jar，这个jar包

1 读Excel文件

```java 
Workbook wb = new Workbook(new File("文件名.xls"));//获得这个workbook
Sheet sheet = wb.getSheet(0);//获得第一个sheet
int count = sheet.getColumnCount();//获得列数
for(int i=0;i<count;i++){
  Cell[] cell = sheet.getColumn(i);//获取某一列
  for(Cell cells : cell){
    System.out.println(cells.getContents()+" ");
  }
  System.out.println();
}
```

2 写出Excel文件

```java
WritableWorkbook  wb = new WritableWorkbook(new File("文件名.xls"));//创建一个Workbook
WritableSheet sheet = wb.createSheet("sheet名字",0);//创建第一个sheet
Label id = new Label(0,0,"id");//第一列第一行添加id
Label name = new Label(1,0,"name");//第二列第一行添加name
Label pass = new Label(2,0,"pass");//向sheet的第三列第一行添加name

Label vid = new Label(0,1,"ET001");//第一列第二行添加name
Label vname = new Label(1,1,"张三");//第二列第二行添加name
Label vpass = new Label(2,1,"12345");//第三列第二行添加name
sheet.add(id);sheet.add(name);sheet.add(pass);//将其添加到sheet中
sheet.add(vid);sheet.add(vname);sheet.add(vpass);
wb.write();//写出
wb.close();
```

