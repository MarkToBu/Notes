# Spring-Jdbc

## 1 含义

```
1 Spring封装JDBC其实就是使用一个强大的傻瓜式的超级工具类JDBCTemplate代替传统的JDBC的开发流程对数据库进行操作
```

## 2 方式

```
1  使用Spring 我么可以在Dao层利用IOC容器注入一个JDBCTemplate对象，这么做有两种方式
		1):IOC容器中配置一个dataSource将DataSource注入给jdbcTemplate
				然后让每一个dao当中都有一个jdbcTemplate
		2):让每个dao层都继承JDBCDaoSupport
				ioc容器中配置dataSource 将dataSource注入给每个dao对象
				[其父类有dataSource属性 使用getJdbcTemplate()得到JdbcTemplate]
```

### [1] 配置DataSource的方式

```
1 使用Spring自带的连接方式
		org.springframework.jdbc.datasource.DriverManagerDataSource 
		driverClassName  URL   username   password
2 使用DBCP连接池的方式
		org.apache.commens.dbcp.BasicDataSource
		driverClassName  url  username  password
3 使用C3P0
		com.mchange.v2.c3p0.ComboPooledDataSource
		driverClass  jdbcUrl  user  password
4 使用JDNI
		org.springframework.jndi.JndiObjectFactoryBean
		jndiName	-依赖web容器
注：不同的方式，连接的前置也会不同
		driverClassName和driverClass
		url和jdbcURL
		username和user
-----------------------------------配置dataSource可以引入properties--------------------------------
<context:property-placeholder location="classpath:db.properties" />
		自然的要使用context命名空间 创建db.properties文件
		然后使用${key取值}   注意username…，因为不同的连接方式会使用不同的前置
----------------------------------使用DBCPU的连接方式-----------------------------------------------
```

### [2] 将dataSource引入JDBCTemplate的方式

```
1 可以直接使用ref的方式
	<bean id ="jt" class="org.springframework.jdbc.core.JdbcTemplate">
       	<property name="dataSource" ref="ds"></property>
    </bean>
2 另一种是可以使用p命名空间 将property作为xml的属性完成赋值
	在头信息之中，加入：xmlns:p=http://www.springframework.org/schema/p
	之后：<bean id="dao" class="com.etoak.dao.impl.FakeDaoImpl" p:jt-ref="jt" />
```

## 3 jdbcTemplate对数据库进行CURD

```
1 增删改使用Update这个方法
	增加：int count = tmp.update("insert into user values(null,?,?)","JayZhou","111111");
	删除：int x = tmp.update("delete from user where id=?",2);
	修改：int count tmp.update("update user set username=?,password=? where id=?",)
2 查询使用特殊的方法
	1):查询表中的数据总数--queryForObject(sql语句,类型.class,可变参数);
			*:必须保证返回的是唯一结果!
			 Integer count = tmp.queryForObject("select count(id) from user", Integer.class);
			 System.out.println(count);
			 还可以用来查询id和唯一的名字-只需要更改sql语句和后边的类型
	2):根据ID查询一个具体的字段的内容--queryForMap
			*:必须保证返回的是唯一结果!
			Map<String,Object> map = tmp.queryForMap("select * from user where id=?",8);
            Set<String> ks = map.keySet();
            for(String k : ks){
                System.out.println(k + ":" + map.get(k));
            }
    3):带有复杂条件的分页查询--queryForList或者是query
    		*:能够查询多行结果
    		(1)-queryForList
    		List<Map<String,Object>> list = tmp.queryForList("select * from user");
            for(Map<String,Object> map : list){
                for(Map.Entry<String,Object> e : map.entrySet()){
                    String k = e.getKey();
                    Object v = e.getValue();
                    System.out.println(k + " : " + v);
                }
            }
            (2)-query
            List<User> list = tmp.query("select * from user", new RowMapper<User>(){
			@Override
			public User mapRow(ResultSet rs, int xxxxx) throws SQLException {
				User u = new User();
				u.setId(rs.getInt("id"));
				u.setUsername(rs.getString("username"));
				u.setPassword(rs.getString("password"));
				return u;
			}
			});
            for(User u : list){
                System.out.println(u.getId() + " : " + u.getUsername() + " : " + u.getPassword());
            }
```

## 4 流程

```
1.导入spring整合JDBC所需要的jar包
	a> 导入Spring-ioc需要的jar包 
	b> 导入Spring-jdbc需要的jar包
	c> 导入数据库驱动
	d> 导入你选择的dataSource所需要的jar包 [dbcp or c3p0]
2.创建一个spring的配置文件 ac.xml
	当中要完成至少三个bean的配置
	a> dataSource
	b> JdbcTemplate	需要将dataSource注入进来
	c> 我们自己的dao
3.使用JdbcTemplate 完成数据的增删改查
	增删改 = update(String sql,Object ... args);
		返回的是受影响的行数
	查记录数: queryForObject(sql语句,返回类型,可变参数);   //结果必须是唯一的 否则异常
	查实体对象: queryForMap()
		*：其中Map的主键是数据库当中的字段名
			Map的值是数据库当中的数据
	查带条件的列表信息: queryForList() or query() ;
		*：queryForList()返回的是List<Map<String,Object>>
			其中每个Map对应数据库当中一行数据
				其主键 是 数据库当中的字段名
				其值 是数据
		*：query(sql语句,RowMapper对象);
			RowMapper用于将一行(Row)数据映射(Mapper)成一个实体对象
				抽象方法mapRow(ResultSet,int)
				就是要通过rs当中得到数据 并且手动的设置给要返回的实体对象
```

## 5 查询语句

```java 
前提 注入JdbcTemplate--通过setter注入的方式
		private JdbcTemplate jt;
        public void setJt(JdbcTemplate jt){
            this.jt = jt;
        }
------------------------------------------------------------------------------------------------
1 插入一条语句
	String sql ="insert into 表名 values(null,name.pass)";
	int count = jt.update(sql,u.getName(),u.getPass());
2 删除一条语句
	String sql ="delete from user where id=?";
	int count = jt.update(sql,id);
3 修改一条语句
	String sql ="update from user set name=?,pass=? where id=?";
	int count = jt.update(sql,u.getName(),u.getPass(),u.getId());
4 查询总数
	String sql ="select count(*) from user";
	int count = jt.queryForObject(sql,Integer.class);
5 查询一个具体的
	String sql = "select * from user where id=?";
	User u = jt.queryObject(sql,UserRowMapper.getInstance(),id);
6 查询所有的
	String sql ="select * from user";
	List<User> user = jt.query(sql,UsrRowMapper.getInstance());
----------------------------------------------------------------------------------------------
  private class UserRowMapper extends RowMapper<User>{
    @Override
    public User mapRow(Result rs,Index xxx)throws SQLException(){
      User u = new User();
      u.setId(rs.getInteger("id"));
      u.setName(rs.getString("name"));
      u.setPass(rs.getString("pass"));
      result u;
    }
  }
```

