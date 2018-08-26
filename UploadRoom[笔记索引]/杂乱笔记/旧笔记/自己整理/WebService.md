# WebService

## 1 含义

```
1 WebService ==》是一种跨编程语言和跨操作系统的平台的远程调用技术
2 Web Service是一个平台独立的，低耦合的，自包含的、基于可编程的web的应用程序，
		可使用开放的XML（标准通用标记语言下的一个子集）
		标准来描述、发布、发现、协调和配置这些应用程序，用于开发分布式的互操作的应用程序。
3 XML+XSD,SOAP和WSDL就是构成WebService平台的三大技术。
```

## 2 用处

```
一个web service必须能合并从多个不同来源的内容，
可以包括股票，天气，新闻等，在传统环境中的内容，
如存货水平，购物订单或者目录信息等，都从后端系统而来
```

## 3 各自的含义

```
A工程的某模块(某功能实现)通过接口规范发布于internet，B工程通过简单代码直接调用A工程提供的功能实现。

A工程作为WebService的提供者和发布者 也就是Server的角色
B工程作为WebService的调用者 也就是Client的角色

Client和Server的角色定义完全取决于“功能”由谁提供和发布
```

## 4 实现的框架

```
Apache CXF = Celtix + XFire，开始叫 Apache CeltiXfire，后来更名为 Apache CXF 了
	是一套实现WebService的Java框架。
```

## 5 流程

### [1] service 提供者

```xml
1.创建一个web工程
2.开发Service和Service对应的实现类
3.在Service接口加上@WebService
4.导入Spring的jar包和cxf的jar包
5.在applicationContext.xml中引入cxf的namespace和schema
	namespace:
	xmlns:cxf="http://cxf.apache.org/jaxws"
	schema:
	http://cxf.apache.org/jaxws
	http://cxf.apache.org/schemas/jaxws.xsd"
6.配置文件当中配置
	<cxf:server serviceClass="com.etoak.service.UserService" address="/user">
		<cxf:serviceBean>
			<bean class="com.etoak.service.UserServiceImpl" />
		</cxf:serviceBean>
	</cxf:server>
	或者：
	<cxf:endpoint implementor="com.etoak.service.impl.UserServiceImpl" address="/user" />
	或者： userServiceImpl使用Service注解自动扫描直接注入bean
	<cxf:endpoint implementor="#userServiceImpl" address="/user" />
7.web.xml中配置一个servlet
	<servlet>
		<servlet-name>cxf</servlet-name>
		<servlet-class>org.apache.cxf.transport.servlet.CXFServlet</servlet-class>
	</servlet>
	<servlet-mapping>
		<servlet-name>cxf</servlet-name>
		<url-pattern>/ws/*</url-pattern>
	</servlet-mapping>
```

### [2] 服务的调用者[不使用Spring]

```Java
1.下载cxf 并且解压它
	2.设置环境变量 
		2-1:JAVA_HOME:C:\Program Files\Java\jdk1.8.0_172
		2-2:PATH:D:\apache-cxf-3.2.5\bin
	3.利用CXF 根据刚才的wsdl生成Service以及其中涉及到的相关类型(User)
		wsdl2java -d d:\src -p com.etoak.service 
			http://localhost:8080/Spring-CXF-Server/ws/user?wsdl
		
		wsdl2java ： 代表根据webservice的描述语言 生成对应的.java文件
		
		-d ： 代表自动创建到指定的目录
		d:\src ： 创建到这个指定目录
		-p ： 指定package 包结构
		com.etoak.service ： 生成的所有.java都应该在com.etoak.service包当中
			package com.etoak.service

		最重要的 把wsdl的路径给它

	4.把生成的.java 拷贝到工程当中
	5.导入cxf需要的jar包

	6.编写测试类：
		JaxWsProxyFactoryBean factory = new JaxWsProxyFactoryBean();
		factory.setAddress("http://localhost:8080/Spring-CXF-Server/ws/user");//提供者的请求路径
		factory.setServiceClass(com.etoak.service.UserService.class);//这里可以直接将包导进去
		//我们的UserService哪里来的 wsdl2java生成的
		UserService service = (UserService)factory.create();
		//我们的User哪里来的 wsdl2java生成的 cxf帮忙生成的
		User u = new User();
		u.setUsername("et1803001");
		u.setPassword("jiubugaosuni");
		boolean isOkay = service.checkUser(u);
		System.out.println(isOkay);
```

### [3] 服务的调用者[使用Spring]

```Java
1.下载cxf 并且解压它
	2.设置环境变量 
		2-1:JAVA_HOME:C:\Program Files\Java\jdk1.8.0_172
		2-2:PATH:D:\apache-cxf-3.2.5\bin
	3.利用CXF 根据刚才的wsdl生成Service以及其中涉及到的相关类型(User)
		wsdl2java -d d:\src -p com.etoak.service 
			http://localhost:8080/Spring-CXF-Server/ws/user?wsdl
		
		wsdl2java ： 代表根据webservice的描述语言 生成对应的.java文件
		
		-d ： 代表自动创建到指定的目录
		d:\src ： 创建到这个指定目录
		-p ： 指定package 包结构
		com.etoak.service ： 生成的所有.java都应该在com.etoak.service包当中
			package com.etoak.service

		最重要的 把wsdl的路径给它

	4.把生成的.java 拷贝到工程当中
	5.导入cxf需要的jar包
	6.在配置文件中配置需要
		<cxf:client id="userService" address="http://localhost:8080/Spring-CXF/ws/user"
        serviceClass="com.etoak.service.UserService" />
          id：是解析配置文件的时候所需要的ID
          address:是提供者发送请求的地址
          serviceClass:是指调用的接口
	7.编写测试类：
		//解析配置文件
		ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");
		//获取依赖注入的对象
		UserService service = (UserService)ac.getBean("userService");
		User user = new User();
		user.setUsername("etoak");
		user.setPassword("123");
		boolean isOkay = service.check(user);
		System.out.println(isOkay);
```

