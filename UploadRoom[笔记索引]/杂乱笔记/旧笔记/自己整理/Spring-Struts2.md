# Spring-Struts2

## 1 含义

```
1 Spring整合Struts，就是把Struts中的所有的action配置到Spring的IOC容器之中，然后Struts配置文件当中的action的Class直接写bean的ID
```

## 2 配置

### [1] web.xml

#### [1] Spring

```xml
1 配置监听器-listener--ContextLoaderListener
	<listener>
  	<listener-class>
  		org.springframework.web.context.ContextLoaderListener
  	</listener-class>
  </listener>
2 还需要配置Spring的配置文件的所在位置
	如果不配置的话，就默认找WEB-INF下的applicationContext.xml文件
```

#### [2] Struts

```xml
1 配置Filter--StrutsPrepareAndExecuteFilter
	<filter>
  	<filter-name>ss</filter-name>
  	<filter-class>
  		org.apache.struts2.dispatcher.filter.StrutsPrepareAndExecuteFilter
  	</filter-class>
  </filter>
  <filter-mapping>
  	<filter-name>ss</filter-name>
  	<url-pattern>/*</url-pattern>
  </filter-mapping>
```

### [2] Struts.xml

```xml
<struts >
	<package name="etoak" extends="struts-default">
		<action name="user" class="userAction">
			<result name="success">ok.jsp</result>
		</action>
	</package>
</struts>
```

### [3] applicationContext.xml

```xml 
1 这是xml配置版的
		<bean id="userAction" class="com.etoak.action.UserAction" scope="prototype" />
		*:因为action对象当中有表单项数据 那是一堆成员变量 所以action不能是singleton的
2 如果是使用注解版的--这个bean就不需要配置了
```

