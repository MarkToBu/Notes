# Springboot

## 1 Springboot的含义

```
1 Springboot是由Pivotal团队提供的全新的框架，其设计目的是用来简化新Spring应用的初始化搭建以及开发过程。
2 改框架使用了特定的方式来进行配置，从而使开发人员不在需要定义样板化的配置。
3 Springboot也称为微服务
4 在Springboot中不管工程是jar还是war，这都可以独立运行
5 Springboot必须使用maven工程，因为其需要继承一个父工程
```

## 2 Springboot的特点

```
1 创建独立的Spring应用程序
2 嵌入的Tomcat，无需部署war文件
3 简化Maven的配置
4 自动配置Spring
5 提供生产就绪型功能，如指标，健康检查和外部配置 ***
6 绝对没有代码生成和对xml没有要求配置
```

## 3 Springboot中的Tomcat

```
1 Tomcat开启的时候默认加载Springboota整合的部分框架，不需要和Spring以前开发使用大量的xml配置信息
2 Springboot默认整合Spring和Springmvc等，但是Springboot没有整合Mybatis，但是Mybatis整合了Springboot
3 Springboot建议使用properties配置文件配置属性，但是也保留了xml配置方式
```

## 4 Springboot的相关

```
https://my.oschina.net/wenbo123/blog/1834618 --线程池
https://my.oschina.net/u/2377110/blog/1835417  --dubbo

http://blog.51cto.com/wyait/1969626
https://my.oschina.net/bianxin/blog/1835680
https://my.oschina.net/hansonwang99/blog/1834393
```

## 5 Springboot的注解

```
1 @SpringBootApplication 继承了@EnableAutoConfiguration 默认整合了部分的框架Spring和SpringMVC
		默认扫描包结构为当前目录(这个注解写在那个包结构下边)和当前目录的子目录
2 @ComponentScan(basePackages={"com.etoak"}) 是用来扫描整个包结构的，用来弥补上一个注解的不足
		还有其他的方式(@ComponentScan(basePackages={"com.etoak"}))
3 @Entity 声明实体类
4 @Table(name="goods") 声明实体类对应的表
5 @Id 表示主键ID 这个注解只有主键字段才能使用
6 @GeneratedValue(strategy=GenerationType.IDENTITY) 表示主键自动增长
7 @Column(name="name") 这个设置在实体类的属性上边 对应表中的字段
8 @Query(value="update goods set name=?1,price=?2 where id=?3",nativeQuery=true)
		是用来写sql语句的，如果写的是sql语句那么后面必须跟上nativeQuery=true
9 @Primary 
		启动的时候先加载这个注解下的方法
10 @ControllerAdvice
  		@ExceptionHandler
  	这两个注解组合使用的时候表示定义一个全局的异常处理
11 @EnableAspectJAutoProxy
		用来扫描aop的注解
12 @EnableScheduling
  		扫描@Scheduled注解 开启触发器 cron
```

## 6 Springboot的流程

```
1 开启内置的Tomcat，扫描@SpringBootApplication这个注解
	默认整合部分框架 Spring Springmvc
  		默认扫描包结构为当前目录(这个注解写在那个包结构下边)和当前目录的子目录
2 客户端发送请求
	Tomcat开启的时候，是没有工程名的，可以用(/)代替吗，端口号为8080
3 Controller类接收参数
	Controller类调用Service类，Service类调用Dao层
	Dao层之中封装类sql语句
```

## 7 Springboot案例

### [1] 在同一个包下

#### [1] pom.xml的配置

```xml
  <!-- 继承springboot的父工程  简化maven配置
  		默认整合部分框架
  -->
  <parent>
  	 <groupId>org.springframework.boot</groupId>
     <artifactId>spring-boot-starter-parent</artifactId>
     <version>1.5.8.RELEASE</version>
  </parent>
  <!-- 依赖jar包管理 -->
  <dependencies>
  	<!-- boot web应用 Springmvc 默认集成Tomcat-->
  	<dependency>
	    <groupId>org.springframework.boot</groupId>
	    <artifactId>spring-boot-starter-web</artifactId>
	</dependency>
	<!-- mysql数据库 -->
	<dependency>
    	<groupId>mysql</groupId>
    	<artifactId>mysql-connector-java</artifactId>
	</dependency>
	<!-- spring-data-jpa 数据库 Hibernate-->
	<dependency>
    	<groupId>org.springframework.boot</groupId>
    	<artifactId>spring-boot-starter-data-jpa</artifactId>
	</dependency>
	<!-- mybatis -->
	<dependency>
        <groupId>org.mybatis.spring.boot</groupId>
        <artifactId>mybatis-spring-boot-starter</artifactId>
        <version>1.1.1</version>
    </dependency>
  </dependencies>
```

#### [2] 启动类

```Java
/*
 * @SpringBootApplication
 * 		@EnableAutoConfiguration == spring.factories
 * 			默认整合部分框架 Spring Springmvc
 * 			默认扫描包结构为当前目录(这个注解写在那个包结构下边)和当前目录的子目录		
 * */
@SpringBootApplication
public class Application 
	public static void main(String[] args) {
		/*第一个参数表示加载@SpringBootApplication这个注解,在application这个类中
		 *第二个参数表示开启应用，默认传入参数args变量即可
		 *默认Tomcat开启的时候 工程名为/(默认是没有工程名的) 端口号为 8080
		 *只是读取过来的了
		 * */
		SpringApplication.run(Application.class,args); 
	}
}
```

#### [3] Controller类

```Java
/*
 * Springboot默认集成了所有的xml配置
 * 开发人员不需要配置任何信息
 * */
@Controller
public class HelloController {
	/*
	 * Springboot 使用springMVC的时候返回json类型可以使用String字符串
	 * SpringMVC 返回json类型的时候不要使用String字符串类型防止报错
	 * */
	@RequestMapping(value="/hello")
	@ResponseBody
	public String hello(){
		return "你好";
	}
}
```

### [2] 在不同的包下

#### [1] pom.xml

和上面相同

#### [2] 启动类

```java
/*
 * @SpringBootApplication
 * 		@EnableAutoConfiguration == spring.factories
 * 			默认整合部分框架 Spring Springmvc
 * 			默认扫描包结构为当前目录(这个注解写在那个包结构下边)和当前目录的子目录
 * 
 * @ComponentScan(basePackages={"com.etoak"})
 * @SpringBootApplication(scanBasePackages="com.etoak")
 * 		扫描com.etoak包结构-两种方式
 */
@SpringBootApplication
@ComponentScan(basePackages={"com.etoak"})
public class Application {
	public static void main(String[] args) {
		/*第一个参数表示加载@SpringBootApplication这个注解,在application这个类中
		 *第二个参数表示开启应用，默认传入参数args变量即可
		 *默认Tomcat开启的时候 工程名为/(默认是没有工程名的) 端口号为 8080
		 *只是读取过来的了
		 *默认加载 properties配置文件 名称为application.properties
		 * */
		SpringApplication.run(Application.class,args); 
	}
}

```

#### [3] Controller类

```Java
/*@Controller*/
//Springmvc中的注解  等价于 @Controller 和 @ResponseBody 实例化对象 返回客户端 所有的方法返回响应json的格式
//@PropertySource 引入properties文件
@PropertySource(value={"classpath:et.properties"},encoding="UTF-8")
@RestController
public class LoginController{
	//@Value获取配置信息
	@Value("${etoak}")
	private String etoak;
	@Value("${et}")
	private String et;
	//获取et1803的时候，如果默认值为空 那么设置默认值为1803
	@Value("${et1803:1803}")
	private String et1803;
	/*@RequestMapping(value="/login")
	@ResponseBody*/
	/*Springmvc中也能用 等价于@RequestMapping(value="/login",method=RequestMethod.GET)
	 * @PostMapping
	 * @PutMapping
	 * @DeleteMapping
	 * */
	@GetMapping(value="/login/{pathVar}")
	/*@PathVariable Springmvc中的注解
	 * */
	public String login(@PathVariable(value="pathVar") String pathVar){
		System.out.println("接收restful参数："+pathVar);
		return "登录";
	}
	@GetMapping(value="/props")
	public String props(){
		return etoak + "|" + et + "|" + et1803;
	}
}

```

### [3] 连接数据库

#### [1] pom.xml

和上边相同

#### [2] 启动类 

```Java
/*
 * @SpringBootApplication
 * 		@EnableAutoConfiguration == spring.factories
 * 			默认整合部分框架 Spring Springmvc
 * 			默认扫描包结构为当前目录(这个注解写在那个包结构下边)和当前目录的子目录
 * @ComponentScan(basePackages={"com.etoak"})
 * @SpringBootApplication(scanBasePackages="com.etoak")
 * 		扫描com.etoak包结构-两种方式
 * ------------------------------------------------------
 * @EnableJpaRepositories(basePackages="com.etoak.dao")
 * 		开启JPa操作 并且扫描包结构下的接口对象
 * 		该接口对应必须继承JpaRepository
 * 		将该接口对应实例化 接口创建对象 通过动态代理
 * 
 * @EntityScan(basePackages="com.etoak.bean")
 * 		扫描实体类中的注解并且和数据库表中的字段进行映射--对应	
 * */
@SpringBootApplication
@ComponentScan(basePackages={"com.etoak"})
@EntityScan(basePackages="com.etoak.bean")
@EnableJpaRepositories(basePackages="com.etoak.dao")
public class Application {
	public static void main(String[] args) {
		/*第一个参数表示加载@SpringBootApplication这个注解,在application这个类中
		 *第二个参数表示开启应用，默认传入参数args变量即可
		 *默认Tomcat开启的时候 工程名为/(默认是没有工程名的) 端口号为 8080
		 *只是读取过来的了
		 *默认加载 properties配置文件 名称为application.properties
		 * */
		SpringApplication.run(Application.class,args); 
	}
}
```

#### [3] 实体类

```Java
@Entity//实体
@Table(name="goods")//实体类对应的表
public class Goods {
	@Id//表示主键ID 这个注解只有主键字段才能使用
	//表示主键自动增长
	@GeneratedValue(strategy=GenerationType.IDENTITY)
	private Integer id;
	//属性对应表中的字段
	@Column(name="name")
	private String name;
	@Column(name="price")
	private Double price;
	//生成getter和Setter方法	
}
```

#### [4] Dao层

```Java
/*JpaRepository<Goods,Integer>
 * 	第一个泛型表示 实体类对象和数据库表一一对应
 * 	第二个泛型表示 实体类属性对应数据库表中的主键类型
 * 		数据库表的主键类型为int Java中的实体类属性对应为Integer
 * */
public interface JpaDao extends JpaRepository<Goods,Integer>{
	//定义sql语句和hql语句
	//如果写的是sql语句那么nativeQuery设置为true
	@Modifying
	@Query(value="update goods set name=?1,price=?2 where id=?3",nativeQuery=true)
	public int update(Goods goods);
	@Query(value="select id,name,price from goods",nativeQuery=true)
	public List<Goods> getAll();
}
```

#### [5] Service层

```Java
@Service
public class GoodsService {
	@Autowired
	private JpaDao dao;
	//添加商品
	public Goods add(Goods goods){
		//JPa提供了save方法 JpaDao继承了JpaReposito
		Goods g = dao.save(goods);
		return g;
	}
	@Transactional
	public void delete(Integer id){
		dao.delete(id);
	}
	@Transactional
	public int update(Goods goods){
		return dao.update(goods);
	}
	@Transactional
	public List<Goods> getAll(){
		return dao.getAll();
	}
}
```

#### [6] Controller层

```java 
@Controller
public class GoodsCotroller {
	@Autowired
	private GoodsService service;
	@RequestMapping(value="/add")
	@ResponseBody
	public Goods add(Goods goods){
		Goods g = service.add(goods);
		return g;
	}
	@RequestMapping(value="/getAll")
	@ResponseBody
	public List<Goods> getAll(){
		return service.getAll();
	}
	@RequestMapping(value="/update")
	@ResponseBody
	public String update(Goods goods){
		int result = service.update(goods);
		return "修改成功";
	}
	@RequestMapping(value="/delete")
	@ResponseBody
	public String delete(Integer id){
		service.delete(id);
		return "删除成功";
	}
}
```

### [4] 连接数据库的(没有xml)

#### [1] pom.xml的配置

```
1 要继承springboot的父工程
2 下载数据库和和Mybatis的jar包还有web的jar
3 但是springboot不支持jsp页面，所以要单读下载jar包 
<parent>
  	 <groupId>org.springframework.boot</groupId>
     <artifactId>spring-boot-starter-parent</artifactId>
     <version>1.5.8.RELEASE</version>
  </parent>
  <dependencies>
  	<!-- boot web应用 Springmvc 默认集成Tomcat-->
  	<dependency>
  		<groupId>org.springframework.boot</groupId>
	    <artifactId>spring-boot-starter-web</artifactId>
  	</dependency>
  	<!-- mysql数据库 -->
	<dependency>
    	<groupId>mysql</groupId>
    	<artifactId>mysql-connector-java</artifactId>
	</dependency>
	<!-- mybatis -->
	<dependency>
        <groupId>org.mybatis.spring.boot</groupId>
        <artifactId>mybatis-spring-boot-starter</artifactId>
        <version>1.1.1</version>
    </dependency>
    <!-- springboot支持 jsp -->
		<dependency>
			<groupId>org.apache.tomcat.embed</groupId>
			<artifactId>tomcat-embed-jasper</artifactId>
			<scope>provided</scope>
		</dependency>
  </dependencies>
```

#### [2] 用Java代替xml

##### [1] 配置数据源

``` Java
1 配置数据源，通过解析jdbc.properties文件来加载数据
2 定义前置信息 只能读取后缀名是properties
3 设置setter方法，来接收解析出来的值
//加载properties文件 并解析信息 编码为 utf-8
@PropertySource(value="classpath:jdbc.properties",encoding="UTF-8")
// 定义properties的配置文件的前置信息 并加载执行 只能读取后缀名是properties
@ConfigurationProperties(prefix="et.datasource")
@Configuration //<beans>
public class DataSourceConfig01 {
	private String driverClassName;
	private String url;
	private String username;
	private String password;
	public String getDriverClassName() {
		return driverClassName;
	}
	public void setDriverClassName(String driverClassName) {
		this.driverClassName = driverClassName;
	}
	public String getUrl() {
		return url;
	}
	public void setUrl(String url) {
		this.url = url;
	}
	public String getUsername() {
		return username;
	}
	public void setUsername(String username) {
		this.username = username;
	}
	public String getPassword() {
		return password;
	}
	public void setPassword(String password) {
		this.password = password;
	}
	//1 配置数据源 默认没有事务
	//springboot  启动的时候先加载这个注解下的方法 唯一
	@Primary
	@Bean(value="dataSource")
	public DriverManagerDataSource getDataSource(){
		DriverManagerDataSource dataSource = new DriverManagerDataSource();
		dataSource.setDriverClassName(driverClassName);
		dataSource.setUrl(url);
		dataSource.setUsername(username);
		dataSource.setPassword(password);
		return dataSource;
	}
}
```

##### [2] 配置Mybatis

```Java
1 引入数据源-两种方式
	1):设置成员变量
	2):设置成局部变量 - 需要用到注解@Qualifier("dataSource") 这是根据别名来查找
					如果删除注解的话就是根据类型来查找
2 还可用来配置事务 - 
@Configuration
//5 开启声明式事务  等价于<tx:annotation-driven
//  默认jdk动态
//  proxyTargetClass=true 设置cglib动态代理 
@EnableTransactionManagement(proxyTargetClass=true)
public class SqlSessionFactoryConfig02 {
	/*两种方式
	 * 	使用成员变量
	 * 	@Autowired
	 *	private DataSource dataSource;
	 *	使用局部变量 
	 *	1 @Qualifier("dataSource") DataSource dataSource 根据别名来查找
	 *	2   去掉@Qualifier 根据类型来查找
	 * */
	@Autowired
	private DataSource dataSource;
	//使用Mybatis 创建SqlSessionFactoryBean对象 加载信息
	@Bean(value="sqlSessionFactory")
	public SqlSessionFactoryBean getSqlSessionFactoryBean(){
		SqlSessionFactoryBean ssfb = new SqlSessionFactoryBean();
		//注入数据源
		ssfb.setDataSource(dataSource);
		//起别名
		ssfb.setTypeAliasesPackage("com.etoak.bean");
		//获取路径资源解析器对象
		PathMatchingResourcePatternResolver resource = new PathMatchingResourcePatternResolver();
		//获取Mybatis sql配置文件
		Resource[] mapperLocation = null;
		try {
			mapperLocation = resource.getResources("classpath:com/etoak/mapper/*.xml");
		} catch (Exception e) {
			// TODO: handle exception
			e.printStackTrace();
		}
		//加载Mybatis sql配置文件
		ssfb.setMapperLocations(mapperLocation);
		return ssfb;
	}
	//4 配置事务管理器
	@Bean(name="tx")
	public DataSourceTransactionManager tx(){	
		return new DataSourceTransactionManager(dataSource);
	}	
}
```

##### [3] 扫描接口

```Java
@Configuration
//防止并发，防止发生在加载这个类之前，SqlSessionFactoryConfig02这个类没有被实例化这种情况
@AutoConfigureAfter(SqlSessionFactoryConfig02.class)
public class MapperScannerConfig03 {
	/*扫描接口
	 * 	
	 * */
	@Bean
	public MapperScannerConfigurer getMapperScannerConfigurer(){
		MapperScannerConfigurer msc = new MapperScannerConfigurer();
		msc.setSqlSessionFactoryBeanName("sqlSessionFactory");
		msc.setBasePackage("com.etoak.mapper");
      	//
		msc.setAnnotationClass(Repository.class);
		return msc;
	}
}
```

##### [4] 配置拦截器

```java 
@Configuration
public class MvcAdapterConfig  extends WebMvcConfigurerAdapter{
	//引入拦截器的方法
  	private AuthInterceptor authInterceptor;
	//配置拦截器
	@Override
	public void addInterceptors(InterceptorRegistry registry){
		//addInterceptor 添加拦截器
		//addPathPatterns 需要拦截的请求地址
		//一旦需要拦截触发addInterceptor方法拦截器
		//excludePathPatterns 不需要拦截的请求地址
		//不需要拦截 不会触发addInterceptor方法拦截器
		registry.addInterceptor(authInterceptor).addPathPatterns("/**")
		.addPathPatterns("/login/login").excludePathPatterns("/");
	}
}
```

#### [3]设置全局异常处理

```java
/*@ControllerAdvice
 * 		@ExceptionHandler
 * 	这两个注解组合使用的时候表示定义一个全局的异常处理
 * 
 * 	Controller层 Service层  和Dao层不管那一层出现异常
 * 	首先触发的是@ExceptionHandle这个注解
 * */
@ControllerAdvice
public class ExController {
	@ExceptionHandler
	public String ex(Exception ex,Model m){
		m.addAttribute("msg",ex.getMessage());
		return "/error";
	}
}
```

#### [5] 设置拦截器执行的方法

```Java
//拦截器
@Component
public class AuthInterceptor implements HandlerInterceptor{
	@Override
	public void afterCompletion(HttpServletRequest arg0,
			HttpServletResponse arg1, Object arg2, Exception arg3)
			throws Exception {	
	}
	@Override
	public void postHandle(HttpServletRequest arg0, HttpServletResponse arg1,
			Object arg2, ModelAndView arg3) throws Exception {
	}
  	// 进入拦截器先执行的这个方法
	@Override
	public boolean preHandle(HttpServletRequest arg0, HttpServletResponse arg1,
			Object arg2) throws Exception {
		System.out.println("执行拦截器");
		return true;
	}	
}
```

#### [6]