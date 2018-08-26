# SSH

## 1 含义

```
1 SSH就是Spring和Struts和Hibernate结合的架子
```

## 2 配置[非注解版]

### [1] web.xml

```xml
1 配置拦截器
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
2 配置监听器
<listener>
	<listener-class>
		org.springframework.web.context.ContextLoaderListener
	</listener-class>
</listener>
```

### [2] applictionContext.xml

```xml 
1 引入properties文件	
	<context:property-placeholder location="classpath:dbcp.properties"/>
2 配置数据源    
    <bean id="ds" class="org.apache.commons.dbcp.BasicDataSource"
    p:driverClassName="${jdbc.driverClassName}"
    p:url="${jdbc.url}"
    p:username="${jdbc.username}"
    p:password="${jdbc.password}"/>
3 配置SessionFactoryBean	  
      <bean id="sf" class="org.springframework.orm.hibernate4.LocalSessionFactoryBean">
            <property name="dataSource" ref="ds"></property>
            <property name="mappingResources">
               <list>
                <value>com/etoak/bean/Demo.hbm.xml</value>
               </list>
            </property>
            <property name="hibernateProperties">
               <props>
                  <prop key="hibernate.show_sql">true</prop>
                  <prop key="hibernate.format_sql">true</prop>
                  <prop key="hibernate.dialect">
                          org.hibernate.dialect.MySQLDialect
                  </prop>
               </props>
            </property>
      </bean>
4 注入SessionFactoryBean
	  <bean id="demoDao" class="com.etoak.dao.DemoDao"
	  p:sf-ref="sf" />
5 在service中注入dao
	  <bean id="demoService" class="com.etoak.service.DemoService"
	  p:demoDao-ref="demoDao"/>
6 在action中注入service  
	  <bean id="demoAction" class="com.etoak.action.DemoAction" 
	  p:demoService-ref="demoService" scope="prototype"/>
```

### [3] Struts.xml

```xml
<struts>
	<package name="demo" extends="struts-default">
		<action name="demo" class="demoAction">
			<result name="success">congratulations.jsp</result>
		</action>
	</package>
</struts>
```

