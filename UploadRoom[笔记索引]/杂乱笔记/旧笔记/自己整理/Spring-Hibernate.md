# Spring-Hibernate

## 1 含义

```
1 Spring整合Hibernate其实就是将原本的Hibernate的SessionFactory交由Spring的IOC容器，然后利用Spring提供的HibernateTemplate完成增删改查的操作
```

## 2 流程

### [1] 配置xml格式的

```
1.导入Jar包
2.完成bean和hbm.xml
3.spring的xml文件
	a> 同样需要配置一个dataSource = 数据的源头
	b> 配置sessionFactory -> 
		org.springframework.orm.hibernateX.LocalSessionFactoryBean
		将dataSource注入给它
		给它注入所有的mapping文件
			mappingResources   list value
		给它注入Hibernate的配置信息
			hibernateProperties props prop key
	c> 根据版本区别 如果大家使用的是Hibernate3或者Hibernate5
		那么接下来可以使用HibernateTemplate
		[Hibernate4没有]
		配置一个HibernateTemplate对象 
		并且将它需要的sessionFactory注入
	d> 将HibernateTemplate也就是ht注入给我们的DAO
		或者....!!!!!
		让DAO继承HibernateDaoSupport 
		然后注入DataSource之后 
		直接getHibernateTemplate()
--------------------------------------------------------------------------------------------------
2nd.配置LocalSessionFactoryBean				
		2-1 注入DataSource							
		2-2 注入mappingResources					
              <list>	
                  <value>com/etoak/bean/User.hbm.xml></value>			   
              </list>												 
		2-3 注入hibernateProperties					
		<props>										
			<prop key=”dialect”> 					
				org.hibernate.dialect.MySQLDialect			
			</prop>								
			<prop key=”show_sql”>true</prop>
			<prop key=”format_sql”>true</prop>
		</props>
```

### [2] 使用注解格式的

```
1.导入Jar包
2.完成bean
3.spring的xml文件
	a> 同样需要配置一个dataSource = 数据的源头
	b> 配置sessionFactory -> Hibernate 4 和 5
		org.springframework.orm.hibernateX.LocalSessionFactoryBean
		将dataSource注入给它
		给它注入实体类文件-
			也是用list和value
		给它注入Hibernate的配置信息
			hibernateProperties props prop key
4 扫描注解
5 但是注解的使用Hibernate3和4,5有很多的不同
	注解版本【3和4、5不同】版本3需要的配置
		配置AnnotationSessionFactoryBean
				2-1 注入DataSource
				2-2 注入annotatedClasses
                    <list>
                        <value>com.etoak.bean.User</user>
                    </list>
				2-3 注入packagesToScan
				2-4 注入hibernateProperties
				【2-2和2-3功能相同 只需要一个】
			 	hibernate3：AnnotationSessionFactoryBean
				hibernate4：LocalSessionFactoryBean
				hibernate5：LocalSessionFactoryBean
```

## 3 流程的其他的特别配置

```
1 *:2-2.也可以直接将hibernate.cfg.xml注入给LocalSessionFactoryBean 
		<property name=” configLocation” value=”classpath:hibernate.cfg.xml”>
		这将直接使用hibernate本身的config文件 
		在这个Hibernate.cfg.xml中配置dataSource数据源
2 配置HibernateTemplate 并且将sessionFactory注入其中- 这是hibernate3和5的配置
	Hibernate4 需要直接将sessionFactory注入dao
	因为没有HibernateTemplate   (Hibernate5需要Spring4.2以上)
	*:版本是个很恶心的东西~
```

## 4 对数据库进行CURD(xml格式)

```Java
1 前提是注入一个HibernateTemplate--利用setter注入的方式
		private HibernateTemplate ht;
        public void setHt(HibernateTemplate ht){
            this.ht = ht;
        }
--------------------------------------------------------------------------------------------------
1 插入一条语句
	 	Serializable ID  = ht.save(u);----传入一个对象
	 	return ID!=null;--返回boolean类型
2 删除一条语句
		//HibernateTemplate的delete方法要求传入一个对象
		//但是这个对象只有ta的ID有意义 只利用id形成删除条件
		User u = new User();
		u.setId(id);--将传入的id set进对象中
		ht.delete(u);
		return true;
3 修改一条语句
		ht.update(u);--直接传入的是对象
		return true;
4 查询一条具体的数据-使用get()方法
		ht.get(实体类.class,id)-根据传入的参数
5 查询数据库中有多少条--两种方式
		1):直接获取的多少条
		//利用list 实现带有模糊查询条件的查询条数
		String hql = "from User u where u.username like ?";
		List<Object> list = ht.find(hql,"%"+username+"%");
		return list.size();
		2):查询数据库中有多少条
		String hql = "select count(*) from User u where u.username like ?";
		List<Object> list = ht.find(hql,"%"+username+"%");
		//list当中只有一个元素 元素的内容就是条数
		Long count = (Long)(list.get(0));
		return (int)(count.longValue());
6 查询多条语句
		/*
		//findByExample支持分页的参数 但是 没法实现模糊查询
		ht.findByExample(exampleEntity, firstResult, maxResults)
		//find 可以拼装成模糊查询 但是这个分页 它支持一半
		*/
		return ht.execute(new HibernateCallback<List<User>>(){
			@Override
			public List<User> doInHibernate(Session session){
				String hql = "from User u where u.username like '%"+username+"%'";
				Query q = session.createQuery(hql);
				q.setFirstResult(start);
				q.setMaxResults(max);
				return q.list();
			}
		});
```

## 5 HibernateTemplate的方法

```
	save() : 传入实体对象 返回生成的id
	delete()：传入实体对象 但是通过其中id形成删除的where条件
	update()：传入实体对象 id作为where条件
	-----------查询-----------
	get()：传入类对象和id 立即加载
	load()：传入类对象和id 延迟加载
	find()：传入hql 返回list
	findByExample() : 传入hql返回list
	execute()：传入HibernateCallback()
```

## 6 对数据库进行CURD(注解格式)

```java
1 前提是注入一个SessionFactory ---通过注解注入
		@Autowired
		private SessionFactory sf;
--------------------------------------------------------------------------------------------------
1 插入一条语句--save()方法
	 	Session ss = sf.openSession();
		User user = new User();
		user.setName("jay");
		user.setPass("asss");
		ss.save(user);
2 删除一条语句--删除语句需要开启事务和提交事务或者是刷新Session
		Session ss = sf.openSession();
		//Transaction tx = ss.beginTransaction();
		User user = new User();
		user.setId(id);
		ss.delete(user);
		//tx.commit();
		ss.flush();
3 修改一条语句--修改语句需要开启事务和提交事务或者是刷新Session
		Session ss = sf.openSession();
		Transaction tx = ss.beginTransaction();
		ss.update(u);
		tx.commit();
4 查询一条具体的数据-使用get()方法
		ht.get(实体类.class,id)-根据传入的参数
5 查询数据库中有多少条--两种方式
		Session ss = sf.openSession();
		String hql ="select Count(*) from User";
		Query query = ss.createQuery(hql);
		return Integer.parseInt(query.uniqueResult().toString());
6 查询多条语句
		String hql = "select u from User u";
		Session ss = sf.openSession();
		Query query = ss.createQuery(hql);
		return query.list();
```

