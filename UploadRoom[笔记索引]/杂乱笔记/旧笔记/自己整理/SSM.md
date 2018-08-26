# SSM

## 1 组成

```
1 SSM框架是由Spring和SpringMVC和Mybatis组成
```

## 2 流程

```
1 Tomcat开启加载web.xml配置文件
	1):spring的加载及Spring和Mybatis的整理的和处理乱码的配置都是在开启的时候加载
	2):SpringMVC的要等到页面发送请求的时候才会加载
2 加载Spring的配置和SpringMybatis整和的配置文件
	1):扫描IOC-有注解的进行实例化和依赖注入
	2):配置数据源，使用Mybatis和扫描接口，使用和配置事务
3 页面发送请求
	1):web.xml拦截请求，根据DispatcherServlet去寻找对应的SpringMVC.xml配置文件
	2):Spring.xml文件，扫描IOC注解，该实例化的实例化，该注入的注入，加载其中的(视图,上传)解析器
	3):跳转到Controller类中，Controller调用Service，Service调用Mapper，其中Mapper.xml解析接口，实现sql		语句
	4):Controller返回json数据，@ResponseBody转换成json字符串
4 页面接收数据
```

## 3 搭建

### [1] xml配置

```
1 SpringMVC和Spring都需要扫描IOC注解，因为其中的@Controller在Spring和SpringMVC中有不同的含义
2 SpringMVC和Spring的配置文件需要，分开解析
3 Spring和Spring整合Mybatis的配置文件可以放在一起解析
```

#### [1] SpringMVC配置(加上事务)

```xml
1 扫描IOC的注解 @Controller @Service @Autowired @Repository
	<context:component-scan base-package="com.etoak">
      <!--加载那个注解-->
      <context:include-filter type="annotation" 	        	                                                 		   expression="org.springframework.stereotype.Controller"/>
	</context:component-scan>
2 映射器，适配器，和 类型转换器
	<!-- 映射器 适配器 类型转换器 -->
	<mvc:annotation-driven></mvc:annotation-driven>
3 加载静态资源文件
	<!-- /请求不加载静态资源文件 -->
	<mvc:resources location="/skin/" mapping="/skin/**"/>
4 配置视图解析器
	<!-- 视图解析器 -->
	<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/pages/"></property>
		<property name="suffix" value=".jsp"></property>
	</bean>
5 配置上传解析器
	<!-- 上传解析器 -->
	<bean  id="multipartResolver"                                                                            	class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
		<property name="defaultEncoding" value="UTF-8"></property>
		<property name="maxUploadSize" value="6000000"></property>
	</bean>
```

#### [2] Spring的配置(加上事务)

```xml
1 扫描IOC的注解
	<context:component-scan base-package="com.etoak"></context:component-scan>
```

#### [3] 整合Mybatis的配置文件

```xml
1 引如properties的配置文件
	<context:property-placeholder location="classpath:properties/jdbc.properties" fiel-encoding="UTF-8">
		<!-- 不加载那个注解 -->
		<context:exclude-filter type="annotation"
		  expression="org.springframework.stereotype.Controller"/>
	</context:property-placeholder>
2 配置数据源 默认是没有事务的
	<bean id="dataSource"  class="org.springframework.jdbc.datasource.DriverManagerDataSource">
	    <!-- 驱动 -->
	 	<property name="driverClassName" value="${jdbc.driverClassName}"></property>
	 	<!-- 连接 -->		
	 	<property name="url"value="${jdbc.url}"></property>
	 	 <!-- 用户名 -->           
	 	<property name="username" value="${jdbc.username}"></property>
	 	<!-- 密码 -->           
	 	<property name="password" value="${jdbc.password}"></property>
	 </bean>
3 扫描Mybatis
	<bean id="sqlSessionFactoryBean" class="org.mybatis.spring.SqlSessionFactoryBean">
	    <!-- 注入数据源 -->
	 	<property name="dataSource" ref="dataSource"></property>
	 	<!-- 实体类起别名  -->
	 	<property name="typeAliasesPackage" value="com.etoak.modules"></property>
	 	<!-- 加载mybatis sql配置文件 -->
	 	<property name="mapperLocations">
	 		<array>
	 			<value>
	 				classpath:com/etoak/**/mapper/*.xml
	 			</value>
	 		</array>
	 	</property>
	 </bean>
4 扫描接口
	<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
	  		<!-- 
	  			引入一个sqlSessionFactoryBean对象  
	  			获取sqlsession对象
	  			sqlSessionFactoryBeanName可以省略默认查找
	  			sqlSessionFactoryBean类型
	  		-->
	  		<property name="sqlSessionFactoryBeanName" value="sqlSessionFactoryBean"></property>
	  		<!-- 
	  			扫描接口 通过代理模式创建接口对象 
	  			哪里需要 注入接口即可使用 
	  			sqlsession.getMapper(接口);
	  		-->
	  		<property name="basePackage" value="com.etoak.**.mapper"></property>
	  		<!-- 
	  			扫描接口时 接口加入注解才会被扫描，通过代理模式创建接口对象 
	  		 -->
	  		<property name="annotationClass" value="org.springframework.stereotype.Repository"></property>
	  </bean>
5 配置事务
	<!-- 配置事务管理器 -->
	 <bean id="tx" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
	 	<property name="dataSource" ref="dataSource"></property>
	 </bean>
6 使用声明式事务
	<!-- 声明式事务 transaction-manager引入事务管理器 proxy-target-class使用cglib动态代理 -->
	 <tx:annotation-driven transaction-manager="tx" proxy-target-class="true"/>
```

#### [4] web.xml

```xml
1 加载Spring和Spring整合Mybatis的配置文件，在Tomcat开启的时候加载配置文件
<listener>
  	<!-- 默认加载spring配置文件路径为 WEB-INF/applicationContext.xml -->
  	<listener-class>
  		org.springframework.web.context.ContextLoaderListener
  	</listener-class>
</listener>
  <!-- 设置全局 加载路径 -->
<context-param>
  <param-name>contextConfigLocation</param-name>
  <param-value>
  	  classpath:spring/applicationContext*.xml
  </param-value>
</context-param>

3 加载SpringMVC的配置文件
  <!-- springend  -->
  <!-- SpringmvcStart 发送请求的时候触发-->
  <servlet>
  	<servlet-name>mvc</servlet-name>
  	<servlet-class>
  		org.springframework.web.servlet.DispatcherServlet
  	</servlet-class>
  	<init-param>
  		<param-name>contextConfigLocation</param-name>
  		<param-value>
  			classpath:spring/springmvc.xml
  		</param-value>
  	</init-param>
  </servlet>
  <servlet-mapping>
  	<servlet-name>mvc</servlet-name>
  	<url-pattern>/</url-pattern>
  </servlet-mapping>
2 这是用来处理乱码的，针对post请求，也是在加载Tomcat开启的时候加载
<!--2  处理乱码 针对post 开启的时候加载-->
  <filter>
  	<filter-name>encoding</filter-name>
  	<filter-class>
  		org.springframework.web.filter.CharacterEncodingFilter
  	</filter-class>
  	<!-- 初始化参数 请求-->
  	<init-param>
  		<param-name>encoding</param-name>
  		<param-value>UTF-8</param-value>
  	</init-param>
  	<!-- 响应  -->
  	<init-param>
  		<param-name>forceEncoding</param-name>
  		<param-value>true</param-value>
  	</init-param>
  </filter>
  <filter-mapping>
  	<filter-name>encoding</filter-name>
  	<url-pattern>/*</url-pattern>
  </filter-mapping>
```

### [2] 类中的配置

```
1 设置注解
2 依赖注入
```

#### [1] Service类中的配置

```Java
1 加上 @Service实例化 @Autowired依赖注入
@Service
public class UserServiceImpl implements UserService {
	@Autowired
	private UserMapper userMapper;
	@Override
	public void add(User user){
		//设置user的ID和日期
		user.setId(UUID.randomUUID().toString());
		user.setCreateTime(new Date());
		//添加之后返回一个影响的行
		int result = userMapper.add(user);
		if(result<=0){
			throw new RuntimeException("新增用户"+user.getUsername()+"失败");
		}
	}
}
```

#### [2] Controller类中的配置

```Java
1 加上@Controller实例化
@Controller
//设置唯一 方便寻找
@RequestMapping(value="/user")
public class UserController{
	@Autowired
  	//依赖注入
	private UserService userService;
	@RequestMapping(value="/add",method=RequestMethod.POST)
  	//设置请求的类型
	@ResponseBody
  	//将Java对象转换成json字符串
	public JsonResult add(User user){
      //设置自定义的类型数据
		try {
			userService.add(user);
		}catch(RuntimeException e){
			e.printStackTrace();
			return new JsonResult(500,e.getMessage());
		}catch (Exception e) {
			e.printStackTrace();
			return new JsonResult(500,e.getMessage());
		}
		return new JsonResult(200,"新增用户:"+user.getUsername()+"成功");
      //返回自定义的数据类型
	}
}
```

#### [3] Mapper.xml中的配置

```xml 
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" 
	"http://mybatis.org/dtd/mybatis-3-mapper.dtd"> 
<!--
	连接Mapper接口
-->
<mapper namespace="com.etoak.modules.user.mapper.UserMapper">
  <!--将表中的字段封装进这个sql中-->
	<sql id="user-columns">
		id,username,password,nickname,email,
		tel,state,create_time
	</sql>
	<insert id="add" parameterType="User">
		insert into
			et_user(<include refid="user-columns"/>)
			values(#{id},#{username},#{password},#{nickname},
			#{email},#{tel},#{state},#{createTime})
	</insert>
</mapper>
```

### [3] 事务

```
1 Spring 父容器
	@Service(事务)
	除了@Controller注解 剩下的注解 全部加载
2 SpringMVC 子容器
	@Controller只加载@Controller注解
		SpringMVC集成Spring
父容器(Spring)实例化的对象存放到容器中
子容器(SpringMVC)实例化的时候会覆盖父容器的信息
父容器 Tomcat开启的时候就实例化对象 @Service @Repository
子容器 第一次请求的时候实例化对象 @Controller
```

