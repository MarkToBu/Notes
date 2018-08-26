### Hibernate 检索方式

1. ◦导航对象图检索方式:  根据已经加载的对象导航到其他对象

   ◦OID 检索方式:  按照对象的 OID 来检索对象

   ◦HQL 检索方式: 使用面向对象的 HQL 查询语言

   ◦QBC 检索方式: 使用 QBC(Query By Criteria) API 来检索对象. 这种 API 封装了基于字符串形式的查询语句, 提供了更加面向对象的查询接口. 

   ◦本地 SQL 检索方式: 使用本地数据库的 SQL 查询语句

### HQL 查询语句

```
HQL 检索方式包括以下步骤:
通过 Session 的 createQuery() 方法创建一个 Query 对象, 它包括一个 HQL 查询语句. HQL 查询语句中可以包含命名参数
动态绑定参数
调用 Query 相关方法执行查询语句. 
Qurey 接口支持方法链编程风格, 它的 setXxx() 方法返回自身实例, 而不是 void 类型
HQL vs SQL:
HQL 查询语句是面向对象的, Hibernate 负责解析 HQL 查询语句, 然后根据对象-关系映射文件中的映射信息, 把 HQL 查询语句翻译成相应的 SQL 语句. HQL 查询语句中的主体是域模型中的类及类的属性
SQL 查询语句是与关系数据库绑定在一起的. SQL 查询语句中的主体是数据库表及表的字段. 

绑定参数:
Hibernate 的参数绑定机制依赖于 JDBC API 中的 PreparedStatement 的预定义 SQL 语句功能.
HQL 的参数绑定由两种形式:
按参数名字绑定: 在 HQL 查询语句中定义命名参数, 命名参数以 “:” 开头.
按参数位置绑定: 在 HQL 查询语句中用 “?” 来定义参数位置
相关方法:
setEntity(): 把参数与一个持久化类绑定
setParameter(): 绑定任意类型的参数. 该方法的第三个参数显式指定 Hibernate 映射类型
HQL 采用 ORDER BY 关键字对查询结果排序

分页查询:
setFirstResult(int firstResult): 设定从哪一个对象开始检索, 参数 firstResult 表示这个对象在查询结果中的索引位置, 索引位置的起始值为 0. 默认情况下, Query 从查询结果中的第一个对象开始检索
setMaxResults(int maxResults): 设定一次最多检索出的对象的数目. 在默认情况下, Query 和 Criteria 接口检索出查询结果中所有的对象

投影查询: 查询结果仅包含实体的部分属性. 通过 SELECT 关键字实现.
Query 的 list() 方法返回的集合中包含的是数组类型的元素, 每个对象数组代表查询结果的一条记录
可以在持久化类中定义一个对象的构造器来包装投影查询返回的记录, 使程序代码能完全运用面向对象的语义来访问查询结果集. 
可以通过 DISTINCT 关键字来保证查询结果不会返回重复元素

报表查询用于对数据分组和统计, 与 SQL 一样, HQL 利用 GROUP BY 关键字对数据分组, 用 HAVING 关键字对分组数据设定约束条件.
在 HQL 查询语句中可以调用以下聚集函数
count()
min()
max()
sum()
avg()

```

