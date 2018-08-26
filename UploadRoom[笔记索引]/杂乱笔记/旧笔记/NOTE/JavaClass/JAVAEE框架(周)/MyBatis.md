## MyBatis

##   1.1.0 **MyBATIS**

******【2010年之前叫iBatis，作者：克林顿.begin】**

**第一、****MyBATIS****是什么？**

**1．****MyBATIS****和****Hibernate****一样，也是一个持久层的****ORM****框架。**

**2．****MyBATIS****底层也是驱动的****JDBC****，也是对数据库进行****CRUD****的。**

**3．****MyBATIS****的特点是：****SQL****语句的映射。【****MyBATIS****中所有的****SQL****语句都是程序员写的****，而不是自动生成的，但是也是会自动的执行，自动返回结果。】所以****MYBATIS****是一个半自动的****ORM****框架。**

**4．****MyBATIS****经常被称作****SQL** **映射框架****。**

##  1.1.1    第二、**MyBATIS****、****Hibernate****和****JDBC****的比较？**

1．**JDBC****：全手动****===>****手动写****SQL****，手动执行****SQL****，手动返回结果**

**效率最高**   **代码冗余**

2．**Hibernate:****全自动** **---****》自动生成****SQL** **自动执行****SQL****自动返回结果**                             **简洁**  **效率** **最低**

3．**MyBAITS:****半自动** **-->****手动写****SQL** **自动执行** **自动返回结果**

**效率比****Hi** **高**  **SQL****开发工作量** **移植性弱。**

## 1.1.2 第三、**MyBATIS****核心的****API**？

1．org.apache.ibatis.session.SqlSessionFactoryBuilder:

（1）build

2．SqlSessionFactory:   

（1）openSession

3．SqlSession:

（1）Insert(String sqlid)

（2）Update(String sqlid)

（3）Delete(String sqlid)

（4）selectOne() selectList()

4． 

## 1.1.4 第四、MyBATIS中的配置文件

1．属性文件 config.xml===>链接数据库的信息

2．映射文件===》SQL语句 

## 1.15 第五、**第一个****MyBATIS的工程?  MyBATIS使用

（Student对象CURD）

第六、**当传递多个参数时，使用****Map**

第七、**ParameterType:****标识参数类型，常用的** **int** **、自定义（****Student****）和****Map****类型**

第八、**MyBATIS****中****#****和****$****的区别？**

1．**#:****相当于****preparedStatement****，支持？占位符的**  **推荐**

2．$:相当于普通的Statement,不支持？占位符，条件都是直接拼接

3．有些情况必须使用$

（1）Select * from xx whre name like ’${name}’

et

\#==》select * from xx where name like ‘”et”’

$==>select * from xx whre name like ‘et’

（2）Select * from ${talbename}

Student

\#===> select * from  “student”

$===>seelct * from student

（3）?用在 where后、update set后边  delete  where  、insert values后边 ，其他地方不能用?

 

第九、**类和表不对应的情况**？

1．添加

（1）Insert into xx(s_id,s_name..字段)values(#{属性}，#{属性})

2．查询

（1）起别名

（2）通过resultMap映射

3． 

第十、**添加数据时，如何获得主键？**

JDBC: 手动获得

MYBATIS:

<selectKey keyProperty=”主键属性” resultType=”返回的类型”

Order=”BEFORE/AFTER”>

Mysql: 

字符串select replace(uuid(),’-’,’’) as id 

数字: select last_insert_id() as id 

Oracle:

字符串： select sys_guid() as id

数字:

序列：select etoak_seq.nextval as id from dual

最大值+1：select max(id)+1 from xxx 

<selectKey>

 

第十一、MyBATIS中的动态查询标签？

Sql  include  if  where foreach

第十二、MyBATIS中批量添加？

1．foreach



```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <settings>
        <setting name="logImpl" value="LOG4J"/>
    </settings>
    <environments default="dev_mysql">
        <environment id="dev_mysql">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql:///et1803?useUnicode=true&amp;characterEncoding=UTF-8"/>
                <property name="username" value="root"/>
                <property name="password" value="admin"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="org/mybatis/exam5+ple/StudentMapper.xml"/>
    </mappers>
</configuration>
```

```java
public class SF {
    private static SqlSessionFactory f ;
    private SF(){}

    static {
        try {
            Reader reader = Resources.getResourceAsReader("config.xml");
            f=  new SqlSessionFactoryBuilder().build(reader);
        }catch (Exception e1){
            e1.printStackTrace();
        }
    }

    public static SqlSession getSession(){
        return f.openSession();
    }

    public static void main(String[] args) {
        System.out.println(SF.getSession());
    }
}
```



Mapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?> <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.et1803">
    <insert id="addStu" parameterType="com.etoak.entity.entity.Student">
        insert into student1 (name, age, birth)
        values
        (#{name},#{age},#{birth})
    </insert>
    <update id="updateStu" parameterType="com.etoak.entity.entity.Student">
        update student1 set  name=#{name},age=#{age},birth=#{birth} where id=#{id}
    </update>
    <delete id="deleteStuById" parameterType="int">
        delete from student1 where id=#{id}
    </delete>
    <select id="queryStuById" parameterType="int" resultType="com.etoak.entity.entity.Student">
        select * from student1 where id=#{id}
    </select>

</mapper>
```

- daoIMPl

  ```java
  package com.etoak.entity.dao;
  
  import com.etoak.entity.entity.Student;
  import com.etoak.entity.factory.SF;
  import org.apache.ibatis.session.SqlSession;
  
  public class StudentDaoImpl implements IStudentDao {
      SqlSession session = null;
      @Override
      public void addStudent(Student stu) {
  
          try {
              session = SF.getSession();
              session.insert("addStu",stu);
              session.commit();
          }catch (Exception ex){
              if(session != null){
                  session.rollback();
              }
          }finally {
              if(session!=null){
                  session.close();
              }
          }
  
      }
  
      @Override
      public Student queryStuById(int id) {
  
          try {
              session = SF.getSession();
              Student stu = session.selectOne("queryStuById",id);
              session.commit();
              return stu;
          }catch (Exception e){
              e.printStackTrace();
              if(session != null){
                  session.rollback();
              }
          }finally {
              if(session != null){
                  session.close();
              }
          }
  
          return null;
      }
  
      @Override
      public void updateStudent(Student stu) {
          try {
              session = SF.getSession();
              session.update("updateStu",stu);
              session.commit();
  
          }catch (Exception e){
              e.printStackTrace();
              if(session != null){
                  session.rollback();
              }
          }finally {
              if(session != null){
                  session.close();
              }
          }
  
  
      }
  
      @Override
      public void deleteStudent(int id) {
          try {
              session = SF.getSession();
               session.selectOne("deleteStuById",id);
              session.commit();
  
          }catch (Exception e){
              e.printStackTrace();
              if(session != null){
                  session.rollback();
              }
          }finally {
              if(session != null){
                  session.close();
              }
          }
  
  
      }
  }
  
  ```

  ### 字符串类型 的id的添加和返回

```
 <selectKey keyProperty="id" resultType="string" order="BEFORE">
             select replace(uuid(),'-','') as id
         </selectKey>
```

##  1.2.1 mapper映射的方式增删改查

实体类的映射文件：	

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.etoak.student.dao.StudentMapper">
    <!--
    1.namespace的名字必须与接口名字保持一致  包名+接口名
    2.接口中的方法一定于语句的id保持一致
    -->
    <insert id="addStu" parameterType="stu">
      <selectKey keyProperty="id" resultType="string" order="BEFORE">
          select replace(uuid(),'-','') as id
      </selectKey>
        insert into student(id,name,age,birth,email,schoolid)
        VALUES(#{id},#{name},#{age},#{birth},#{email},#{schoolid})
    </insert>
        <!--stu.schoolid = sch.id-->
        <resultMap id="rMap_stu" type="stu">
            <id property="id" column="sid"></id>
            <result property="name" column="sname"></result>
            <result property="age" column="age"></result>
            <result property="birth" column="birth"></result>
            <result property="email" column="email"></result>
            <result property="schoolid" column="schoolid"></result>
            <association property="schoo" javaType="com.etoak.student.entity.School"
                         column="schoolid">
                <id property="id" column="schid"></id>
                <result property="name" column="schname"></result>
                <result property="loc" column="loc"></result>
            </association>
        </resultMap>
    <select id="querySome" parameterType="map" resultMap="rMap_stu">
        select
        s.id as sid,s.name as sname,s.age,s.birth,s.email,s.schoolid,
        sch.id  as schid ,sch.name as schname ,sch.loc
        from student s join school sch on s.schoolid = sch.id
        <where>
            <if test="name!=null">
                and s.name like '%${name}%'
            </if>
            <if test="schid!=null">
                and schoolid =#{schid}
            </if>
        </where>
        limit #{start},#{max}
    </select>
    <select id="countStu" parameterType="map" resultType="int">
        select count(*) from student
        <where>
            <if test="name!=null">
                and name like '%${name}%'
            </if>
            <if test="schid!=null">
                and schoolid =#{schid}
            </if>
        </where>
    </select>
</mapper>
```

映射文件所对应的接口

```java
package com.etoak.student.dao;
import com.etoak.student.entity.Student;
import java.util.List;
import java.util.Map;

public interface StudentMapper {
    public void addStu(Student stu);
    public List<Student> querySome(Map<String,Object> map);
    public int countStu(Map<String,Object> map);
}

```

- 调用接口的方法

  ```java
  package com.etoak.student.service;
  
  import com.etoak.student.dao.StudentMapper;
  import com.etoak.student.entity.Student;
  import com.etoak.student.factory.SF;
  import org.apache.ibatis.session.SqlSession;
  
  import java.util.HashMap;
  import java.util.List;
  import java.util.Map;
   
  public class StudentService {
      SqlSession session = null;
      public List<Student> querySomeByTj(String name, String schid, Integer start, Integer max){
          try{
              Map<String,Object> map = new HashMap<String,Object>();
              if(name!=null && !name.equals("")){
                  map.put("name",name);
              }
              if(schid!=null && !schid.equals("") && !schid.equals("-1")){
                  map.put("schid",schid);
              }
              map.put("start",start);
              map.put("max",max);
              session = SF.getSession();
              //获得MyBatis动态提供的StudentMapper类型的对象
              StudentMapper dao = session.getMapper(StudentMapper.class);
             // session.selectList("querySome",map);
              List<Student> data = dao.querySome(map);
              //调用接口的方法。通过接口传递值
              session.commit();
              return data;
          }catch(Exception e){
              e.printStackTrace();
              if(session!=null)session.rollback();
              return null;
          }finally{
              if(session!=null)session.close();
          }
      }
      public int countStudent(String name,String schid){
          try{
              Map<String,Object> map = new HashMap<String,Object>();
              if(name!=null && !name.equals("")){
                  map.put("name",name);
              }
              if(schid!=null && !schid.equals("") && !schid.equals("-1")){
                  map.put("schid",schid);
              }
  
              session = SF.getSession();
              //获得MyBatis动态提供的StudentMapper类型的对象
              StudentMapper dao = session.getMapper(StudentMapper.class);
              // session.selectList("querySome",map);
             int i = dao.countStu(map);
              session.commit();
              return i;
          }catch(Exception e){
              e.printStackTrace();
              if(session!=null)session.rollback();
              return 0;
          }finally{
              if(session!=null)session.close();
          }
      }
  }
  
  ```

  ####  调用过程解释：

  ​    调用service ---->service.method {session.getMapper(StudentMapper.class).methodXX();  } 

  

  ​              --->通过接口确定所调用的方法名，以此来确定调用哪个xml文件的方法。

## 1.1.3   分页查询

```

//分页查询dao层
public List<Student> querySome(String name,int age,int start ,int size){
	try{
        Map<String,Object> map = new HashMap<Stirng,Object>();
        if(name ！= null && name.equals("")){
            map.put("name",name);
        }
        if(age != 0){
            map.put("age",age);
        }
        map.put("start",start);
        map.put("size",size);
        session = SF.getSession();
        List<Student> stus = session.selectList("querySomeStu",map);
        session.commit();
        return stus;
	}catch(Exception ex){
	 ex.printStackTrace();
	 if(session != null){
	     return nulll;
	 }
	}finally{
         if(session ！= null){
            session.close();
         }
	}
}

///配置文件
 <select id="querySomeStu" parameterType="map" resultType="stu">
    <include refid="commonsql"></include>
    <where>
        <if test="name!=null">
           and s_name like '%${name}%'
        </if>
        <if test="age!=null">
            nad s_age=#{age}
        </if>
    </where>
    limit #{start},#{size}
 </select>  

```

## 1.1.4 MyBatis设置主键自增

```
Mybatis设置在插入时主键自增
... 省略号，
<mapper namespace="...">
   <insert id="addStu" parameterType="stu">
        <selectKey keyProperty="id" resultType="int" order="BEFORE">
        /* select etaok_seq.nextval as id from dual */
            select max(s_id)+1 as  id from  stu
        </selectKey>
        insert into stu(s_id,s_name,s_age,s_birth)
        values
        (#{id},#{name},#{age},#{birth})
   </insert>
</mapper>

```

#### string型主键

```xml
 <insert>
    <selectKey keyProperty="id" resutType="string" order="BEFORE">
       /*  select sys_guid() as id  */
        select replace(uuid(),'-','') as id
    </selectKey>
    insert into s(sid,sname)
    values
    (#{id},#{name})
 </insert>
```





## 1.1.5 批量添加：

```java
 List<Student> data = new ArrayList<>();

 Collections.addAll(data,new Student(,,),new Student(,,),new Student(,,),new Student(,,));

 Map<String,List<Student>> mapp = new HashMap<>();

 map.put("stus",data);

 dao.addBat(map);
 
//dao层的书写
 public void addBat(Map<String,List<Student>> map){

    session = SF.getSession();

    session.insert("addStuBat",map);

    ....

 }
  //配置文件
   <insert id="addStuBat" parameterType="map">

       insert into stu(s_name,s_age,s_birth) values

       <foreach collection="stus" item="stu" separator=",">

           (#{stu.name},#(stu.age),#{stu.birth})  

       </foreach>

   </insert>

```

