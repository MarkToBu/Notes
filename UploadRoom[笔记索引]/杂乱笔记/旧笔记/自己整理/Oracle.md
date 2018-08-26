# Orical数据库

oracle  神谕  他的第一个客户=>CIA
sqlplus = 让我们和oracle数据库进     行交流

SQL = Structured Query Language => 结构化查询语言

DDL DML DQL DCL TCL

DDL => Data Defination Language => 数据定义语言
create 创建  alter 修改  drop 扔掉  truncate 截断6

DML => Data Manipulation Language => 数据操纵语言
insert 新建  delete 删除  update 修改

DQL => Data Query Language => 数据查询语言
query  查询

DCL => Data Control Language => 数据控制语言
grant 授权  revoke 撤销授权

TCL => Transaction Control Language => 事务控制语言
commit 提交  rollback 回滚  savepoint 保存还原点

## (1) 基本语句

### [1] 账户和用户

如何解锁一个账户：
 alter user scott account unlock;

如何锁定一个账户：
 alter user scott account lock;

如何修改一个账户的密码：
 alter user scott identified by etoak;

如何创建一个新账户：
 create user et1803 identified by et1803;

如何给用户授权：
 grant dba to scott;

如何给用户撤销授权：
 revoke dba from scott;

如何删除用户：
drop user 用户名;

如何切换用户：
conn 用户名/密码

查看当前用户：
show user

清屏 host cls

### [2] 表格语句

如何创建一张表：

 create table studentr（
   name varchar2(20),
  birthday date,
  salary number(5,-2)
);

如何查看已经存在的表结构
desc student;

如何修改已经创建的表名
alter table student rename to teacher;

建表以后 增加字段
alter table student add email varchar2(20);

建表以后 修改字段名
alter table student rename column email to youxiang;

建表以后 修改字段数据类型
alter table student modify name varchar2(10);

建表以后 删除字段
alter table student drop column youxiang;

建表以后 删除表
drop table student purge;
truncate table student;

插入一条数据
1：
insert into student(name,salary,birthday)
values('xiaowang',123.45,sysdate);
2:
insert into student values('xiaohong',456.12,sysdate);

修改数据
update student set name = 'xiaogang';

update student set name = 'xiaoxin' where name = 'xiaowang';

update student set name = 'xiaobai',salary = salary + 200
where name = 'xiaoxin';

删除数据
delete from student where name = 'xiaoxin';

查询数据
select * from student;
select name,salary from student;

like:
%:代表任意位的任意字符  
_:代表一位上的任意字符
select * from student where name like '%gx%';

### [3] 符号

```sql
运算符：
算术：
+ - * / mod(x,y)
select mod(14,5) from dual;

关系比较：
> < >= <= = !=       
****<>****
<>是标准语法，可以移植到其他任何平台
!=是非标准语法，可移植性差

条件：
and           表示：并且  同时成立
or            表示：或者  有一个条件成立就行
between and   表示：闭合区间

与空判断：
is null      表示：空
is not null  表示：不空
```

### [4] 函数

```sql
多行函数：聚组函数：组函数：聚簇函数
count() 记录数   sum() 求和  avg() 平均数  max() 最大值  min() 最小值

1.查看当前表有多少条记录
select count(*) from student;
select count(email) from student;
2.统计某个字段的和
select sum(salary) from student;
3.求平均数
select avg(salary) from student;
4.求最大值
 select max(salary) from student;
5.求最小值
 select min(salary) from student;

单行函数:
ceil()：返回大于等于x的最小整数
floor()：返回小于等于x的最大整数
select floor(11.1) from dual;

round()：四舍五入
round(a1,a2)  保留指定位小数位的四舍五入
select round(3.141592653,3) from dual;

trunc()：直接截断
trunc(a1,a2)：保留指定位数的小数
select trunc(3.141592653,3) from dual;

sign()：求符号位   正数：1  负数：-1  零返回0
select sign(-888) from dual;

abs()：求绝对值
select abs(-888) from dual;

power(a,b)：求a的b次方
select power(2,3) from dual;

sqrt()：求正平方根
 select sqrt(16) from dual;

转换函数:
to_number(C)：将一个字符类型的数字变成数值类型
select * from student where 5=to_number(EMAIL);
to_char()：
1.将数字转换为字符串
select * from student where '456.12' = to_char(salary);
2.常用在货币单位，格式化字符串
select to_char(123123123123.00,'999,999,999,999,999.00') from dual;
3.日期转换
to_char(日期，'yyyy-MM-dd HH:mm:ss')
select to_char(sysdate,'yyyy-mm-dd hh24:mi:ss') from dual;
select to_char(systimestamp,'yyyy-mm-dd hh24:mi:ss:ff3') from dual;

to_date(c1,c2)
c1:字符类型的日期
c2:格式
select to_date('2018-03','yyyy-mm') from dual;
sqlplus中变换日期输出格式的方法--当前对话框
alter session set nls_date_format = 'yyyy-mm-dd hh24:mi:ss';

取出年  取出月  取出日
select to_char(to_date('2018-04-04','yyyy-mm-dd'),'yyyy') from dual;
select to_char(to_date('2018-04-04','yyyy-mm-dd'),'month') from dual;
select to_char(to_date('2018-04-04','yyyy-mm-dd'),'dd') from dual;

日期函数：
日期可以加减（整数）运算  单位是：天  相差结果是多少天
两个日子没法相加

yyyy 年  year 年
mm   月  month 带"月"的月份
ddd  年中的天
dd   月中的天
d    周中的天
hh   12小时制
hh24 24小时制
mi   分钟
ss   秒
xff  毫秒

java:yyyy-MM-dd HH:mm:ss
Oracle:yyyy-mm-dd hh24:mi:ss:xff
ff3  毫秒数保留三位

add_months(c1,c2)  在某个日子上增加多少个月
c1:日期
c2:整数值
select add_months(sysdate,-1) from dual;

months_between(c1,c2)  计算两个日期之间的月份
计算方式：c1-c2
select months_between(sysdate,'2018-03-03') from dual;

last_day(c1)  计算给定日期的所在月份的最后一天
select last_day(sysdate) from dual;

next_day(c1,c2)  
c1:日期
c2:周中的某天
select next_day(sysdate,'星期二') from dual;

字符函数：
lower():转换成小写
upper():转换成大写
initcap():首字母大写
length():求长度
select upper(name),initcap(name),length(name) 
from student where name='xiaoli';

substr(c1,c2,c3)  截取字符串
c1:被截取的字符串
c2:从哪个位置开始截取
c3:截取长度  默认截取到最后
select substr('woshizhizhuxia',6) from dual;
select substr('woshizhizhuxia',6,6) from dual;

instr(c1,c2,c3,c4)  索引字符串
c1:被查询的字符串
c2:希望找到的字符
c3:从哪个位置开始找  默认是1
c4:第几次出现
select instr('woshizhizhuxia','z',1,2) from dual;

concat(c1,c2)  拼接字符串
select concat('123','456') from dual;
select '123' || '456' from dual;

lpad(c1,c2,c3)  左侧补全
rpad(c1,c2,c3)  右侧补全
c1:希望补全的字符串
c2:补全到多少位
c3:以哪个字符来补全
select rpad('abc',10,'a') from dual;

trim(c1) 默认c1的两侧去除空格
trim(c1 from c2) 把c2的两侧移除指定的c1
 select trim('a' from 'aaaabaabcaabaaaa') from dual;

ltrim(c1,c2) 左侧去除
rtrim(c1,c2) 右侧去除
c1:被去除的字符串
c2:去除的字符串  默认是空格
 select ltrim('           abc',' ') from dual;

replace(c1,c2,c3)  完全替换
c1:原字符串
c2:被替换的字符串
c3:替换的字符串
 select replace('12311111','1','5') from dual;
 select replace(name,'x','3') from student;

通用函数
nvl():空值处理
nvl(字段，替换显示的内容)
select email,nvl(email,'888') from student;

nvl2():空值处理二代
nvl2(字段，不是空显示什么，是空显示什么)
select email,nvl2(email,'888','666') from student;

decode(c1,c2,c3,c4,c5,....Cx,Cx+1)
c1：原来拿来判断的值
从第二个参数开始，每两个参数看作是一组，拿每一组的第一个参数和c1进行比较
如果相同则返回该组的第二个参数
第一次判断：c2==c1?c3:
第二次判断：c4==c1?c5:
如果参数个数是奇数个，并且最终判断没有相同的数据，则返回空
如果参数个数是偶数个，并且最终判断没有相同的数据，则返回最后一个参数的值
select name,decode(email,'3',salary+200,'7',salary-200,salary+500) a 
from student;

条件取值语句：
case email                --开始条件取值
when '3'                  --当条件成立
then salary+200           --则。。
when '7' then salary-200  --。。。
else salary+500           --默认值
end                       --结束


select name，email,
(case email
when '3' then salary+200
when '7' then salary-200
else salary+500
end) a
 from student 

查询手机号中间四位以****代替
select substr(phone,1,3) || '****' || substr(phone,8) from student;
```

### [5] 排序和分组

排序  order by

```sql
排序字段：desc 代表降序  asc 代表升序
order by 后面可以加多个字段
order by 字段1 desc,字段2 asc 
select * from student order by salary desc;降序排列
select * from student order by salary asc;升序排列
```

分组  group by

```sql
根据某一个或多个列上的值，将该列上相同的值所在的记录划分为一个组
这样一张表就可以被划分为多个组
如果以字段A分组，那么只能直接查询A
****或者用组函数统计其他字段，不能直接查询其他字段
select count(email),name from student group by name;
having 后面加限制条件 
```

## (2)约束和限制

### [1] 定义

主键约束：primary key
在一个表中，能够唯一定位一条数据的列
主键列：非空且唯一
一张表只能有一个主键
数据库会在具有唯一性的列上自动添加唯一性约束
create table 表名（
 id number constraint 约束名 (id_zhujian) primary key,
 列名  数据类型
）
*：constraint ：约束名  可以不写

非空：not null

唯一：unique

检查：check

### [2] 外键约束

外键约束：foreign key  *:references
外键：在子表中，如果有一个列引用了母表中的主键列上的值，
那么在子表中这个列就是外键列
student 母表   id  主键
class   子表   id  主键  sid对应student的id
*:一张表可以有多个外键

### [3] 约束语句

如何添加约束：
alter table test add constraint aa  primary key(tid);

如何查看已经存在的约束：
select constraint_name,constraint_type from user_constraints
where table_name = upper('test');

如何删除约束：
alter table test drop constraint SYS_C0011126;	

```
*：数据库中表的约束越多，表越健壮
*：约束越多，效率越低
*：通常一张表中常用的约束  主键约束  外键约束   
```

### [4] 子查询

子查询 = 嵌套查询 = 某些查询的条件是通过查询得出来的

案例：

```
select (select name from school where id=sid),name from student;
select name from student where sid=(select id from student
where name like '哪吒');
```

### [5] 连接查询

#### [1] 内连接查询

内连接：通过关系能够关联出部分数据
innner join on = join on
select student.name,school.name from student
inner join school on student.sid = school.id;

#### [2] 外链接[左外链接]

外连接：不仅包括有关联关系的数据，还包括没有关联关系的数据
outer join on
左外联接：以左表为主，左表中的每一行数据，无论是否满足条件
最终都会显示，但是关联表的所有信息都以空显示

from 左表（旧表） left join 右表（新表） on 关联关系

*：旧表关联上新表的数据+关联不上的旧表的部分数据

select school.name,student.name from school
left join student on school.id = student.sid;

#### [3] 右外链接

右外连接：右表为主 右表中的每行数据都显示 无论是否满足条件
最终都会显示，但是关联表的所有信息都以空显示
​    
from 左表（旧表）  right join 右表（新表） on 关联关系
*：旧表关联上新表的数据 + 关联不上的新表的部分数据

select student.name,school.name from student
right join school on student.sid = school.id;

#### [4] 全外链接

全外连接：两张表的数据都展示
full join on
from 左表（旧表） full join 右表（新表） on 关联关系
*：旧表关联上新表的数据+关联不上的旧表的数据+关联不上的新表的数据
select student.name,school.name from student
full join school on student.sid = school.id;

#### [5] 交叉连接

交叉连接：两张表的数据一一对应 交叉形成最终结果 -- 没有关联关系也行
cross join
select student.name,book.bname from student
cross join book;

select class.name,school.name,student.name from student
left join class on class.id = student.cid
right join school on school.id = student.sid;

#### [6] 子查询和多表连接

*什么时候用多表连接？什么时候用子查询？

1 如果需要查询的数据在多个表中，一定要用多表连接

2 不需要表A中的列，但是又有该表A的条件，可以用子查询

3 子查询中如果使用了 in some any all 这几个关键字，效率比较低，可以转换成多表关联的的方式

## (3)伪列

伪列 = 假列 = 根本不存在的列
*：不属于表中真正的列，数据库查询的表添加的列

#### [1] rownum 分页

这是Oracle对查询动态结果动态的编号，用来实现”分页查询“

有序的整数列，每多一条自动加1

1）不能和order by 在同一个查询语句中

因为order  by 会打乱select的查询的顺序

2）不能使用表名，rownum的写法

3）特性

如果rownum用在where条件之后则：rownum>只能是0  rownum<任意数  rownum<=任意数  rownum>=1

案例：

```sql
1.实现第一页数据 编号1-10
select rownum,substr(content,1,10)||'...' from lyric
where rownum between 1 and 10;

2.实现第二页数据 编号11-20
select rownum,a.* from (select rownum rn,substr(content,1,10)||'...' 
from lyric where rownum <=20)a where rn>=11;

3.实现第三页数据 编号21-30
select rownum,a.* from (select rownum rn,substr(content,1,10)||'...' 
from lyric where rownum <=30)a where rn>=21;

4.排序效果的分页
select rownum,a.* from (select rownum rn,substr(content,1,10)||'...'
from (select * from lyric order by content)where rownum<=30)a where rn>=21;
```

#### [2] 计算每页的行数

curpage：当前页

pagesize：每页行数

当前页的开始数：int   startpage  = (curpage - 1) * pagesize  +1

当前页的截止数： int endpage = curpage * pagesize;

#### [3] rowid

rowid = 是映射每一行数据物理地址的唯一标识

通常适用于删除删除完全重复的数据

案例：

```
delete from lyric where rowid not in (select min(rowid) from lyric
group by content);
```

#### [4] 联合关键字

```sql
union  结果唯一
select * from school where name like '%大'
union
select * from school where name like '北%';

union all 结果不唯一
select * from school where name like '%大'
union all
select * from school where name like '北%'

intersect 求交集
select * from school where name like '%大'
intersect
select * from school where name like '北%'

minus 从第一个查询结果中，减去第二个查询结果中重复出现的数据
select * from school where name like '%大'
minus
select * from school where name like '北%'
in:表示条件符合查询结果中某一个值就成立
条件 in(值1，值2。。。。)
相当于：条件=值1 or 条件=值2 or ...
 select * from school where id in(select sid from student);

not in:表示条件不能是查询结果中的任意一个值
条件 not in(值1，值2。。。。)
相当于:条件<>值1 and 条件<>值2 and 。。。
select * from school where id not in (select sid from student);

some/any:这两个用法和in相同
区别在于:in可以在无符号的情况
         some/any用在有符号的情况
select * from school where id = some(select sid from student);

all:表示所有的值都大 或 都小  >all  <all
***:
>any   >min
<any   <max
>all   >max
<all   <min

select name,salary from student where salary
<any(select salary from student where cid = 1);

select name,salary from student where salary
<all(select salary from student where cid = 1);

*:因为in的效率低，所以用exists代替

exists:是否存在   not exists
用法:不需要用列来比较，只关心后面的查询有没有结果
select * from school s where exists
(select * from student st where st.sid = s.id);
```

### (4) 序列

一个单独的数据对象，是一个能够生成有序的整数列值的对象
*****：oracle通过调用序列的形式来实现主键自增
***：在一个新的会话中，必须要调用下一个值才能获得当前得值
***：seq_student.nextval会作为下一个添加的初始值

#### [1] 序列的语句

```sql
create sequence 序列名;

create sequence seq_student
increment by 1/-1  --增长，一次增1   正数+1  负数-1
start with 1       --从1开始    升序默认就是+1  降序-1
minvalue 1         --最小值     -10的26次幂
maxvalue 100       --最大值     10的27次幂
cycle              --循环       默认nocycle
nocache;           --不缓存     cache 4 默认生成20个序列号

查询当前用户下有多少序列
select * from user_sequences;

如何获取下一个列的值
seq_student.nextval
insert into test values(seq_test.nextval,'g');

如何获取当前序列的值
seq_student.curval

如何删除序列
drop sequence seq_test;

如何修改序列
alter sequence seq_test increment by 50;

消除延迟段创建特性
alter session set deferred_segment_creation=false;
```

## (4)视图

视图  view=》假表 用查询结果动态生成一张表

视图是编译之后将查询语言保存到数据库中，下次调用视图，可以不用再编译，直接获取数据

创建视图

create view  视图名 as select  * from student

查询视图

select  * from 视图名

案例

```sql
create view v_student as select id,name from student;
select * from v_student;

create view studentName_schoolName as
select student.name sn,school.name cn from student
left join school on student.sid = school.id;
```

使用视图的原因？

1 节省编译时间，提高查询效率

2 屏蔽原表中的字段，避免没有权限的用户查询其他字段（看到不该看的）

3 视图中能够根据动态来源于原表（alter view 视图名 complie）

4 简单视图是可以更新原表的数据

5 复杂的视图无法更新（关联出来的数据不能只更新其中的一条）

## (5) 表空间 tablespace

oracle 中的用户都有属于自己的默认的空间

在一段内存空间中大部分存储的是表，所以称为表空间

类别：用户的表空间

 	系统用户的表空间

​	普通用户的表空间

为什么要给普通用户创建属于自己的空间呢？

项目中很可能与其他项目使用同一数据库，多个用户在使用同一数据库的时候有可能访问同一数据库文件，就会造成资源争用问题，给不同用户制定不同的表空间，就可以让她们使用不同的数据文件，解决争用问题。

临时表空间
数据最终存储在数据块中
数据块=>盘区=>段=>数据文件=>表空间

创建表空间

create tablespace 表空间名  nologging

datafile  ‘’                         -----文件位置（一定要存在）

size                100m 	-----初始大小

autoxdent  on  text          -----下一次扩展大小

maxsize      			-----最大值

给用户指定表空间

```
create  user  et1803  indetified  by  et1803
default tabkespace  表空间名  et1803；
```

创建用户后修改

```
alter  user et1803 default tablespace 表空间名  et1803;
```

删除表空间

```
drop tabledopace 表空间 including ontends and datailes;
```

\*删除表空间后，原先指向该表空间的用户仍然默认的空间位置，需要通过 alter user 命令将用户的表空间指向一个有效的表空间。

## (6) 索引 index

数据库会在具有唯一性的列上自动添加唯一性索引

创建索引

create  index 索引名  on  表名 （字段）；

查询索引

```
select index_name,table_name,uniqueness,status from user_indexes
where table_name like 'STUDENT';
```

修改索引

```
alter index index_name rename to new_index_name;
```

删除索引

```
drop index 索引名;
drop index new_index_name;
```

类型：

普通索引：normal

create index  索引名  on  表名（列名）；

唯一索引：unique

create unique  index  索引名 on 表名（列名）；

位图索引（分类）：bitmap
这种索引适合用在数据量比较大，基数比较小的列    比如：男/女/..
create bitmap index 索引名 on 表名（列名）;

函数索引：在一个列上经过函数计算后的结果上创建索引
create index 索引名 on 表名（函数（列名））;

创建索引的优缺点

1 能够更快的帮助我们进行提高查询的效率

2 增删改表中的数据，数据库就需要耗费资源去维护索引，降低增删改的效率

3 数据量如果很少，没必要用索引

4 数据量比较大，不需要经常增删改操作而且查询比较多，适合使用索引

## (7) 存储过程  procedure

在服务器端，能够被一个或多个应用程序调用的一段sql语句集

创建存储过程

```sql
create or replace  procedure
过程名（参数名  in  参数类型，参数名 in 参数类型，参数名  out 参数类型）
as 
变量名  变量类型  ：= 值；
begin
sql语句集
end；
```

注意：

1 存储过程可以没有参数，如果没有参数则过程名之后不能出现括号

2 存储过程可以有参数

3 传入参数用int标明，传出参数用out标明

4 可以有多个参数传入，也可以有多个参数传出

5 可以只有传入参数，也可以只有传出参数

6 存储过程没有返回值，而是通过返回参数来返回数据

案例：

如果传入参数1，则返回你好
如果传入参数2，则返回再见

```sql
创建存储过程
create or replace procedure
pro_hi(mykey in number,value out varchar)
as
begin
if mykey = 1
then value := '你好';
else if mykey = 2
then value := '再见';
end if;
end if;
end;
/
开启
set serveroutput on；
调用存储过程
declare
val varchar2(20):='';
begin
pro_hi(1,val);
dbms_output.put_line(val);
pro_hi(2,val);
dbms_output.put_line(val);
end;
```

## (8) 触发器 trigger

for each row：行级触发器的标志

创建触发器

```sql
create or replace trigger 触发器名
before/after  insert/update/delete on 表名
for each row
begin
sql语句集
end；
```

案例1 ：

```sql
---创建表
create table good（
ID int primary key
good_name varchar2(20)
);
---创建序列
create sequence seq_ids；
---创建触发器
create or replace trigger tri_insert_good\
before insert on good
for each row
begin 
select seq_ids.nextval
into :new.id
from dual;  --从序列中生成一个新的数值，赋值给当前插入行的id列
end;
---增加数据
insert into good（good_name） values ('苹果');
insert into good（good_name） values（'香蕉'）；
---查询数据看结果
select * from good；
```

案例2 ：

```sql 
---创建日志表
create table test_log（
name varchar2(20),
time date
);
---创建一个触发器
create or replace trigger test_dept
before delete or insert or update on dept
declare var_tag varchar2(20); --声明一个变量，存储对dept表执行的操作类型
begin
if inserting then 
var_tag :='insert';
elsif updating then
var_tag :='update';
elsif deleting then 
var_tag :='delete'；
end if;
insert into test_log values(var_tag,sysdate); --向日志表中插入对dept表的操作信息
end;
---分别执行语句对dept表进行操作
insert into dept values(66,'教育部','济南');
update dept set loc='易途' where deptno = 66;
delete from dept where deptno = 66;

select * from dept;
select * from test_log；
```

## (9) 去重 distinct 和group by

支持单列，多列的去重方式

select   distinct sid  from student;

select  distinct  sid,name,id  from student;

group by ：聚合统计[也可以去除重复]

## (10) sql 语句优化

1 建议不要用 *  来代替列名

2 用truncate (只删除数据，触动触发器)代替delete(删除数据，也触动触发器)

3 在确保语句完整性的情况下多用commit语句(用在begin...end中)

4 尽量减少表的查询次数

5 表连接在where条件之前，数据多的记录在where字句的末尾，表连接之前过滤的数据越多越好

6 用not exists 代替 not in

7 合理使用索引

8 sql的语句尽量大写,oracle总是先解析sql语句,把小写的转换成大写的再执行

9 连接多个表时尽量使用表的别名，减少解析时间

10 优化group by，将不需要的记录在group by之前过滤掉

## (11) 查询低效率的执行的sql

```
 SELECT EXECUTIONS,DISK_READS,BUFFER_GETS, 
      ROUND((BUFFER_GETS-DISK_READS)/BUFFER_GETS,2) HIT_RADIO,
      ROUND(DISK_READS/EXECUTIONS,2) Reads_per_run,SQL_TEXT 
 FROM V$SQLAREA 
 WHERE EXECUTIONS > 0 AND BUFFER_GETS > 0 
 AND (BUFFER_GETS-DISK_READS)/BUFFER_GETS < 0.8 
 ORDER BY 4 DESC;
```

## 创建表的要求

```
创建表的3范式
     1.关系型数据库  oracle mysql
     2.在表中的一条记录中有个多字段，
          多个字段中必须有一个字段是唯一的
          主键
          例如  id 主键
     3.字段不能冗余      
     反3范式（第三范式）
           字段可以冗余          
     单表
      一对一  主 从（主外键）
      一对多  主 从（主外键）
      多对多  桥表（外键）
```

