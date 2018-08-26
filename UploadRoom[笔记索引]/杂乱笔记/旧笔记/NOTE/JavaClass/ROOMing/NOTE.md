##  5.15

复习对接：  JDBC    理解 ： 

   官方制定标准，数据库实现，开发者使用

元数据

9.ResultSetMetadata（接口）
封装数据库的元信息，元信息如：表结构的信息，列的类型、约束等信息
10.Type（接口）
封装了一些常量



java.sql.DataSource(数据源)

  网址： apache.org

## 5.16

- JDBC封装

- DBUtil

- 处理Excle文件

  ------

  ## web基础知识

1.    两种架构
2. 



Servlet 总结

##   5.17

tomcat工作原理

下载文件

## 5.18

- 跳转

    同步

   

  

  

    异步

servlet   

 相对路径  不是以 '/'开头的

绝对路径

转发

​    request.getRequestDispatcher("a/b/c/d/other").forward(request, response)；

重定向

​	response.sendRedirect(request.getContextPath()+"/servlet/a/b/c/d/other");

<a herf="<%=request.geetContextPath()%>/servlet/login"></a>



####  Cookie 和 Session

- cookie的使用场景

​    自动登录

​    浏览记录

  上次访问时间

 网页换肤

##### 使用

 cookie.setPath(); 

```java
response.setCharacterEncoding("utf-8");
		Cookie c1 = new Cookie("lat", getDate());
        c1.setMaxAge(60*60*24);
        response.addCookie(c1);
		response.setContentType("text/html");
		PrintWriter out = response.getWriter();
		Cookie[] cookies = request.getCookies();
```

### Session机制

原理

- 应用

  session:  购物车， 防止表单重复提交

##  5.19

#### bootstrap

​     ENGINE=InnoDB   DEFAULT CHARSET=gb2312

## 5.21

form表单中用隐藏域传值。

#### 主键处理

字符串：         

- 手动生成：

  ​     I .UUID.randomUUID().toString().replace("-","");

  2. 某种固定的格式：

     ​     使用方法生成。

     

- 数据库函数： 

  ​    mysql：  replace（uuid（），”-“，""）;

     oracle:     sys_guid();

  

数字：

​     mysql：  自动增长，

​     oracle:

#### 文件上传表单和普通表单的区别

 enctype ='multipart/form-data'

##  5.22

文件上传

####  JXL 和 POI区别

## 5.23 框架

1. 框架：类 -> 组件 ->框架

功能：完成一些重要的，非核心的，重复性功能。

完成度：框架是半成品

2. MVC

   MVC：MVC是一种软件架构模式。

   - M:  PO entity  POJO BEAN+DAO  JDBC  [Hibernate+MYBATIS]
   - V:   bootstrap   js等前端框架。
   - C: Controller 控制层  servlet  【框架：Strus Spring】

###   Hibernate

 持久层框架，用save方法存储对象，需要参考映射文件。  .xml格式。

ORM技术  object  relation   mapping

> 核心API:
>
> - 1. org.hibernate.Configuration:
>
>      (1)该类是一个配置类， 解析配置文件+映射文件
>
>   2. org.hibernate.SessionFactory:
>
>   3. 

​    

----

使用hibernate

 导包：    1. hibernate3.jar               2. lib/require/所有jar包       3.mysql驱动bao

#### hibernate实体类的要求

1.必须有主见

2.必须有getter和setter

3.必须有无参构造函数

4.public class。

 ## Hibernate

缓存 ：  get      一级缓存 二级缓存

### HIbernte的查询方式

1：OID 查询（根据 ）

HQL 查询：

QBC 查询：  根据条件查询

SQL 语句



## 5.25  hibernate 设置  数据库中的 关系

###    one-to-one:

<one-to-one name="idcard" class="com.etoak.entity.Idcard"
                    cascade="all"> </one-to-one>

cascade:级联

### one-to-many

### many-to- many

## 5.28 Struts框架

 ## 介绍

开源、免费、容易搭建MVC架构的具体实现框架。约定大于配置，支持可扩展的插件体系（支持ajax和json）

### 导包

..\struts-2.3.24.1-all (1)\struts-2.3.24.1\apps\struts2-blank.war

G:\document\易途\官方课件\阶段4\插件组件工具\struts-2.3.24.1-all (1)\struts-2.3.24.1\apps

### 修改配置文件

####  web.xml

配置过滤器

#### 配置struts.xml

###   1..1.1 Struts中的Action类和Servlet中的servlet类区别

1.创建对象

2.实现方法

3.测试依赖

### 1.1.2 Struts的实现方法？

1. 默认实现
2. Implements Action   ---- com.opensympnony.xwork2.
3. extends ActionSuport  --- 1.具有Action功能，2.可以做一些服务器端校验

## 1.2.应用

### 1.2.1 配置转发和重定向

<result name="success" type="">success.jsp</result>

> type: ==>   dispatcher:         转到页面
>
> ​                   redirect:             重定向到页面
>
> ​                   chain:               转到其他action
>
> ​                   redirectAction:  重定向其他action

### 1.2.2  参数传递

- 属性驱动

```
public class HelloAction {
	public String execute() {
		System.out.println("HelloAction中的execute方法执行了");
		System.out.println(username);
		System.out.println(pwd);
		return "success";
	}

	// 属性驱动
	private String username;
	private String pwd;

	public void setUsername(String username) {
		this.username = username;
	}

	public void setPwd(String pwd) {
		this.pwd = pwd;
	}

}
```

- 模型驱动

  ```
  
  ```

  1.2.3

  

  

## 5.29 

##  struts & idea

properties

## 5.30

文件上传

###  普通上传

### 1.1.1  struts 文件上传

### 1.1.2 在struts中使用Servlet的api

 1.ServletActionContext工具类

2. ActionContext 工具类 获得4大map
3.  实现RequestAware  SessionAware  ApplicationAware 获得map
4.  是ServletRequestAware 

### 1.1.3 手动引入拦截器

## 5.31 

##  1.1.1 通配符映射

## 1。1.2 json过滤

struts-emp.xml

   ```
<param name="exculdeProperties">
  \[\d+\]\.emps
</param>
   ```

### 1.1.3 bootstrap-table使用



1.1.4  struts 导出

  导出

## 6.1 树状菜单

   bootstrap-table



##        6.4  过滤器  Filter  和   拦截器

###  6.4.1 javax.serlvet.Filter

（1）执行过滤请求，过滤相应任务的对象

（2）方法列表

​        1） Init （FilterConfig）

​         2）doFilter(request,rsponse)

##   6.4.2 servlet 和filter的 区别





### 6.4.3 拦截器



## 6.4.4

### 监听器 

javaweb中的监听器

监听容器的开启和关闭

拦截器：  方法

过滤器： 页面

监听器：监听状态的改变





## 案例：

1. 统计网站访问人数
2. 

 

##  MyBATIS

1. 和hibernate一样，也是一个ORM框架。
2. 底层也是驱动的jdbc。对数据库进行crud。
3. 半自动ORM框架，sql语句由开发者进行手写。

###  7.1 MyBatis 和 JDBC 和 Hibernate 区别





### 7.2

配置文件



CRUD

当传递多个参数时 用 map

## 7.2.1 MyBatis 中 # 和 $ 区别

  1.#  带 ‘’ 号       $直接应用



## 7.2.2

配置文件，写数据库链接



### 7.2.3

使用别名

表名和实体类不同时

1. 在数据库中 使用别名

### 7.2.4  mysql和 Hibernate 和 MyBaties 获得主键id

myBaties返回主键

```
<selectKey keyProperty="id" resultType="int"/>
```



### 7.3.1 MyBatis动态查询标签

<sql id="column_list">s_id as id ,s_name as name,s_hbirht as birth</sql>

<sql id="commonsql">select <include refid="column_list"> </include></sql>

<include refid="commonsql">   </include>

sql   ---- include 

where if   foreach

< forEach collection=“ages”  item="age" open="("  close")" seporator=",">

### 7.4.1  批量添加

框架中实体类要生成  空参 构造方法。

sesion.addbat();



## 6.6

###   1.1 mapper映射的方式增删改查

```xml
<mapper namespace ="com.etoak.student.dao.StudentMapper">  
     <insert id="addStu">
    
    </insert>
</mapper>
```

//namespace的名字必须和几口的名字一致，   

语句的id就是方法的名字，接口中的方法一定与语句的id保持一致。

新建接口

```java
public interface StudentMapper{
    public List<Student> querySome();
}
```

新建service类

```
public class StudentService{
    public List<Student> querySomeByTj(String name,String schid,Integer int){
        
    }
}

```

### 1.2 多表查询

```xml
<resultMap id="rMap_stu" type="stu">
   <id property="id" column="sid"></id>
    <result property="name" column="sid"></result>
    <result property="age" column="sname" ></result>
    <association property="schoo"  javaType="com.etoak.student." column="schoolid">
       <id property></id>
    </association>
</resultMap>
<select id="querySome" parameterType="map" resultType="rMap_stu">

</select>
```

#####  1.3.1 

## eclipse MyBatis Generator介绍   使用



##  6.7 反射

利用反射和配置文件，给对象赋值





## 6.8  枚举   enum

```
public class Annotation {
	enum Color {
		RED("紅色的", "熱情"), BLUE("藍色的", "隨和，平淡"), YELLOW("黃色的", "。。。"), GREEN(
				"綠色的", "生機與活力");
		String name;
		String info;

		private Color() {
		};

		private Color(String name, String info) {
			this.name = name;
			this.info = info;
		}

		private String getInfo() {
			return info;
		}
	}

	public static void main(String[] args) {
		System.out.println("name:" + Color.BLUE.name);
		System.out.println("info" + Color.BLUE.info);

		/*
		 * name = request.getParameter("name"); name: Color.name.name
		 */
	}

}
```

### 注解

```
@Override

@Deprecated;  方法已经过时

@SupperessWarnings;   取消警告
```

自定义注解

@interface





###    6.8  注解实现Hibernate

```
package com.etoak.entity;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.Table;

/**
 * S entity. @author MyEclipse Persistence Tools
 */
@Entity
@Table(name = "s", catalog = "et1803")
public class S implements java.io.Serializable {

	// Fields

	private String id;
	private String name;

	// Constructors

	/** default constructor */
	public S() {
	}

	/** minimal constructor */
	public S(String id) {
		this.id = id;
	}

	/** full constructor */
	public S(String id, String name) {
		this.id = id;
		this.name = name;
	}

	// Property accessors
	@Id
	@Column(name = "id", unique = true, nullable = false, length = 32)
	public String getId() {
		return this.id;
	}

	public void setId(String id) {
		this.id = id;
	}

	@Column(name = "name", length = 32)
	public String getName() {
		return this.name;
	}

	public void setName(String name) {
		this.name = name;
	}
}
```

## 总结

####  1. JDBC流程

####  2. JDBC封装

​    1.DBUtils ++ JDBC Template

####  3. 事务

​    事务概念：    特征，  场景， 隔离级别  



------

###   10:32    2018/6/11

## spring

  管理javabean，实现程序中的代码解耦， 最终提供系统扩展性和维护性。

- 作用:

    整合框架

   管理javabean

详解：

​      ioc

​       aop



--------------------------

 spring的创建对象的方法

​                   set方法

​              构造方法           工厂

spring 作用域

​           singleton

​         prototype



## 6/12

####  aop      常用于：  过滤器 ， 拦截器 ，  权限管理



aop 与  oop  ：  

注入    list ，map ，property，  

测试 对象的方法

注解  三层架构

### 6/13

 日志：  打印

使用xml配置切片

### 6/14

使用注解 配置aop 

面试题部分总结

spring整合mybatis    --     jar：ioc  +  aop  + mysql +  mybatis   17个包

-  数据库   --  创建表的三范式

创建表的3范式

 Spring+ mybatis ：配置mybatis实现增加

### 6/15

Spring

  aop  通知/增强

事务：  声明式：    事务测试

​            编程式:      编程式事务测试

### 6/19   springmvc

>  spirngmvc            c:接受参数
>
> ​            m：dao  业务  javabean   service
>
> ​            c ： 视图  jsp  html  freemark 



400   参数不匹配

spring默认不处理日期类型

### 6/20

springmvc scope：  request 和 session
请求转发和重定向  forward:/success      状态码



#### 下午

上传下载

----批量上传 --

### 6/21

- 下载，   ftp下载aa
-  json格式

springmvc 只兼容2中请求格式a

1. *.do  * .action   

    禁止使用 *.html     不兼容json格式a

2 . 斜杠   /              禁止使用/*

使用 / 请求  表示springmvc 继承restful

使用/请求  默认不加载静态资源 js、css、<img>

- 下午

  - ajax  & json    

  ​    常见  错误： 415 416  ajax没导包

- 拦截器  配置

### 6/22

             ```
知识点：  maven  quartz  dubbo springboot springcloud***

              消息队列（mq）
              sso 
              redis    mongdb
              mycat   mysql 
              jvm        线程池
              搜索引擎  es 
              docker 容器
              
              hbase
              vue.js
              node.js
              
              hsf
    
             ```

- 上午

   搭建环境

-----

- 增删改查

### 6/25

流程整理

###  6/25

maven

​     为java提供的自动化构建工具，

​        创建工程，管理jar包

基于jar包管理，基于多模块开发。

maven测试

   解压，配置环境变量，设置本地仓库

---

### 6/26

maven继承    父类   pom

​                     子类   。。

分布式集群，   dubbo   负载均衡   主机    HTTPS

集群  nginx   cxf

tomcate 热部署。。		

- mave  聚合

```
<modules>
  	<module>et-bean</module>
  </modules>
```

- 下午

 dubbo

高性能  分布式服务框架

通过rpc协议进行输入和输出，soap的治理方案。



spring整合。



http key value

soap soa xml  简单对象协议。   单一性。  客户端，  访问，服务端。

rpc   远程过程调用协议

​        nio、netty 、mina  异步  不需要考虑线程。

​    客户端 访问  服务端

远程调用

​    解决高延时 高并发。

- dubbo    du

### 6/27

quartz  （任务调度）  定时任务          指定时间执行指定的业务逻辑

组件：

1. 任务
2. 触发器

   3.调度器

----消息队列

  springboot +  spring、cloud  +  定时任务  +消息队列

### 6/28     

SpringBoot

SpringBoot整合JPA实现增删改查

----------

### 7/2   spring整合jdbc

复习jdbc
jdbcTemaplet方法：  update  

​                                  queryForObject   //结构唯一 

dataSource    daosuppor

### 7/3

Spring整合hibernate  

hibernate复习

hibernateTemplate

ht.save()  ht.delete();   ht.update();   ht.execute();

```java
ht.execute(new HibernateCallback<List<User>>() {
			@Override
			public List<User> doInHibernate(Session session) throws HibernateException, SQLException {

				Query query = session.createQuery("from User u where u.name like ?");
				query.setFirstResult(start);
				query.setMaxResults(max);
				query.setString(0, "'%" + name + "%'");
				return (List<User>) query.list();

			}
		});
```



###  7/4   Spring 整合  struts

spring 搭建步骤



Struts  步骤：



SSH

 ### 7/6

CXF