# Hibernate面试题

## 1 Hibernate是什么？

```
Hibernate是一个ORM映射框架，是jdbc的封装，Hibernate自动完成了实体类和表的映射，我们只需要提供映射关系文件，Hibernate自动根据映射关系生成sql语句，自动生成
```

## 2 持久层的框架

```
1 Hibernate
2  mybatis
3 jpa
4 jdbc
```

## 3 Hibernate的流程

```
1 Configuration解析两个配置文件（属性文件和映射文件）
2 创建sessionFactory
3 获得session
4 开启事务 session.BeginTranscation()
5 save update delete get
6 commit/rollback()
7 close
```

## 4 Hibernate中的主键自增长策略

### 1 原因

```
1 Hibernate实现了各种数据库之间的来切换，我们不需要关注底层sql的实现细节
2 不同的数据库在如何实现主键自动增长上的策略是不同的，mysql中支持auto_increment自动增长，Oracle中没有自动		增长语句，ORACLE自动增长需要序列的方式实现或者查询最大id+1的方式。
3 Hibernate中设置了许多自动增长的策略
```

### 2 策略

```
1 assigned:手动设置主键，通过程序的方式生成主键，可以自定义主键的生成策略
2 identity：使用数据库中的主键自动增长策略，手动设置的主键不起作用，不适用与oracle数据库
3 sequence：适用于oracle数据库，是通过序列来生成主键-hibernate_sequence名字，所有必须保证数据库中有相应的		序列
	--序列的作用:生成一系列的数的，通常用来维护主键
	--视图 view：1简化查询 把复杂SQL语句可以写到一个视图中，每次查询视图。 2.效率高 3.安全性高
	--索引index:提高查询效率 
4 native:自动根据底层的数据库来选择相应的策略，如果是mysql数据库，则是identity，如果是oracle则是，				sequence	
5 increment:1 先查询最大ID 2 最大id+1作为新的ID添加到数据库中
6 uuid:Hibernate使用UUID算法生成16进制的32位的字符串-		（(UUID.randomUUID().toString().replace(“-”,””)）
7 foreign:关联
```

## 5 Hibernate中的对象的状态

```
1 Transient：临时状态--刚刚new出来 数据库中没有对应的记录 也没有session关联
2 persistent：持久化状态--只要调用了语句方法，就变成了持久化状态，并且有唯一的session关联，并保存到一级缓			存中	
3 Detached: 脱管状态--session.close（）关闭之后就变成了脱管状态
```

## 6 session中相似的方法

```
1 save/saveUpdate/Update
	1):save：保存对象：insert
	2):Update: 修改对象 update--需要有主键
	3):saveUpdate：有主键就update，没主键就save
2 get/load 都是查询数据
	（1）get:立即加载，当执行到get方法立刻发出SQL语句，立刻执行SQL语句。
	（2）load:延迟加载，执行到load方法并不会立刻发出SQL语句，而是到真正需要用来对象中的属性时，才去发出SQL语句。
	（3）当数据库中没有真正的数据时，get方法返回null，load方法返回ObjectNotFoundException
3 close/clear/evict
	close：销毁 关闭session
	clear：清空缓存
	evict：将指定的对象从session中清空，而不是清楚所有
```

## 7 Hibernate中的缓存机制![图片](D:\yitupeixun\自己整理\图片.png)

```
1 session中的一级缓存，与session绑定在一起的，session创建的时候，一级缓存一起创建，session销毁，一级缓存			销毁,其中有些什么对象，是session自动维护的‘
2 sessionfactory维护的是二级缓存，默认是关闭的 
```

## 8 Hibernate如何分页

```
Hibernate使用底层数据库的分页机制，如果底层使用的是Mysql数据库，则使用limit分页，
			 * 如果使用的oracle数据库，则使用rownum分页，Hibernate将分页操作封装到了两个方法中，
			 * setFirstResult():从第几条记录开始查询 下标从0开始
			 * setMaxResults()：最大显示多少条记录
```

