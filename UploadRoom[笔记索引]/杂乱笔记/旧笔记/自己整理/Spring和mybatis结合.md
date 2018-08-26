# Spring和mybatis结合

## 1 xml文件配置

```xml
1 扫描IOC注解，先扫描所有的类中的IOC注解
	<context:component-scan base-package="com.etoak" />
2 配置数据源
	<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
	    <!-- 驱动 -->
		<property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
		<!-- 连接 -->
		<property name="url" value="jdbc:mysql://localhost:3306/1803"></property>
	    <!-- 用户名 -->
	    <property name="username" value="root"></property>
	    <!-- 密码 -->
	    <property name="password"  value="mjylst"></property>            
	</bean>
3 使用持久层框架
	<bean id="sqlSessionFactory"  class="org.mybatis.spring.SqlSessionFactoryBean">
	    <!-- 注入数据源，将上边的数据源注入进来 -->
		<property name="dataSource" ref="dataSource"></property>
		<!-- 
			扫描包结构 将实体类在mybatis中封装成别名
			别名的规则为：com.etoak.bean.User
			            去除包结构原名输出：User，最好使用大写的
			            去除包结构类名所有单词全部小写  user
			    注意事项： 定义包结构时不能使用通配符*  定义路径写绝对路径
		 -->
		<property name="typeAliasesPackage" value="com.etoak.bean"></property>              
		<!-- 
			加载mybati sql.xml配置文件
			classpath当前工程src下路径
			    java porject ：bin
			    web project  ：tomcat/工程名/classes 
			classpath*当前工程jar路径也加载
		 -->
		<property name="mapperLocations">
			<array>
				<value>
					classpath:com/etoak/mapper/*.xml
				</value>
			</array>		
		</property>
	</bean>
---------------------------------------------------------------------------------------------------
1 上边这两段代码相当于mybatis中的
		1.解析主配置文件
            settings属性
		    数据源 连接数据库
            实体类起别名
            映射sql.xml配置
            插件
         2.SqlSessionFactory对象
---------------------------------------------------------------------------------------------------
4 扫描接口
	<!-- 
		3.扫描接口 
		getBean("sqlSessionFactory");
		获取SqlSessionFactory对象
		下面这段代码相当于，在service中
		SqlSession session = SqlSessionFactory.openSession();
		接口 别名 = （接口）session.getMapper(接口);	
	-->
	<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
		<!-- 注入SqlSessionFactory对象 -->
		<property name="sqlSessionFactoryBeanName"  value="sqlSessionFactory"></property>
		<!-- 扫描接口实例化，创建接口对象  可以使用通配符*-->
		<property name="basePackage"  value="com.etoak.mapper"></property>
	</bean>
---------------------------------------------------------------------------------------------------
		3.通过SqlSessionFactory对象获取SqlSession
        4.SqlSession开启事务
        5.持久化操作SqlSession
            ibatis
              selectOne("id","param")
              selectList（“id”，“param”）
            mybatis
                接口 别名(接口)getMapper(接口.class)
                     别名.接口方法名称（）；
        6.SqlSession提交或回滚
        7.关闭SqlSession
```

## 2 Service中的配置

```Java
1 要先实例化Service，通过注解的方式
2 将mapper接口注入进来
3 重写mapper中的方法
@Service
public class UserService {
	//注入mapper接口
	@Autowired
	private UserMapper userMapper;
	public int add(User user) {
		//执行mybatis接口方法 ，表示调用的就是sql语句
		int result = userMapper.add(user);
		return result;
    }
}
```

## 3 测试类的配置

```Java
@RunWith(SpringJUnit4ClassRunner.class)
将Spring和junit结合起来
@ContextConfiguration(locations="classpath:applicationContext.xml")
通过注解的方式解析applicationContext.xml文件，实例化
public class SpringTest {
	@Autowired
	注入service 
	private UserService service;
	@Test
	public void test(){
		if (service == null) {
			System.out.println("null");
			return;
		}
		User user = new User();
		user.setId(UUID.randomUUID().toString());
		user.setUsername("李英海");
		user.setPassword("12345");
		user.setNickname("断戟之犬");
		user.setEmail("6666@qq.com");
		user.setTel("110");
		user.setCreateTime(new Date());
		int result = service.add(user);
		System.out.println("返回插入的行数："+result);
	}
}
```

