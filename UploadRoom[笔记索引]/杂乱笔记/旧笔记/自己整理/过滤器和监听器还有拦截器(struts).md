# 过滤器和监听器还有拦截器(struts)

## 1 过滤器

含义: 在不影响原有的业务流程的基础上，添加对请求的过滤控制

### [1] 核心api

#### javax.servlet.Filter

用处：过滤器是用来过滤请求和过滤响应任务的对象--凡是跳转jsp页面都要走一遍过滤器

##### 方法列表

```
			Filter														Servlet
1 init(FilterConfig)--初始化方法								1 init(ServletConfig)
2 doFilter(request,response,chain)--运行的方法				2 service(req,resp)
3 destory--销毁的方法										3 destory
```

```
	FilterConfig						 						ServletConfig
getInitParameter() 					 						getInitParameter()
getInitParameterNames()				 						getInitParameterNames()
getServletContext()					 						getServletContext()
getFilterName					     						getServletName()
----------------------------------------------------------------------------------------
1 FilterConfig对象是由容器创建的给filter传递一些初始化参数
2 FIlter和FilterConfig也是一对一的，一个Filter对应一个FilterConfig
```

Filter必须在web.xml中声明

### [2] Servlet和Filter的区别

1 Servlet默认在请求到达的时候构造对象，Filter在服务器启动的时候构造对象

2 Servlet实现Servlet接口，Filter实现filter接口

## 2 监听器

用处：监听到某一时间节点，我们需要执行那一段代码，

### 监听器列表

```
1 ServletContextListener---监听容器开启或者关闭的代码
2 ServletContextAttributeListener---监听Application范围属性的变化
3 ServletRequestListener---监听请求的开启和销毁
4 ServletRequestAttributeListener--监听请求中属性的变化
5 HttpSessionListener---监听session的创建和销毁
6 HttpSessionIdListener--监听sessionid
```

监听器也是在服务器启动的时候被构造

## 3 拦截器

含义：拦截器是开发人员提供了一种，在action执行前后执行的一段自定义的代码--只要是跳转到action都要过一遍拦截器

### [1] 要求

```
1 拦截器必须实现拦截接口，struts中的拦截器实现的是struts中的interceptor接口
2 Struts需要在Struts.xml中注册
3 拦截器也是在服务器启动的时候构造对象
4 当在Struts.xml中使用通配符的方式映射的时候，需要使用<paramname="excludeMethods">login,login1</param>		指定不拦截的action
5 拦截器中有两种实现方式--一种是implements Interceptor，另一种是extends MethodFilterInterceptor
	其中的实现的方法不同
```

### [2] 构造对象的时机

```
1 servlet 请求到达的时候构造对象
2 filter(过滤器)：服务器启动的时候构造对象
3 拦截器：Struts中的拦截器-服务器启动的时候构造对象
4 action：Struts中的action默认是请求到达的时候构造对象
```

### [3] struts.xml中的配置

```
<struts>
	<package name="etoak" extends="struts-default">
		<!--声明action中的拦截器-->
		<interceptors>
			<interceptor name="myinter" class="com.etoak.inter.Myinter"></interceptor>
		</interceptors>
		<!--全局结果-->
		<global-results>
			<result name="relogin">index.jsp</result>
		</global-results>
		<!--<action name="login" class="com.etoak.action.StudentAction" method="login">
			<result name="login_success" type="chain">main</result>
		</action>-->
		<action name="*" class="com.etoak.action.StudentAction" method="{1}">
			<!--引用拦截器-->
			<interceptor-ref name="myinter">
				<!--指定不拦截-->
				<param name="excludeMethods">login,login1</param>
			</interceptor-ref> 
			<!--系统拦截器-->
			<interceptor-ref name="defaultStack"></interceptor-ref>
			<result name="main_success">main.jsp</result>
			<result name="login_success" type="chain">main</result>
			<result name="login1_success">success.jsp</result>
		</action>
	</package>
</struts>
--------------------------------------------------------------------
1 拦截器必须在Struts.xml中声明，写在package的下面
2 在action中引用声明的拦截器
3 还必须引用系统的拦截器-一旦引用了自己写的拦截器系统的拦截器就会被覆盖
4 还可以在引用的拦截器中指定不拦截那个action--拦截器必须使用extends继承实现的的那个
```

## 4 过滤器和拦截器

#### [1] 区别

```
1 拦截器是基于Java反射机制的，而过滤器是基于函数回调
2 拦截器不依赖与servlet容器，过滤器依赖于servlet容器
3 拦截器只能对action请求其作用，过滤器则可以对所有的请求起作用
4 拦截器可以访问action上下文 值栈里的对象，而过滤器不能访问
5 在action生命周期中，拦截器可以多次被调用，而过滤器只能在容器初始化的时候被调用
6 拦截器可以获取IOC容器的各个bean，而过滤器不行，在拦截器中注入一个service可以调用业务逻辑
```

#### [2] 执行的顺序

```
1 过滤前 – 拦截前 – Action处理 – 拦截后 – 过滤后
```

#### [3] 在哪配置

```
1 过滤器在web.xml之中进行配置
2 拦截器在struts.xml中进行配置
```

