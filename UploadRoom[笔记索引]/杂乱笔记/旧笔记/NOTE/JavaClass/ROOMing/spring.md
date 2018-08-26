## Spring框架总结

## 1.0 spring简介

 1.0  Spring 是一个企业级开发框架，为简化企业级项目开发而创建，框架的主要优势之一就是分层架构，允许开发者自主选择组件。

 **Spring的两大核心机制就是 IOC  和  AOP**

从开发角度上来说，使用Spring就是使用它的IOC和AOP。

>  IOC是典型的工厂模式，通过工厂去注入对象。
> AOP是代理模式的体现。

       作用 
             主要整合技术框架，
             管理javabean，实现程序中的代码解耦，
             最终提供系统扩展性和维护性
             
     ioc
             控制反转 inversion of control 
        （实例化对象）
               在xml配置文件中表示<bean>元素 class属性
               反射 newInstance()
        di
             依赖注入  dependency injection
             （赋值）
             反射 + 解析xml（jdk）
## 1.1 Spring   IOC

###  spring ioc 介绍

​    IOC 控制反转，创建对象交给IOC容器来创建，在推送给调用者。流程反转，所以叫做控制反转。

```
  原理：
            将所有的javabean对象统一的放入xml中进行管理                 在xml中以bean元素标签体现出来，
            并且形成依赖关系，
              
            容器（tomcat web.xml）,创建所有的对象
            并且形成依赖（赋值）关系，这种依赖关系
            在必要的时候注入使用
            
             最原始的内部new对象，
	        转移到外部new对象
            
            内部 new Object（）
            外部 xml bean元素 通过反射实例化对象
            ioc xml + 反射 实例化对象
   
            整合框架（hibernate、mybatis、struts2）

             管理javabean（实例化对象  new）
```

### 注入方法：

```
    ioc注入 3种
     1.setter方法
     2.构造方法
     4.普通方法   工厂
```

### 使用示例

#### 准备工作：

​		1.添加Spring相关jar包。

​		2.创建配置文件，可以自定义文件名spring.xml。

​		3.调用API。	

#### 程序思路

1. 在spring.xml 中配置bena标签，IOC容器通过bean标签来创建对象。

2. 调用API获取ioc对象。 

   > IOC容器可以调用无参构造或者有参构造来创建对象，我们先来看无参构造的方式。 

##### >无参构造：  <bean id="sut"   class=”com...Student“><bean>

​    调用：   >  ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");               

​                                  Student stu = (Student) applicatioinContext.getBean("stu");

######   > 无参构造流程

​           -  第一步：加载spring.xml配置文件，生成ApplicationContext对象。

​           -   第二步：调用ApplicationContext的getBean方法获取对象，参数为配置文件中的id值。             

​          程序在加载spring.xml时创建stu对象，通过反射机制调用无参构造函数，所有要求交给IOC容器管理的类必须有无参构造函数。                  

######如何赋值：

<bean id="stu" class=“ com.etoak.entrity.Student”>
​    <property name="id" vlaue="1">  </property>
​    <property name="name" value="张三"></property> 
​    <property name="age" value="23"></property>
</ban>

​         添加property标签：name对应属性名，value是属性的值。 若**包含特殊字符，比如name="<张三>"，使用<![CDATA[<张三>]]>进行配置。**

> <property name="name">             
> ​     <value><![CDATA[<张三>]]></value>       
>   </property> 

​        Spring通过调用每个属性的setter方法来完成属性的赋值。所以实体类必须有setter方法，否则加载时报错，getter方法可省略。 

**2, 通过运行时获取对象**

   ````java
 Student stu = applicationContext.getBean(Student.class);
   ````

> 此方法有一个弊端： 当spring.xml中配置两个Student的bean时，程序报错。容器无法确定返回哪一个。

以上是IOC容器通过无参构造创建对象的方式，同时IOC容器也可以调用有参构造来创建对象。 

#####  > 有参构造

  首先在实体类中创建有参构造.

    ```
public Student(int id, String name, int age) {
        super();
        this.id = id;
        this.name = name;
        this.age = age;
    }
    ```

xml配置

```
<bean id ="stu3" class="com..entity.Student">
    <constructor-arg name="id" value="3"></constructor-arg>
    <constructor-arg name="name" value="xiaoming"></constructor-arg>
    <constructor-arg name="age" vlaue="22"></constructor-arg>
</bean>
```

IOC 容器会根据constructor-arg标签去加载对应的 有参构造函数，创建对象并完成属性赋值。

除了使用name对应参数外，还可以通过下标index对应。

```xml
<bean id="stu3" class="com.etoak.Student">
    <constructor-arg index="0" value="3"></constructor-arg>
    <constructor-arg index="1" value="小明"></constructor-arg>
    <constructor-arg index="2" value="22"></constructor-arg>
</bean>
```

以上是IOC容器通过有参构造创建对象的方式，获取对象同样有两种方式可以选择：id和运行时类。 

>  如果IOC容器管理多个对象，并且对象之间有级联关系，如何实现？

在 Student中有一个Classes类 ，表示所属班级。

- 配置

````xml
<beans>
    <bean id="classes" class="com.etoak.entity.Classes">
        <property name="id" value="1"></property>
        <property name="name" value="Java学习"></property>
    </bean>
    
    <bean id="stu" class="com.etoak.entity.Student">
        <property name="id" value="1"></property>
        <property name="name">
            <value><![CDATA[<张三>]]></value>
        </property>
        <property name="age" value="23"></property>
        <property name="classes" ref="classes"></property>
    </bean>

</beans>
````

> 在spring.xml中，通过ref属性将其他bean赋给当前bean对象，这种方式叫做依赖注入。DI是将不同对象进行关联的一种方式，是IOC具体实现方式。通常DI和IOC紧密结合在一起，所以一般说IOC包括DI。

##### > 集合的依赖注入：

 Classes类中添加List<Student>属性。 

```java
public class Classes {
    private int id;
    private String name;
    private List<Student> students;

    public List<Student> getStudents() {
        return students;
    }
    public void setStudents(List<Student> students) {
        this.students = students;
    }
    public int getId() {
        return id;
    }
    public void setId(int id) {
        this.id = id;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
}
```

spring.xml中配置2个student对象，1个classes对象，并将2个student对象注入到classes对象中 

```xml
...
<beans>
    <bean id="classes" class="com.etoak.entity.Classes">
        <property name="id" value="1" ></property>
        <property name="name" vlaue="Javak开发"></property>
        <property name="students">
            <list>
                <ref bean="stu"></ref>
                <ref bean="stu2"></ref>
            </list>
        </property>
    </bean>
    
     <bean id="stu" class="com.hzit.entity.Student">
        <property name="id" value="1"></property>
        <property name="name">
            <value><![CDATA[<张三>]]></value>
        </property>
        <property name="age" value="23"></property>
     </bean>
    
      <bean id="stu2" class="com.hzit.entity.Student">
        <property name="id" value="2"></property>
        <property name="name" value="李四"></property>
        <property name="age" value="23"></property>
     </bean>
</beans>
```

集合属性通过list标签和ref标签完成注入。ref的bean属性指向需要注入的bean对象。 

> 总结：

IOC是Spring的核心，很重要。使用Spring开发项目时，控制层，业务层，DAO层都是通过IOC来完成依赖注入的。

## 1.2 Spring bean的 Scope

**1.Spring中的bean是根据scope来生成的，表示bean的作用域。**

###  **scope有4种类型：**

1.singleton：单例，表示通过Spring容器获取的该对象是唯一的。

2.prototype：原型，表示通过Spring容器获取的对象都是不同的。

3.reqeust：请求，表示在一次http请求内有效。

4.session：会话，表示在一个用户会话内有效。

- 3和4只适用于web项目，大多数情况下，我们只会使用singleton和prototype两种scope，并且scope的默认值是singleton。 

 **默认通过Spring容器创建的对象都是单例模式。** 

```xml
 <bean id="user" class="com.southwind.entity.User" scope="prototype">
        <property name="id" value="1"></property>
        <property name="name" value="张三"></property>
        <property name="age" value="23"></property>
     </bean>
```



**2.Spring的继承，与Java的继承不一样，但思想很相似，子bean可以继承父bean中的属性。** 

```xml

     <bean id="user" class="com.southwind.entity.User">
        <property name="id" value="1"></property>
        <property name="name" value="张三"></property>
        <property name="age" value="23"></property>
     </bean>

     <bean id="user2" class="com.southwind.entity.User" parent="user">
         <!-- 覆盖name属性 -->
        <property name="name" value="李四"></property>
    </bean>
```

Spring中的bean能不能在不同类之间继承？

答案是可以，但是需要这两个类的属性列表完全一致，否则会报错，实际开发中并不会使用到这种方式。

### **3.Spring的依赖。** 

依赖也是bean和bean之间的一种关联方式，配置依赖关系后，被依赖的bean一定先创建，再创建依赖的bean。 

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

     <bean id="user" class="com.southwind.entity.User">
        <property name="id" value="1"></property>
        <property name="name" value="张三"></property>
        <property name="age" value="23"></property>
     </bean>

     <bean id="car" class="com.southwind.entity.Car">
        <property name="id" value="1"></property>
        <property name="brand" value="宝马"></property>
     </bean>

</beans>
```

3.测试类中获取两个bean 

```
ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
        User user = (User) applicationContext.getBean("user");
        Car car = (Car) applicationContext.getBean("car");
```

  结果，先创建User，后创建Car，这是由spring.xml中bean的配置顺序来决定的，先到先得，先配置User bean，所以先创建了User对象。 

- 现在修改spring.xml配置，User依赖Car，设置depends-on属性。 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

     <bean id="user" class="com.southwind.entity.User" depends-on="car">
        <property name="id" value="1"></property>
        <property name="name" value="张三"></property>
        <property name="age" value="23"></property>
     </bean>

     <bean id="car" class="com.southwind.entity.Car">
        <property name="id" value="1"></property>
        <property name="brand" value="宝马"></property>
     </bean>

</beans>
```

结果， 先创建Car，后创建User，因为User依赖于Car，所以必须先创建Car对象，User对象才能完成依赖。

###  **4.Spring读取外部资源 --JDBC

​    开发中，数据库的配置会保存在一个properties文件中，便于维护，如果使用Spring容器来生成数据源对象，如何读取到properties配置文件中的内容？

1.创建jdbc.properties。

  ```properties
driverName = com.mysql.jdbc.Driver
url = jdbc:mysql://localhost:3306/myTest?useUnicode=true&characterEncoding=UTF-8
user = root
pwd = 123456
  ```

2.spring.xml中配置C3P0数据源。 

  ```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd">

     <!-- 导入外部的资源文件 -->
     <context:property-placeholder location="classpath:jdbc.properties"></context:property-placeholder>

     <!-- 创建C3P0数据源 -->
     <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="user" value="${user}"></property>
        <property name="password" value="${pwd}"></property>
        <property name="driverClass" value="${driverName}"></property>
        <property name="jdbcUrl" value="${url}"></property>
     </bean>

</beans>
  ```

第一步：导入外部资源文件。

使用context:property-placeholder标签，需要导入context命名空间。

第二步：通过${}表达式取出外部资源文件的值

3.测试类中获取Spring创建的dataSource对象。

```
public class Test {
    public static void main(String[] args) {
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
        DataSource ds = (DataSource) applicationContext.getBean("dataSource");
        Connection conn = null;
        try {
            conn = ds.getConnection();
        } catch (SQLException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        System.out.println(conn);
    }
}
```

### 5.  p命名空间的使用 

除了使用<property> 元素为Bean 的属性装配值和引用外，Spring 还提供了另外一种bean属性的装配方式：p命名空间，该方式进一步简化配置代码。

- spring.xml中创建User bean和Car bean，并且通过p命名空间给属性赋值，同时完成依赖注入，注意需要引入p命名间。

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xmlns:p="http://www.springframework.org/schema/p"
    xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd">

     <bean id="user" class="com.southwind.entity.User" p:id="1" p:name="张三" p:age="23" p:car-ref="car"></bean>
     <bean id="car" class="com.southwind.entity.Car" p:id="1" p:brand="宝马"></bean>

</beans>
```

##   1.3 通过ioc创建对象

#### 1.IOC通过工厂方法创建对象

##### 学习静态工厂方法。

1.创建Car实体类

  2.创建静态工厂类，静态工厂方法。

```
public class StaticCarFactory {
    private static Map<Integer,Car> cars;
    static{
        cars = new HashMap<Integer,Car>();
        cars.put(1, new Car(1,"奥迪"));
        cars.put(2, new Car(2,"奥拓"));
    }
    public static Car getCar(int num){
        return cars.get(id);
    }
}
```

3.在spring.xml中配置静态工厂。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- 配置静态工厂创建car对象 -->
    <bean id="car1" class="com.southwind.entity.StaticCarFactory" factory-method="getCar">
        <constructor-arg value="1"></constructor-arg>
    </bean>

</beans>
```

 

##### 2.实例工厂方法

1.创建实例工厂类，工厂方法 。  

```
public class InstanceCarFactory {
    private Map<Integer,Car> cars;
    public InstanceCarFactory() {
        cars = new HashMap<Integer,Car>();
        cars.put(1, new Car(1,"奥迪"));
        cars.put(2, new Car(2,"奥拓"));
    }
    public Car getCar(int num){
        return cars.get(num);
    }
}
```

2.spring.xml中配置bean。

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- 配置实例工厂对象 -->
    <bean id="carFactory" class="com.southwind.entity.InstanceCarFactory"></bean>

    <!-- 通过实例工厂对象创建car对象 -->
    <bean id="car2" factory-bean="carFactory" factory-method="getCar">
        <constructor-arg value="2"></constructor-arg>
    </bean> 

</beans>
```

3.在测试类中直接获取car2对象。

```
public class Test {
    public static void main(String[] args) throws SQLException {
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
        Car car2 = (Car) applicationContext.getBean("car2");
        System.out.println(car2);
    }
}
```

​    对比两种方式的区别，静态工厂方法的方式创建car对象，不需要实例化工厂对象，因为静态工厂的静态方法，不需要创建对象即可调用。所以spring.xml只需要配置一个Car bean，而不需要配置工厂bean。

实例工厂方法创建car对象，必须先实例化工厂对象，因为调用的是非静态方法，必须通过对象调用，不能直接通过类来调用，所以spring.xml中需要先配置工厂bean，再配置Car bean。

其实这里考察的就是静态方法和非静态方法的调用。

#### 2.IOC自动装载（autowire）

​    Spring还提供了另外一种更加简便的方式：自动装载，不需要手动配置property，IOC容器会自动选择bean完成依赖注入。 

 自动装载有两种方式：

byName：通过属性名自动装载

byType：通过属性对应的数据类型自动装载

1,.创建Person实体类。

```
public class Person {
    private int id;
    private String name;
    private Car car;
    public int getId() {
        return id;
    }
    public void setId(int id) {
        this.id = id;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public Car getCar() {
        return car;
    }
    public void setCar(Car car) {
        this.car = car;
    }
    @Override
    public String toString() {
        return "Person [id=" + id + ", name=" + name + ", car=" + car + "]";
    }

}
```

2.spring.xml中配置Car bean和Person bean，并通过自动装载进行依赖注入。

创建person对象时，没有在property中配置car属性，所以IOC容器会自动进行装载，autowire="byName"表示通过匹配属性名的方式去装载对应的bean，Person实体类中有car属性，所以就将id="car"的bean注入到person中。

注意：通过property标签手动进行car的注入优先级更高，若两种方式同时配置，以property的配置为准。

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="person" class="com.southwind.entity.Person" autowire="byName"> 
        <property name="id" value="1"></property>
        <property name="name" value="张三"></property>
    </bean>

    <bean id="car" class="com.southwind.entity.StaticCarFactory" factory-method="getCar">
        <constructor-arg value="2"></constructor-arg>
    </bean>

</beans>
```

3.测试类中获取person对象。

```
public class Test {
    public static void main(String[] args) throws SQLException {
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
        Person person = (Person) applicationContext.getBean("person");
        System.out.println(person);
    }
}
```



**同理，byType即通过属性的数据类型来配置。**

1.spring.xml配置如下。

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="person" class="com.southwind.entity.Person" autowire="byType"> 
        <property name="id" value="1"></property>
        <property name="name" value="张三"></property>
    </bean>

    <bean id="car" class="com.southwind.entity.StaticCarFactory" factory-method="getCar">
        <constructor-arg value="1"></constructor-arg>
    </bean>

    <bean id="car2" class="com.southwind.entity.StaticCarFactory" factory-method="getCar">
        <constructor-arg value="2"></constructor-arg>
    </bean>
</beans>
```

2.测试类中获取person对象。

```
public class Test {
    public static void main(String[] args) throws SQLException {
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
        Person person = (Person) applicationContext.getBean("person");
        System.out.println(person);
    }
}
```

​        控制台直接打印错误信息，这是因为使用了byType进行自动装载，但是spring.xml中配置了两个Car bean，所以IOC容器不知道应该将哪一个bean装载到person对象中，所以报错。使用byType进行自动装载时，spring.xml中只能配置一个装载的bean。 

----

#### 通过IOC容器架构程序的分层。

实现有两种方式：基于xml配置文件，基于注解。

思路：

我们把程序分为3层：Controller层，Service层，DAO层。

关系为Controller层调用Service层，Service层调用DAO层，并且Service层和DAO层设计为接口，这是一个典型的MVC模式后台代码分层结构。

> 代码：

基于xml配置方式：

1.创建UserController类。  

```
public class UserController {

    private UserService userService;

    public UserService getUserService() {
        return userService;
    }

    public void setUserService(UserService userService) {
        this.userService = userService;
    }

    public User getUserById(int id){
        return userService.getUserById(id);
    }
}
```

2.创建UserService接口以及实现类UserServiceImpl。

```
public interface UserService {
    public User getUserById(int id);
}
```

```
public class UserServiceImpl implements UserService{

    private UserDAO userDAO;

    public UserDAO getUserDAO() {
        return userDAO;
    }

    public void setUserDAO(UserDAO userDAO) {
        this.userDAO = userDAO;
    }

    @Override
    public User getUserById(int id) {
        // TODO Auto-generated method stub
        return userDAO.getUserById(id);
    }

}
```

3.创建UserDAO接口以及实现类UserDAOImpl。

```
public interface UserDAO {
    public User getUserById(int id);
}
```

```
public class UserDAOImpl implements UserDAO{

    private static Map<Integer,User> users;

    static{
        users = new HashMap<Integer,User>();
        users.put(1, new User(1, "张三"));
        users.put(2, new User(2, "李四"));
        users.put(3, new User(3, "王五"));
    }

    @Override
    public User getUserById(int id) {
        // TODO Auto-generated method stub
        return users.get(id);
    }

}
```

4.创建User实体类。

```
public class User {
    private int id;
    private String name;
    public int getId() {
        return id;
    }
    public void setId(int id) {
        this.id = id;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public User(int id, String name) {
        super();
        this.id = id;
        this.name = name;
    }
    public User() {
        super();
    }
    @Override
    public String toString() {
        return "User [id=" + id + ", name=" + name + "]";
    }

}
```

5.在spring.xml配置userController，userService，userDAO，并完成依赖注入。

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- 配置UserController -->
    <bean id="userController" class="com.southwind.controller.UserController">
        <property name="userService" ref="userService"></property>
    </bean>

    <!-- 配置UserService -->
    <bean id="userService" class="com.southwind.service.impl.UserServiceImpl">
        <property name="userDAO" ref="userDAO"></property>
    </bean>

    <!-- 配置UserDAO -->
    <bean id="userDAO" class="com.southwind.dao.impl.UserDAOImpl"></bean>


</beans>
```

6.在测试类中获取userController对象，调用方法获取user对象。

```
public class Test {
    public static void main(String[] args) {
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
        UserController userController = (UserController) applicationContext.getBean("userController");
        User user = userController.getUserById(1);
        System.out.println(user);
    }
}
```

#### 基于注解的方式：

第一步：将UserController，UserService，UserDAO类扫描到IOC容器中。

第二步：在类中设置注解完成依赖注入。

1.修改spring.xml。        

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd">

    <!-- 将类扫描到IOC容器中 -->
    <context:component-scan base-package="com.southwind"></context:component-scan>

</beans>
```

base-package="com.southwind"表示将com.southwind下所有子包的类全部扫描到IOC容器中，一步可将所有参与项目的类完成扫描注入。

注意：需要引入context命名空间。

2.修改UserController，添加注解。

```
@Controller
public class UserController {

    @Autowired
    private UserService userService;

    public User getUserById(int id){
        return userService.getUserById(id);
    }
}
```

对比之前的代码，有两处改动：

（1）在类名处添加@Controller注解，表示该类作为一个控制器。

（2）userService属性出添加@Autowired注解，表示IOC容器自动完成装载，默认是byType的方式。

3.修改UserServiceImpl。

```
@Service
public class UserServiceImpl implements UserService{

    @Autowired
    private UserDAO userDAO;

    @Override
    public User getUserById(int id) {
        // TODO Auto-generated method stub
        return userDAO.getUserById(id);
    }

}
```

同上，做了两处改动：

（1）在类名处添加@Service注解，表示该类是业务层。

（2）userDAO属性出添加@Autowired注解，表示IOC容器自动完成装载，默认是byType的方式。

4.修改UserDAOImpl。

```
@Repository
public class UserDAOImpl implements UserDAO{

    private static Map<Integer,User> users;

    static{
        users = new HashMap<Integer,User>();
        users.put(1, new User(1, "张三"));
        users.put(2, new User(2, "李四"));
        users.put(3, new User(3, "王五"));
    }

    @Override
    public User getUserById(int id) {
        // TODO Auto-generated method stub
        return users.get(id);
    }

}
```

做了一处改动：在类名处添加@Repository注解，表示该类是数据接口层。

​    

5.运行测试代码。

```
public class Test {
    public static void main(String[] args) {
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
        UserController userController = (UserController) applicationContext.getBean("userController");
        User user = userController.getUserById(1);
        System.out.println(user);
    }
}
```

 

成功，通过代码可以看到，使用注解的方式，可简化代码，所以实际开发中，推荐使用基于注解的方式来架构分层。

我们分别给UserController，UserService，UserDAO添加了@Controller，@Service，@Repository注解。

IOC中可以给类添加的注解有4种：

1.@Controller  

2.@Service

3.@Repository

4.@Component

这四种注解方式没有区别，我们在开发时可以随意使用任意一个注解，但是基于代码规范我们一般选择使用@Controller，@Service，@Repository分别表示Controller层，Service层，DAO层。

前面我们提到过，类中属性自动装载，默认是通过byType的方式。

自动装载除了byType的方式，还可以使用byName的方式，使用byName的方式，需要结合@Qualifier注解一起使用。

```
@Controller
public class UserController {

    @Autowired()
    @Qualifier("userService")
    private UserService userService;

    public User getUserById(int id){
        return userService.getUserById(id);
    }
}
```

我们知道byName的方式，是通过属性名去匹配对应bean的id属性值，但是基于注解的方式我们并没有给bean设置id，如何完成呢？

其实我们在类中添加注解时，已经设置了默认的id，即类名首字母小写之后的值就是id的默认值。

```
@Service
public class UserServiceImpl implements UserService
```

此时，IOC容器中默认赋值，UserService bean的id=userService，与UserController中的属性名一致，所以可以完成自动。

现在做出修改，手动赋值，设置UserService bean的id=myUserService。

```
@Service("myUserService")
public class UserServiceImpl implements UserService{

    @Autowired
    private UserDAO userDAO;

    @Override
    public User getUserById(int id) {
        // TODO Auto-generated method stub
        return userDAO.getUserById(id);
    }

}
```

很显然，UserController中的userService属性也需要去匹配name=myUserService的bean，所以设置@Qualifier（"myUserService"）。

```
@Controller
public class UserController {

    @Autowired()
    @Qualifier("myUserService")
    private UserService userService;

    public User getUserById(int id){
        return userService.getUserById(id);
    }
}
```

@Qualifier（）中的值必须与@Service（）中的值一致，才能完成自动装载。

 

------

##  代码实例：

#### spring 注入方法

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 

	  1.xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	    new
	  2.xsi:schemaLocation="http://www.springframework.org/schema/beans
       	http://www.springframework.org/schema/beans/spring-beans-4.0.xsd"
       	 类名    对象 Object
      3.xmlns="http://www.springframework.org/schema/beans"
                 别名  变量名称
       
       Object object =  new Object();
       object.属性 方法
 -->
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-4.0.xsd">

		<!-- 
			bean元素表示控制反转  实例化对象
			bean元素下的子元素     依赖注入   赋值
		 -->

        <!-- 
        	1.setter注入
         -->
		<bean id="stuSetter" class="com.etoak.bean.Student">
			
			<property name="id" value="1"></property>
			<property name="name" value="etoak"></property>
			<property name="age" value="10"></property>
			
		</bean>
		
		<!-- 2.构造方法 参数名称 注入-->
		<bean id="stuName" class="com.etoak.bean.Student">
		
			<constructor-arg name="id" value="2"></constructor-arg>
			<constructor-arg name="name" value="易途"></constructor-arg>
			<constructor-arg name="age" value="10"></constructor-arg>
		
		</bean>

        <!-- 2.构造方法 参数下标 注入  参数下标从0开始 -->
		<bean id="stuIndex" class="com.etoak.bean.Student">
			
			<constructor-arg index="0" value="3"></constructor-arg>
			<constructor-arg index="1" value="et1803"></constructor-arg>
			<constructor-arg index="2" value="5"></constructor-arg>
		
		</bean>
		
		<!-- 3.普通方法注入  工厂 -->
		<bean id="stuFactoryMethod" 
			  class="com.etoak.bean.ClassRoom">
		
		    <!-- 
		    	name属性表示当前bean元素的class属性类中的方法 
		    	bean属性表示引入某个bean元素id属性值
		    -->
			<lookup-method name="getStu" bean="stuSetter" />
		
		</bean>
		
</beans>
```

#### Spring 作用域

>	spring作用域 
>			     1.singleton 单例  默认 
>			     	始终保持一个对象
>			     	容器启动时就实例化对象
>			     2.prototype 非单例
>			                 每次都会创建一个新的对象
>			                容器启动时不实例化对象
>			                当调用getBean方法时，才会实例化对象

####  代码实例：

````xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-4.0.xsd">

		<!-- 
			spring作用域 
			     1.singleton 单例  默认 
			     	始终保持一个对象
			     	容器启动时就实例化对象
			     2.prototype 非单例
			                 每次都会创建一个新的对象
			                容器启动时不实例化对象
			                当调用getBean方法时，才会实例化对象
		 -->
	
	     <bean id="stuSingleton" 
	           class="com.etoak.bean.Student" 
	           scope="singleton" />
	           
	      <bean id="stuProtoType" 
                class="com.etoak.bean.Student"
                scope="prototype" />		
</beans>
````

#### 集合类型，引用类型的 注入

配置：

````xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-4.0.xsd">


	<bean id="student" class="com.etoak.bean.Student">
	
		<property name="id" value="1"></property>
		
		<property name="name">
			<value>etoak</value>
		</property>
		
		<property name="age">
			<value>10</value>
		</property>
	
	</bean>

	
	<bean id="ob" class="com.etoak.bean.ObjectBean">
	    <!-- list集合注入 -->
		<property name="lists">
			<list>
				<value>北京</value>
				<value>上海</value>
				<value>天津</value>
				<value>重庆</value>
			</list>
		</property>
		
		<!-- set集合注入 -->
		<property name="sets">
			<set>
				<value>etoak</value>
				<value>et</value>
				<value>易途</value>
				<value>et1803</value>
			</set>
		</property>
		
		<!-- map集合注入 -->
		<property name="maps">
			<map>
				<entry key="etoak" value="易途"></entry>
				<entry key="et" value="et1803"></entry>
			</map>
		</property>
		
		<!-- properties集合注入  -->
		<property name="props">
			<props>
				<prop key="et">etoak</prop>
				<prop key="etoak">易途</prop>
			</props>
		</property>
		
		<!-- 
			注入student对象 
			ref属性表示引入某个bean元素的id属性值
			
			ref
				对象 Object
			value
			   基本数据类型  int
			   包装数据类型  Integer
			   字符串            String
			   日期类型        Date
			     spring默认 不处理日期类型 需要开发人员自定义日期类型  转换
		-->
		<property name="stu" ref="student"></property>
		<!-- <property name="stu">
			<ref bean="student" />
		</property> -->
		<!-- <property name="stu">
			<bean class="com.etoak.bean.Student">
	
				<property name="id" value="1"></property>
				
				<property name="name">
					<value>易途</value>
				</property>
				
				<property name="age">
					<value>10</value>
				</property>
            </bean>
		</property> -->
	</bean>
</beans>
````

pojo ：

```java
package com.etoak.bean;

import java.util.List;
import java.util.Map;
import java.util.Properties;
import java.util.Set;

public class ObjectBean {
	
	private List<String> lists;
	private Set<String> sets;
	private Map<String,Object> maps;
	private Properties props;
	
	private Student stu;
	
	public List<String> getLists() {
		return lists;
	}
	public void setLists(List<String> lists) {
		this.lists = lists;
	}
	public Set<String> getSets() {
		return sets;
	}
	public void setSets(Set<String> sets) {
		this.sets = sets;
	}
	public Map<String, Object> getMaps() {
		return maps;
	}
	public void setMaps(Map<String, Object> maps) {
		this.maps = maps;
	}
	public Properties getProps() {
		return props;
	}
	public void setProps(Properties props) {
		this.props = props;
	}
	public Student getStu() {
		return stu;
	}
	public void setStu(Student stu) {
		this.stu = stu;
	}
}

```

- 测试：

```java
//定义spring容器
	private ApplicationContext ac;
	//定义ObjectBean对象
	private ObjectBean ob;
	
	//定义初始化操作
	@Before
	public void before() {
		//创建spring容器 解析xml配置信息  
		//控制反转（实例化） 依赖注入（赋值）
		ac = 
		new ClassPathXmlApplicationContext("applicationContext.xml");
		
		//获取实例化对象
		ob = ac.getBean(ObjectBean.class);
	}
	
	//list集合注入
	@Test
	public void lists() {
		List<String> list = ob.getLists();
		System.out.println(list);
	}
	
	//set集合注入
	@Test
	public void sets() {
		Set<String> set = ob.getSets();
		System.out.println(set);
	}
	
	//map集合注入
	@Test
	public void maps() {
		Map<String,Object> map = ob.getMaps();
		System.out.println(map);
	}
	
	//properties集合注入
	@Test
	public void props() {
		Properties p = ob.getProps();
		System.out.println(p);
	}
	
	//注入对象
	@Test
	public void stu() {
		Student stu = ob.getStu();
		System.out.println(stu.getId());
		System.out.println(stu.getName());
		System.out.println(stu.getAge());
	}
```

####  配置文件配置  各种方法的设置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-4.0.xsd">
	
	<!-- 
		init-method    初始化方法 自定义方法名称时  方法不需要定义参数
		destroy-method 销毁方法 自定义方法名称时  方法不需要定义参数
		                                 容器关闭时 会触发销毁方法
		                                 
		lazy-init      懒加载 默认default false
		
		       true懒加载 
		                 当用到（调用）这个方法的时候才会被加载
		                 容器启动时不会实例化对象、也不会注入
		       false 容器启动时，实例化对象、注入
		       
		name和id属性类似     name属性可以设置多个别名
		                                    例如 name="student,stu"
		                                    
		abstract设置当前bean元素为抽象类
		parent继承某个bean元素  通常abstract属性和parent属性组合
	 -->
	<bean id="student"
		  class="com.etoak.bean.Student"
	      init-method="init"
	      destroy-method="destroy"
	      lazy-init="false"
	      abstract="true">
	
		<property name="name" value="etoak"></property>
	
	</bean>
	
	<!-- 当前bean继承student -->
	<bean id="stu" parent="student">
		<property name="name" value="et1803"></property>
	</bean>
	
	
	
	
	<!-- 
		静态工厂
		类名.静态方法名称();
		
		class属性和factory-method属性组合时
		表示的就是静态工厂
	 -->
	<bean id="staticParam" class="com.etoak.bean.Student"
	            factory-method="staticParam">
		
		<constructor-arg name="str" 
					value="我是小明，我要去上学！"></constructor-arg>
		
	</bean>
	
	<!-- 工厂 -->
	
	<!-- 
		实例工厂
		
		Object object = new Object();
		object.方法名称();
		
		 factory-bean属性和factory-method属性组合时
		  表示是一个实例工厂
		  
		  factory-bean引入某个bean元素id属性值
		  factory-method调用实例对象中的某个方法名称
	 -->
	 
	 <bean id="stuInstance" class="com.etoak.bean.Student" />
	
	
	 <bean id="instanceParam"
	       factory-bean="stuInstance"
	       factory-method="instanceParam">
	 
	 	<constructor-arg name="str"
	 	                value="你瞅啥，瞅你咋地！"></constructor-arg>
	 </bean>
</beans>
```

- pojo

```java
package com.etoak.bean;

public class Student {
	
	private String name;
	
	public Student() {
		System.out.println("构造方法：" + this);
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
		System.out.println("setter注入：" + name);
	}
	
	//初始化
	public void init() {
		System.out.println("初始化方法！" + getName());
	}
	
	//销毁
	public void destroy() {
		System.out.println("销毁方法！");
	}
	
	//定义静态方法   类名.静态方法
	public static String staticParam(String str) {
		return str;
	}
	
	//定义非静态方法
	//Object object = new Object();
	//object.方法名称();
	public String instanceParam(String str) {
		return str;
	}
}
```

- 调用

```java
public static void main(String[] args) {
		//spring容器
		ApplicationContext ac = 
		new ClassPathXmlApplicationContext("applicationContext.xml");

		//父类引用指向子类对象
		//左属性 右方法
		
		/*父类 
		int i = 1;
		public void test1() {
			System.out.println("test1");
		}
		
		子类继承父类
		int i = 2;
		public void test1() {
			System.out.println("test2");
		}
		
		左属性 右方法
		父类 引用 = new 子类（）；
		引用.i = 1
		引用.test1(); test2*/
		
		//默认启动容器
		((ClassPathXmlApplicationContext)ac).start();
		
		//获取实例化对象
		Student stu = (Student)ac.getBean("stu");
		
		//输出信息
		System.out.println(stu.getName());
	     System.out.println("------------------------------");
		
		//工厂
		//调用的是静态方法 staticParam();
		String str = (String)ac.getBean("staticParam");
		System.out.println("静态工厂：" + str);
		
		//调用的是非静态方法instanceParam();
		str = (String)ac.getBean("instanceParam");
		System.out.println("实例工厂：" + str);
        //关闭容器时触发destroy方法 销毁方法
		((ClassPathXmlApplicationContext)ac).close();	
	}
```

#### Spring  基本注解

##### **注解总结**

  扫描ioc注解
   	 @Controller @Service @Repository  实例化
   	 @Autowired @Resource 注入

​     component-scan 扫描  当前工程包结构、jar包
     <context:component-scan base-package="com.etoak" />

- @Controller 应用在controller层   处理请求、响应

​    实例化对象  默认id属性别名为：类名首字母小写

<bean id="loginController"  class="com.etoak.controller.LoginController" />

- @Autowired 注入



- @Service 应用在servie层 处理业务逻辑

​     实例化对象 默认id属性别名为：类名首字母小写

​     <bean id="loginServiceImpl"  class="com.etoak.service.impl.LoginServiceImpl" />

- @Autowired匹配类型  只找类名 不找别名（id）

		       @Qualifier("loginDaoImpl")
	 	@Autowired 
	   **@Autowired 和 @Qualifier组合使用 只找别名**
	
		   **@Resource 注入  jdk**
		 
		  **注意事项 ： 在使用@Autowired注解时 当前类必须先实例化 才能注入**
	   LoginDao dao = new LoginDaoImpl();
	   LoginDao dao = new LoginDaoImpl2();
	   @Resource(name="loginDaoImpl")
	    只找别名，别名没有找到，抛异常
	   @Resource(type=LoginDao.class)
	    只找类型，类型没有找到，抛异常
	   @Resource(name="loginDao2" ,type=LoginDao.class)
	   **错一个都不行** 

- @Qualifier("loginDaoImpl")

		 @Autowired 

- @Repository 应用在dao层  sql语句 

  ​	实例化对象

  ​	默认id属性别名为：类名首字母小写

  ​	<bean id="loginDaoImpl" class="com.etoak.dao.impl.LoginDaoImpl" />

  ​	LoginDaoImpl dao = new LoginDaoImpl();

  ​	LoginDao dao = new LoginDaoImpl();

------



- xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context-4.0.xsd">

   <!-- 
   	扫描ioc注解
   	 @Controller @Service @Repository  实例化
   	 @Autowired @Resource 注入
   	 
   	 递推  File
   	 ClassLoader
   	 
   	 component-scan 扫描  当前工程包结构、jar包
    -->
	<context:component-scan base-package="com.etoak" />
	
	
<!-- 	<bean id="dao" class="com.etoak.dao.impl.LoginDaoImpl">
	
	</bean>
	
	<bean id="service" class="com.etoak.service.impl.LoginServiceImpl">
		<property name="loginDao" ref="dao"></property>
	</bean>
	
	<bean id="controller" class="com.etoak.controller.LoginController">
		<property name="loginService" ref="service"></property>
	</bean>
 -->
 <bean        id="newFixedThreadPool"
 			  class="java.util.concurrent.Executors"
              factory-method="newFixedThreadPool">
 	<property name="nThreads" value="10"></property>
 </bean>
</beans>
```

- pojo

###### controller

```java
package com.etoak.controller;

import java.util.concurrent.ExecutorService;

import javax.annotation.Resource;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;

import com.etoak.service.LoginService;

/**
 *  @Controller 应用在controller层   处理请求、响应
 *    实例化对象
 *    默认id属性别名为：类名首字母小写
 *    
 *   <bean id="loginController" 
 *        class="com.etoak.controller.LoginController" />
 */
@Controller("loginController")
public class LoginController {
	
	@Resource(name="newFixedThreadPool")
	private ExecutorService es;
	
	/**
	 * 	@Autowired 注入
	 */
	@Autowired
	private LoginService loginService;
	
	public String add(String username,String password) {
		
	/*	es.submit(new Runnable() {
			
			@Override
			public void run() {
				System.out.println("sadfsadsdf");
				
			}
		});*/
		
		String result = null;
		
		try {
			//controller层调用service层
			result = loginService.add(username, password);
		} catch(RuntimeException e) {
			e.printStackTrace();
			
			//获取service层 抛出的异常信息
			result = e.getMessage();
		}
				
		return result;
	}
}
```

###### Service

```java
package com.etoak.service;

public interface LoginService {
	
	public String add(String username,String password);

}
--------------------------------------------------------------
/**
 * @Service 应用在servie层 处理业务逻辑
 *         实例化对象
 *         默认id属性别名为：类名首字母小写
 *  
 * <bean id="loginServiceImpl" 
 * 			class="com.etoak.service.impl.LoginServiceImpl" />
 */
@Service("loginService")
public class LoginServiceImpl implements LoginService {
	
	/**
	 * service层调用dao层
	 * 
	 * @Autowired匹配类型  只找类名 不找别名（id）
	 * 
	 * 	@Qualifier("loginDaoImpl")
	 *	@Autowired 
	 *  @Autowired 和 @Qualifier组合使用 只找别名
	 *  
	 *  @Resource 注入  jdk
	 * 
	 *  注意事项 ： 在使用@Autowired注解时 当前类必须先实例化 才能注入
	 *  
	 *  
	 *  
	 *  LoginDao dao = new LoginDaoImpl();
	 *  LoginDao dao = new LoginDaoImpl2();
	 *  
	 *  
	 *  @Resource(name="loginDaoImpl")
	 *   只找别名，别名没有找到，抛异常
	 *  @Resource(type=LoginDao.class)
	 *   只找类型，类型没有找到，抛异常
	 *  @Resource(name="loginDao2" ,type=LoginDao.class)
	 *  错一个都不行
	 */
	/*@Qualifier("loginDaoImpl")
	@Autowired */
    
	@Resource
	private LoginDaoImpl loginDao;

	@Override
	public String add(String username, String password) {
		
		if(username == null || "".equals(username)) {
			throw new RuntimeException("用户名不能空！");
		}
		
		if(password == null || "".equals(password)) {
			throw new RuntimeException("密码不能空！");
		}
		
		//service层调用dao层
		//dao层将结果返回给service层
		String result = loginDao.add(username, password);
	
		//service层将结果返回给controller层
		return result;
	}

```

###### Dao

```java
package com.etoak.dao;

public interface LoginDao {
	
	public String add(String username,String password);

}
---------------------------------------------------------------
    
package com.etoak.dao.impl;

import org.springframework.stereotype.Repository;

import com.etoak.dao.LoginDao;

/**
 * @Repository 应用在dao层  sql语句 
 * 		实例化对象
 *      默认id属性别名为：类名首字母小写
 *  <bean id="loginDaoImpl" class="com.etoak.dao.impl.LoginDaoImpl" />
 * 
 *  LoginDaoImpl dao = new LoginDaoImpl();
 *  LoginDao dao = new LoginDaoImpl();
 */
@Repository("loginDao")
public class LoginDaoImpl implements LoginDao {

	@Override
	public String add(String username, String password) {
		
		System.out.println("用户名：" + username);
		System.out.println("密码：" + password);
		
		return username + "|" + password;
	
	}

}
-------------------------------------------------------------------------
 package com.etoak.dao.impl;

import org.springframework.stereotype.Repository;

import com.etoak.dao.LoginDao;

/**
 * @Repository 应用在dao层  sql语句 
 * 		实例化对象
 *      默认id属性别名为：类名首字母小写
 *  <bean id="loginDaoImpl" class="com.etoak.dao.impl.LoginDaoImpl" />
 * 
 *  LoginDaoImpl dao = new LoginDaoImpl();
 *  LoginDao dao = new LoginDaoImpl();
 */
@Repository("loginDao2")
public class LoginDaoImpl2 implements LoginDao {

	@Override
	public String add(String username, String password) {
		
		System.out.println("用户名2：" + username);
		System.out.println("密码2：" + password);
		
		return username + "|" + password;
		
	}

}


```

###### Test

```java
public static void main(String[] args) {
		
		ApplicationContext ac = 
		new ClassPathXmlApplicationContext("applicationContext.xml");
		
		LoginController lc = 
				(LoginController)ac.getBean("loginController");
		
		String result = lc.add("etoak", "et1803");
		
		System.out.println(result);
		
	}
```

--------------------

##  2.0  Spring   aop 简介

#### aop相关

```
ioc   实例化 赋值
aop 动态代理设计模式  反射   jdk  cglib
   面向切面对象
    全局
   oop  面向对象  局部
        分散
   过滤器 拦截器 权限
     异常
  	 事务
   before   开启事务
   test      insert update delete
   after     提交 或 回滚
          
	before   开启事务
     test
	
     test
 	after     提交 或 回滚
```

#### aop简介

```
 aop
        分2部分

        非核心部分  通知
           异常、日志、事务、过滤器 、权限
           缓存 

            事务 线程 锁 线程池  防止并发
            写 insert update delete
            读 select

        核心部分 切入点
             增删改查
  
     目标对象
	LoginController  交给第三方代理
                         jdk cglib 反射
                         被代理对象
     切入点

	核心部分LoginController类下所有的方法

        定义连接点方法（切入点）

     通知/增强
         非核心部分
          通知/增强 4种
          前置通知、后置通知、环绕通知、异常通知

     连接点
	  
 	底层连接点
		 	 
	通过连接点将通知和切入点
	（织入）组装在一起  就一个完整的aop 
		 	 
	aop = 通知 + 切入点
     before方法
     login方法

     原理：
          aop分2部分，非核心和核心，
		通过动态代理设计模式（反射）将非核心封装成		aop组件，通过连接点将通知和切入点
	       （织入）组装在一起  就一个完整的aop 
```

 AOP是Spring框架除了IOC之外的另一个核心概念。

AOP：Aspect Oriented Programming，意为面向切面编程。

这是一个新的概念，我们知道Java是面向对象编程（OOP）：指将所有的一切都看做对象，通过对象与对象之间相互作用来解决问题的一种编程思想。

AOP是对OOP的一个补充，在运行时，动态地将代码切入到类的指定方法、指定位置上的编程思想就是面向切面的编程。将不同方法的同一位置抽象成一个切面对象，对该切面对象进行编程就是AOP。

![img](https://mmbiz.qpic.cn/mmbiz_png/t4kgaKYXPLexsDu1uv0NHlYnnW90Fq0l4CA6Bb7IbRTrDeDoCT58DGIQ9BH2ibKewbez7JGMrRGqib6OIzibJASlQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

AOP的优点：

```
1.降低模块之间的耦合度。
2.使系统容易扩展。
3.更好的代码复用。
4.非业务代码更加集中，不分散，便于统一管理。
5.业务代码更简洁纯粹，没有其他代码的影响。
```

说概念太空泛，不好理解，我们还是通过代码来直观感受什么是AOP。

#### 代码示例1：

1.创建一个计算器接口 Cal。

定义四个方法：加，减，乘，除。

```java
public interface Cal {
    public int add(int num1,int num2);
    public int sub(int num1,int num2);
    public int mul(int num1,int num2);
    public int div(int num1,int num2);
}
```

2.创建接口实现类CalImpl。

实现四个方法。

```java
public class CalImpl implements Cal{

    @Override
    public int add(int num1, int num2) {
        // TODO Auto-generated method stub
        int result = num1+num2;
        return result;
    }

    @Override
    public int sub(int num1, int num2) {
        // TODO Auto-generated method stub
        int result = num1-num2;
        return result;
    }

    @Override
    public int mul(int num1, int num2) {
        // TODO Auto-generated method stub
        int result = num1*num2;
        return result;
    }

    @Override
    public int div(int num1, int num2) {
        // TODO Auto-generated method stub
        int result = num1/num2;
        return result;
    }

}
```

3.在测试方法中创建CalImpl对象，调用方法。

```java
public class Test {
    public static void main(String[] args) {
        Cal cal = new CalImpl();
        cal.add(10, 3);
        cal.sub(10, 3);
        cal.mul(10, 3);
        cal.div(10, 3);
    }
}
```

以上这段代码很简单，现添加功能：在每一个方法执行的同时，打印日志信息：该方法的参数列表和该方法的计算结果。

这个需求很简单，我们只需要在每一个方法体中，运算执行之前打印参数列表，运算结束之后打印计算结果即可，对代码做出如下修改。

```java
public class CalImpl implements Cal{

    @Override
    public int add(int num1, int num2) {
        // TODO Auto-generated method stub
        System.out.println("add方法的参数是["+num1+","+num2+"]");
        int result = num1+num2;
        System.out.println("add方法的结果是"+result);
        return result;
    }

    @Override
    public int sub(int num1, int num2) {
        // TODO Auto-generated method stub
        System.out.println("sub方法的参数是["+num1+","+num2+"]");
        int result = num1-num2;
        System.out.println("sub方法的结果是"+result);
        return result;
    }

    @Override
    public int mul(int num1, int num2) {
        // TODO Auto-generated method stub
        System.out.println("mul方法的参数是["+num1+","+num2+"]");
        int result = num1*num2;
        System.out.println("mul方法的结果是"+result);
        return result;
    }

    @Override
    public int div(int num1, int num2) {
        // TODO Auto-generated method stub
        System.out.println("div方法的参数是["+num1+","+num2+"]");
        int result = num1/num2;
        System.out.println("div方法的结果是"+result);
        return result;
    }

}
```

再次运行代码，成功打印日志信息。

![img](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

功能已经实现了，但是我们会发现这种方式业务代码和打印日志代码的耦合性非常高，不利于代码后期的维护。

如果需求改变，需要对打印的日志内容作出修改，那么我们就必须修改4个方法中的所有相关代码，如果是100个方法呢？每次就需要手动去改100个方法中的代码。

换个角度去分析，会发现4个方法中打印日志信息的代码基本相同，那么有没有可能将这部分代码提取出来进行封装，统一维护呢？同时也可以将日志代码和业务代码完全分离开，解耦和。

按照这思路继续向下走，我们希望做的事情是把这4个方法的相同位置（业务方法执行前后）提取出来，形成一个横切面，并且将这个横切面封装成一个对象，将所有的打印日志代码写到这个对象中，以实现与业务代码的分离。

这就是AOP的思想。

如何实现？

使用动态代理的方式来实现。

我们希望CalImpl只进行业务运算，不进行打印日志的工作，那么就需要有一个对象来替代CalImpl进行打印日志的工作，这就是代理对象。

代理对象首先应该具备CalImpl的所有功能，并在此基础上，扩展出打印日志的功能。

1.删除CalImpl方法中所有的打印日志代码，只保留业务代码。  

   

```java
public class CalImpl implements Cal{

    @Override
    public int add(int num1, int num2) {
        // TODO Auto-generated method stub
        int result = num1+num2;
        return result;
    }

    @Override
    public int sub(int num1, int num2) {
        // TODO Auto-generated method stub
        int result = num1-num2;
        return result;
    }

    @Override
    public int mul(int num1, int num2) {
        // TODO Auto-generated method stub
        int result = num1*num2;
        return result;
    }
    @Override
    public int div(int num1, int num2) {
        // TODO Auto-generated method stub
        int result = num1/num2;
        return result;
    }

}
```

2.创建MyInvocationHandlerl类，并实现InvocationHandler接口，成为一个动态代理类。

```java
public class MyInvocationHandler implements InvocationHandler{
    //被代理对象
    private Object obj = null;

    //返回代理对象
    public Object bind(Object obj){
        this.obj = obj;
        return Proxy.newProxyInstance(obj.getClass().getClassLoader(), obj.getClass().getInterfaces(), this);
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args)
            throws Throwable {
        // TODO Auto-generated method stub
        System.out.println(method.getName()+"的参数是:"+Arrays.toString(args));
        Object result = method.invoke(this.obj, args);
        System.out.println(method.getName()+"的结果是:"+result);
        return result;
    }

}
```

bind方法是MyInvocationHandlerl类提供给外部调用的方法，传入需要被代理的对象，bind方法会返回一个代理对象。

bind方法完成了两项工作：

1.将外部传进来的被代理对象保存到成员变量中，因为业务方法调用时需要用到被代理对象。

2.通过Proxy.newProxyInstance方法创建一个代理对象。

解释一下Proxy.newProxyInstance方法的参数：

（1）我们知道对象是JVM根据运行时类来创建的，此时需要动态创建一个代理对象，可以使用被代理对象的运行时类来创建代理对象：obj.getClass().getClassLoader（）获取被代理对象的运行时类。

（2）同时代理对象需要具备被代理对象的所有功能，即需要拥有被代理对象的所有接口，所以传入obj.getClass().getInterfaces（）。

（3）this指当前MyInvocationHandler对象。

以上全部是反射（reflect）的知识点。

invoke方法：method是描述被代理对象的所有方法的对象，agrs是描述被代理对象方法的参数列表的对象。

method.invoke(this.obj, args)是通过反射机制来调用被代理对象的方法，即业务方法。

所以在method.invoke(this.obj, args)前后添加打印日志信息，就等同于在被代理对象的业务方法前后添加打印日志信息，并且已经做到了分类，业务方法在被代理对象中，打印日志信息在代理对象中。

测试方法中执行代码。

```java
public class Test {
    public static void main(String[] args) {
        //被代理对象
        Cal cal = new CalImpl();
        MyInvocationHandler mh = new MyInvocationHandler();
        //代理对象
        Cal cal2 = (Cal) mh.bind(cal);
        cal2.add(10, 3);
        cal2.sub(10, 3);
        cal2.mul(10, 3);
        cal2.div(10, 3);
    }
}
```

![img](https://mmbiz.qpic.cn/mmbiz_png/t4kgaKYXPLexsDu1uv0NHlYnnW90Fq0lw2wXzR2NYHa8DE7EPpYH8dib4TTTVDEX6XiaUM9YdjYru4G9wW4daRuQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

成功，并且现在已经做到了代码分离，CalImpl类中只有业务代码，打印日志的代码写在MyInvocationHandler类中。

以上就是通过动态代理实现AOP的过程，我们在使用Spring框架的AOP时，并不需要这么复杂，Spring已经对这个过程进行了封装，让开发者可以更加便捷的使用AOP进行开发。

接下来我们就来学习Spring框架的AOP如何使用。

在Spring框架中，我们不需要创建动态代理类，只需要创建一个切面类，Spring底层会自动根据切面类以及目标类生成一个代理对象。

1.创建切面类 LoggerAspect

```java
@Aspect
@Component
public class LoggerAspect {

    @Before("execution(public int com.southwind.aspect.CalImpl.*(..))")
    public void before(JoinPoint joinPoint){
        //获取方法名
        String name = joinPoint.getSignature().getName();
        //获取参数列表
        String args = Arrays.toString(joinPoint.getArgs());
        System.out.println(name+"的参数是:"+args);
    }

    @After("execution(public int com.southwind.aspect.CalImpl.*(..))")
    public void after(JoinPoint joinPoint){
        //获取方法名
        String name = joinPoint.getSignature().getName();
        System.out.println(name+"方法结束");
    }

    @AfterReturning(value="execution(public int com.southwind.aspect.CalImpl.*(..))",returning="result")
    public void afterReturn(JoinPoint joinPoint,Object result){
        //获取方法名
        String name = joinPoint.getSignature().getName();
        System.out.println(name+"方法的结果是"+result);
    }

    @AfterThrowing(value="execution(public int com.southwind.aspect.CalImpl.*(..))",throwing="ex")
    public void afterThrowing(JoinPoint joinPoint,Exception ex){
        //获取方法名
        String name = joinPoint.getSignature().getName();
        System.out.println(name+"方法抛出异常："+ex);
    }

}
```

LoggerAspect类名处添加两个注解：

1.@Aspect：表示该类是切面类。

2.@Component：将该类注入到IOC容器。

分别来说明类中的4个方法注解的含义。

```java
    @Before("execution(public int com.southwind.aspect.CalImpl.*(..))")
    public void before(JoinPoint joinPoint){
        //获取方法名
        String name = joinPoint.getSignature().getName();
        //获取参数列表
        String args = Arrays.toString(joinPoint.getArgs());
        System.out.println(name+"的参数是:"+args);
    }
```

1.@Before：表示before方法执行的时机。

2.execution(public int com.southwind.aspect.CalImpl.*(..))：表示切入点是

com.southwind.aspect包下CalImpl类中的所有方法。

即CalImpl所有方法在执行之前会首先执行LoggerAspect类中的before方法。

after方法同理，

表示CalImpl所有方法执行之后会执行LoggerAspect类中的after方法。

afterReturn方法表示CalImpl所有方法在return之后会执行LoggerAspect类中的afterReturn方法。

afterThrowing方法表示CalImpl所有方法在抛出异常时会执行LoggerAspect类中的afterThrowing方法。

所以我们就可以根据具体需求，选择在before，after，afterReturn，afterThrowing方法中添加相应代码。

2.目标类也需要添加@Component注解。

```java
@Component
public class CalImpl implements Cal{

    @Override
    public int add(int num1, int num2) {
        // TODO Auto-generated method stub
        return num1+num2;
    }

    @Override
    public int sub(int num1, int num2) {
        // TODO Auto-generated method stub
        return num1-num2;
    }

    @Override
    public int mul(int num1, int num2) {
        // TODO Auto-generated method stub
        return num1*num2;
    }

    @Override
    public int div(int num1, int num2) {
        // TODO Auto-generated method stub
        return num1/num2;
    }

}
```

3.spring.xml中进行配置。

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.0.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd">

    <!-- 自动扫描 -->
    <context:component-scan base-package="com.southwind.aspect"></context:component-scan>

    <!-- 使Aspect注解生效，为目标类自动生成代理对象 -->
    <aop:aspectj-autoproxy></aop:aspectj-autoproxy>

</beans>
```

1.将com.southwind.aspect包中的类扫描到IOC容器中。

2.添加aop:aspectj-autoproxy注解，Spring容器会结合切面类和目标类自动生成动态代理对象，Spring框架的AOP底层就是通过动态代理的方式完成AOP。

4.测试方法执行如下代码：

从IOC容器中获取代理对象，执行方法。

```java
public class Test {
    public static void main(String[] args) {
        //加载spring.xml
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
        //获取代理对象
        Cal cal = (Cal) applicationContext.getBean("calLogger");
        //执行方法
        cal.add(10, 3);
        cal.sub(10, 3);
        cal.mul(10, 3);
        cal.div(10, 3);
    }
}
```

![img](https://mmbiz.qpic.cn/mmbiz_png/t4kgaKYXPLexsDu1uv0NHlYnnW90Fq0lc7FkkhAp34lLAuAEicztAQYyXIL8lfkxYicrdbnENPIWBdxWruwibsITw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

成功。结合代码，回过头来说几个概念更好理解。

切面：横切关注点被模块化的特殊对象。

CalImpl所有方法中需要加入日志的部分，抽象成一个切面对象LoggerAspect。

通知：切面对象完成的工作。

LoggerAspect对象打印日志的操作。

目标：被通知的对象，即被横切的对象。

CalImpl对象。

 代理：切面通知目标混合之后的内容。

 连接点：程序要执行的某个特定位置。

切面方法要插入业务代码的具体位置。

 切点：AOP通过切点定位到连接点。

#### 代码示例2：

- applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
       http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop-4.0.xsd">

	
	<!-- advice 通知/增强 -->
	
	<!-- 前置通知 -->
	<bean id="beforeAdvice" 
	        class="com.etoak.utils.BeforeAdvice" />
	<!-- 后置通知 -->
	<bean id="afterAdvice" 
			class="com.etoak.utils.AfterAdvice" />
    <!-- 环绕通知  适合做缓存-->
    <bean id="aroundAdvice" 
            class="com.etoak.utils.AroundAdvice" />
    <!-- 异常通知  适合做输出日志信息  -->
    <bean id="throwAdvice" 
    		class="com.etoak.utils.ThrowAdvice" />
	        
	<!-- aop配置 -->
	<aop:config>
	
		<!-- 
			定义切入点 核心部分 代理LoginController对象
			                  LoginController对象交给第三方管理
			
			第一个*表示 方法的返回值 
			第二个*表示 所有的类
			第三个*表示 所有的方法
			com.etoak.controller 表示包结构路径
			(..) 表示参数
			
		 -->
		 <aop:pointcut 
		 		expression="execution(* com.etoak.controller.*.*(..))" 
		 		id="p" />
		 		

		 
		 <!-- 
		 	 底层连接点
		 	 
		 	 通过连接点将通知和切入点组装在一起  就一个完整的aop 
		 	 
		 	 aop = 通知 + 切入点
		  -->		
		 <!-- <aop:advisor advice-ref="beforeAdvice" 
		              pointcut-ref="p" />
		              
		 <aop:advisor advice-ref="afterAdvice" 
		              pointcut-ref="p" /> -->
		   
		 <!-- <aop:advisor advice-ref="aroundAdvice" 
		              pointcut-ref="p" /> -->
		              
		 <aop:advisor advice-ref="throwAdvice" 
		 			  pointcut-ref="p"/>
	</aop:config>
	
	
	<!-- 业务类 核心 -->
	<bean id="loginController"
		 class="com.etoak.controller.LoginController" />
</beans>
```

- LoginController.java

```java
package com.etoak.controller;

//核心部分
public class LoginController {
	
	//业务  在spring aop中称为 切入点
	public String login(String username,String password) {
		System.out.println("用户名：" + username);
		System.out.println("密码：" + password);
		
		//测试异常通知
		int i = 1/0;
		
		return username + "|" + password;
	}
}

```

- BeforeAdvice.java   (前置通知)

```java
package com.etoak.utils;

import java.lang.reflect.Method;

import org.springframework.aop.AfterReturningAdvice;

//非核心
//切面类 和 切面
public class AfterAdvice implements AfterReturningAdvice {

	//通知  后置通知  之后通知
	@Override
	public void afterReturning(
			Object result, // 目标对象方法返回值
			Method method, // 目标对象方法
			Object[] args, // 目标对象方法参数
			Object object // 目标对象，被代理对象
			) throws Throwable {
		
		System.out.println("目标对象：" 
					+ object.getClass().getName());
		
		System.out.println("目标对象方法名称：" + method.getName());
		
		//目标对象方法返回值
		if(result != null) {
			System.out.println("目标对象方法返回值：" 
						+ result.getClass().getName() 
						+ "|" + result);
		}
		
		//目标对象方法参数
		if(args != null && args.length > 0) {
			for(Object arg : args) {
				System.out.println(
						arg.getClass().getName() 
						+ "|" 
						+ arg);
			}
		}		
	}

}

```

- AfterAdvice.java  (后置通知)

```java
package com.etoak.utils;

import java.lang.reflect.Method;

import org.springframework.aop.AfterReturningAdvice;

//非核心
//切面类 和 切面
public class AfterAdvice implements AfterReturningAdvice {

	//通知  后置通知  之后通知
	@Override
	public void afterReturning(
			Object result, // 目标对象方法返回值
			Method method, // 目标对象方法
			Object[] args, // 目标对象方法参数
			Object object // 目标对象，被代理对象
			) throws Throwable {
		
		System.out.println("目标对象：" 
					+ object.getClass().getName());
		
		System.out.println("目标对象方法名称：" + method.getName());
		
		//目标对象方法返回值
		if(result != null) {
			System.out.println("目标对象方法返回值：" 
						+ result.getClass().getName() 
						+ "|" + result);
		}
		
		//目标对象方法参数
		if(args != null && args.length > 0) {
			for(Object arg : args) {
				System.out.println(
						arg.getClass().getName() 
						+ "|" 
						+ arg);
			}
		}
	}

}

```

- AroundAdvice.java    (环绕通知)

```java
package com.etoak.utils;

import java.lang.reflect.Method;

import org.aopalliance.intercept.MethodInterceptor;
import org.aopalliance.intercept.MethodInvocation;
				
//非核心
//切面类 或 切面
public class AroundAdvice implements MethodInterceptor {

	//通知  环绕通知
	@Override
	public Object invoke(MethodInvocation invocation) 
				throws Throwable {
		
		/*if(true){
			return "et1801";
		}*/
		
		//非核心
		before(invocation);
		
		//执行切入点方法    表示开发人员调用的哪个方法就表示是哪个方法
		//login add
		//核心点
		Object result = invocation.proceed();
		
		//非核心
		after();
		
		return result;
	}
	
	//前置通知/增强
	private void before(MethodInvocation invocation) {
		//目标对象方法参数
		Object[] args = invocation.getArguments();
		//目标对象方法
		Method method = invocation.getMethod();
		//目标对象
		Object object = invocation.getThis();
		
		System.out.println("目标对象:" + object.getClass().getName());
		System.out.println("目标对象方法:" + method.getName());
		
		if(args != null && args.length > 0) {
			for(Object arg : args) {
				System.out.println(
						arg.getClass().getName() 
						+ "|" 
						+ arg);
			}
		}
		
	}
	
	//后置通知/增强
	private void after() {
		System.out.println("环绕通知：执行后置通知");
	}

}

```

- 异常通知

```java
package com.etoak.utils;

import java.io.FileOutputStream;
import java.io.PrintWriter;
import java.lang.reflect.Method;
import java.text.SimpleDateFormat;
import java.util.Date;

import org.springframework.aop.ThrowsAdvice;

//非核心部分
//切面类 或 切面
public class ThrowAdvice
			 implements ThrowsAdvice {
	
	//异常通知  等价后置通知
	//当切入点方法出现异常时 触发afterThrowing方法
	public void afterThrowing(Method method, Object[] args, Object target,Exception ex) {
		
		System.out.println("异常信息：" + ex.getMessage());
		
		
		
		//格式化时间 每天按时间来生成文件 ， 每天一个异常信息文件
		SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
		String date = sdf.format(new Date());
		
		//将异常信息保存文件中，方便开发人员定位异常信息
		try (
				FileOutputStream out =
						new FileOutputStream("C:/" + date + ".txt",true);
				
				PrintWriter pw = new PrintWriter(out);
		    ) {
			
			//将异常信息传输给PrintWriter包装流，
			//PrintWriter包装流再传输给FileOutputStream流
			//最后生成文件  格式：yyyy-MM-dd.txt
			pw.write(target.getClass().getName() + "\t\n");
			pw.write(method.getName() + "\t\n");
			ex.printStackTrace(pw);
			pw.write("\t\n");
			pw.write("==================" + "\t\n");
			
		} catch(Exception e) {
		    e.printStackTrace();	
		}	
	}
}

```

- Test

```java
package com.etoak.test;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import com.etoak.controller.LoginController;

public class SpringTest {

	public static void main(String[] args) {
		//spring容器
		ApplicationContext ac = 
		new ClassPathXmlApplicationContext("applicationContext.xml");
		
		//获取实例化对象
		LoginController lc =
				(LoginController)ac.getBean("loginController");
		
		//代理之后的切入点方法 会执行通知/增强操作
		String result = lc.login("etoak", "et1803");
		
		//输出结果
		System.out.println("返回结果：" + result);
	}
}
```

## 3.0 Spring整合mybatis

####  操作数据库相关知识

```
plsql  
		
               1.解析主配置文件
                  settings属性
		  数据源 连接数据库
                  实体类起别名
                  映射sql.xml配置
                  插件

               2.SqlSessionFactory对象

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
                  


               mybatis  
                      半自动
                     手写sql语句  方便优化sql语句
                     返回结果集 resultType resultMap
                     通过反射封装面向对象



              spring
                   非注解形式
                     <bean>元素
                   注解形式



		非注解
                    spring框架提供的类  <bean>元素
                     
	 	注解
                    业务逻辑 增删查改
                    开发人员自定义的类可以使用注解实例化


               mybatis
                  动态标签
                   if where foreach sql
 
               #(?) 
               $(字符串 原样输出) like

                select * from 表名 
		where username = ‘etoak’
		 and password =    “or 1=1”

             jdbc vs  mybatis  vs hibernate 

               get vs post
               请求转发 vs 重定向
               九大内置对象 **********************
               session vs cookie  单点登录 sso
               jquery
             
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
            
             user
 		id   name  deptId deptName

             dept
                id   name
   
          select b.name from 
		user a
                	left join 
                dept b
                       on  a.deptId = b.id

          select deptName from user
```



##   4.0  Spring mvc 总结

#### SpringMvc简介

SpringMVC是什么？

SpringMVC是目前最好的实现MVC设计模式的框架，是Spring框架的一个分支产品，以SpringIOC容器为基础，并利用容器的特性来简化它的配置。SpringMVC相当于Spring的一个子模块，可以很好的和Spring结合起来进行开发，是JavaWeb开发者必须要掌握的框架。

SpringMVC能干什么？

实现了MVC设计模式，MVC设计模式是一种常用的软件架构方式：以Controller（控制层），Model（模型层），View（视图层）三个模块分离的形式来组织代码。

MVC流程：控制层接受到客户端请求，调用模型层生成业务数据，传递给视图层，将最终的业务数据和视图响应给客户端做展示。

![img](https://mmbiz.qpic.cn/mmbiz_jpg/t4kgaKYXPLddlc7umnrUF7XI83xXqY2plsI3ibyPtJM8pRe3emUh66HkZbNdDr2SF8YEqqrFpwatm7icub99iaQbg/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1)

SpringMVC就是对这套流程的封装，屏蔽掉很多底层代码，开放出接口，让开发者可以更加轻松快捷的完成基于MVC模式的Web开发。

#### SpringMVC实现原理：

核心组件：

1.DispatcherServlet：前端控制器，是整个流程控制的核心，控制其他组件的执行，统一调度，降低组件之间的耦合性，相当于总指挥。

2.Handler：处理器，完成具体业务逻辑，相当于Servlet或Action。

3.HandlerMapping：DispatcherServlet接收到请求之后，通过HandlerMapping将不同的请求分发到不同的Handler。

4.HandlerInterceptor：处理器拦截器，是一个接口，如果我们需要做一些拦截处理，可以来实现这个接口。

5.HandlerExecutionChain：处理器执行链，包括两部分内容：Handler和HandlerInterceptor（系统会有一个默认的HandlerInterceptor，如果需要额外拦截处理，可以添加拦截器设置）。

6.HandlerAdapter：处理器适配器，Handler执行业务方法之前，需要进行一系列的操作包括表单数据的验证，数据类型的转换，将表单数据封装到JavaBean等等，这一系列的操作，都是由HandlerAdapter来完成，DispatcherServlet通过HandlerAdapter执行不同的Handler。

7.ModelAndView：装载了模型数据和视图信息，作为Handler的处理结果，返回给DispatcherServlet。

8.ViewResolver：视图解析器，DispatcherServlet通过它将逻辑视图解析成物理视图，最终将渲染结果响应给客户端。

以上就是SpringMVC的核心组件。那么这些组件之间是如何进行交互的呢？

我们来看SpringMVC的实现流程：

1.客户端请求被DispatcherServlet（前端控制器）接收。

2.根据HandlerMapping映射到Handler。

3.生成Handler和HandlerInterceptor（如果有则生成）。

4.Handler和HandlerInterceptor以HandlerExecutionChain的形式一并返回给DispatcherServlet。

5.DispatcherServlet通过HandlerAdapter调用Handler的方法做业务逻辑处理。

6.返回一个ModelAndView对象给DispatcherServlet。

7.DispatcherServlet将获取的ModelAndView对象传给ViewResolver视图解析器，将逻辑视图解析成物理视图View。

8.ViewResolver返回一个View给DispatcherServlet。

9.DispatcherServlet根据View进行视图渲染（将模型数据填充到视图中）。

10.DispatcherServlet将渲染后的视图响应给客户端。

![img](https://mmbiz.qpic.cn/mmbiz_jpg/t4kgaKYXPLddlc7umnrUF7XI83xXqY2pvrk0TFI4MrZSs7yUAYiblHsHKjrJeHIzr1krlYOxgg7yjAwgqKiaa0BA/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1)

```
  springmvc集成 spring、servlet、restful
                             
                        restful
                              网络传输，高并发、高延迟
                        
                        get      select
                        post     insert
                        put      update
                        delete   delete

		http://www.tuicool.com/articles/67vyIbv  			jersey

              springmvc集成spring
                spring是父容器  
                springmvc是子容器

           
           jdk          tomcat      web.xml
           1.6 1.7       6.X         2.5
           1.7.1.8       7.X         3.0
           1.8           8.X         3.1   nio
          

              new io
    web.xml
    mybatis-----------
     jsp  
     web.xml
     controller
     service
     dao mapper

```

常见错误 ：400   参数不匹配

spring默认不处理日期类型

##### 环境搭建

如何使用？

看到上面的实现原理，大家可能会有这样的担心，SpringMVC如此众多的组件开发起来一定很麻烦吧？答案是否定的，SpringMVC使用起来非常简单，很多组件都由框架提供，作为开发者我们直接使用即可，并不需要自己手动编写代码，真正需要我们开发者进行编写的组件只有两个，Handler：处理业务逻辑， View：JSP做展示。

###### 代码示例 1：

1.环境搭建    

创建maven工程，创建pom.xml配置SpringMVC依赖jar包。

```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.hzit</groupId>
  <artifactId>SpringMVCMaven</artifactId>
  <packaging>war</packaging>
  <version>0.0.1-SNAPSHOT</version>
  <name>SpringMVCTest Maven Webapp</name>
  <url>http://maven.apache.org</url>

  <dependencies>

    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.8.1</version>
      <scope>test</scope>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>4.3.1.RELEASE</version>
    </dependency>
   </dependencies>

  <build>
    <finalName>SpringMVCTest</finalName>
  </build>
</project>
```

2.web.xml中配置SpringMVC的DispatcherServlet。

```
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="2.5" 
    xmlns="http://java.sun.com/xml/ns/javaee" 
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
    xsi:schemaLocation="http://java.sun.com/xml/ns/javaee 
    http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd">
  <display-name></display-name>    
      <welcome-file-list>
           <welcome-file>index.jsp</welcome-file>
      </welcome-file-list>

    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
              <param-name>contextConfigLocation</param-name>
              <!-- 指定springmvc.xml的路径 -->
              <param-value>classpath:springmvc.xml</param-value>
        </init-param>
      </servlet>
      <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
      </servlet-mapping> 

</web-app>
```

3.创建springmvc.xml配置文件。

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

    <!-- 配置自动扫描 -->
    <context:component-scan base-package="com.southwind.handler"></context:component-scan>

    <!-- 配置视图解析器 -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!-- 前缀 -->
        <property name="prefix" value="/"></property>
        <!-- 后缀 -->
        <property name="suffix" value=".jsp"></property>
    </bean>

</beans>
```

自动扫描的类结合注解交给IOC容器来管理，视图解析器是SpringMVC底层的类，开发者只需要进行配置即可完成JSP页面的跳转，如何配置很简单，记住一条：目标资源路径=前缀+返回值+后缀。

比如DispatcherServlet返回index，配置文件中前缀是/，后缀是.jsp，代入上述公式：

目标资源路径=/index.jsp。一目了然，不需要再解释。

4.创建Handler类。

```
@Controller
public class HelloHandler {

    @RequestMapping("/index")
    public String index(){
        System.out.println("执行index业务代码");
        return "index";
    }

}
```

直接在业务方法出添加注解@RequestMapping("/index")，将URL请求index直接与index业务方法映射，使用起来非常方便。

5.启动tomcat，打开浏览器输入URL测试。

![img](https://mmbiz.qpic.cn/mmbiz_png/t4kgaKYXPLddlc7umnrUF7XI83xXqY2phnT4S3QeAia1QxDHrK8ymic8tjrJ7gtGtcic7JBDDm4A6xuYKYTXCcPEQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

![img](https://mmbiz.qpic.cn/mmbiz_png/t4kgaKYXPLddlc7umnrUF7XI83xXqY2pwiaSvbXyUfovsiciaYUUibEib2uRTgnynu4bDy0a28lJITKCOAbpicZ8ChiaA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

1.DispatcherServlet接收到URL请求index，结合@RequestMapping("/index")注解将该请求交给index业务方法。

2.执行index业务方法，控制打印日志，并返回"index"字符串。

3.结合springmvc.xml中的视图解析器配置，找到目标资源：/index.jsp，即根目录的index.jsp，将该jsp资源返回客户端完成响应。

SpringMVC环境搭建成功。

###### 代码示例2：

- etoak-servlet.xml  （springmvc配置文件）

````xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context-4.0.xsd
       http://www.springframework.org/schema/mvc
       http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd">


	<!-- 
		扫描ioc注解
			@Controller
		      spring 实例化
              springmvc 实例化 服务端
		    @Service @Repository @Autowired等
		
		当前扫描操作不是spring ，是springmvc
	 -->
	<context:component-scan base-package="com.etoak" />
	
	<!-- 
		多个bean元素  
		映射器(请求)、适配器（执行方法）、类型转换 （参数、json）
	 -->
	<mvc:annotation-driven />
	
	<!-- 
		视图解析器 
		/success.jsp
	-->
	<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
	    <!-- 视图前缀 -->
		<property name="prefix" value="/"></property>
	    <!-- 视图后缀 -->
		<property name="suffix" value=".jsp"></property>
	</bean>
	
</beans>
````

- web.xml    (web.xml中配置SpringMVC的DispatcherServlet。)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd" id="WebApp_ID" version="3.0">
  <display-name>springmvc01</display-name>

  
  <!-- 处理乱码 post请求 start -->
  <filter>
  	<filter-name>encoding</filter-name>
  	<filter-class>
  		org.springframework.web.filter.CharacterEncodingFilter
  	</filter-class>
  	<!-- 请求 默认编码 iso-8859-1 -->
  	<init-param>
  		<param-name>encoding</param-name>
  		<param-value>UTF-8</param-value>
  	</init-param>
  	<!-- 响应 默认编码 iso-8859-1 -->
  	<init-param>
  		<param-name>forceEncoding</param-name>
  		<param-value>true</param-value>
  	</init-param>
  </filter>
  
  <filter-mapping>
  	<filter-name>encoding</filter-name>
  	<url-pattern>/*</url-pattern>
  </filter-mapping>
  <!-- 处理乱码 post请求 end -->
  
  <!-- springmvc start -->
  <servlet>
  	<servlet-name>etoak</servlet-name>
  	<!-- 
  		默认加载springmvc配置文件路径为：
  				WEB-INF/[servlet-name]-serlvet.xml
  	 -->
  	<servlet-class>
  		org.springframework.web.servlet.DispatcherServlet
  	</servlet-class>
  </servlet>
  
  <servlet-mapping>
  	<servlet-name>etoak</servlet-name>
  	<url-pattern>*.do</url-pattern>
  </servlet-mapping>
  <!-- springmvc end -->
  
</web-app>
```

- 前端页面

```html
login.jsp----------------------------------
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
</head>
<body>

	<!-- 
		${pageContext.request.contextPath }
		el表达式 获取工程名
		
		get请求 尽量别传中文参数  会乱码
		
		/springmvc01/login.do?username=value&password=value
 	-->
	<form action="${pageContext.request.contextPath }/login.do"
	      method="post">
	      
	      用户名：<input type="text" name="username" />
	      <br/>
	      密码：  <input type="password" name="password" />  
	      <br/>
	      
	        <!-- 
	        		注意事项：使用js事件
	        		 例如点击事件 按钮类型不要使用submit类型 
	        -->
	        <input type="submit" value="登录" /> 
	   
	</form>

</body>
</html>
register.jsp--------------------------------------------
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
</head>
<body>

	<form action="${pageContext.request.contextPath }/login.do"
	      method="post">
	      
	      用户名：<input type="text" name="username" />
	      <br/>
	      
	      密码：<input type="password" name="password" />
	      <br/>
	      
	      生日：<select name="birthDay">
	        <option value="1990-01-01">1990-01-01</option>
	        <option value="1991-02-02">1991-02-02</option>
	        <option value="1992-03-03">1992-03-03</option>
	       </select>
	      <br/>
	      
	       性别：<input type="radio" name="gender" value="1" />男
	       <input type="radio" name="gender" value="2" />女
	       <input type="radio" name="gender" value="3" />人妖
	       <br/>
	       
	       爱好：<input type="checkbox" name="hobby" value="1" />抽烟
	       <input type="checkbox" name="hobby" value="2" />喝酒
	       <input type="checkbox" name="hobby" value="3" />烫头
	       <input type="checkbox" name="hobby" value="4" />纹身
	       <input type="checkbox" name="hobby" value="5" />撩妹
	       <br/>
	       
	       <input type="submit" value="注册" />
	
	</form>

</body>
</html>

success.jsp-------------------------
    <%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
</head>
<body>
成功！！！！！
<br/>
${msg }
</body>
</html>

```

- controller层

```java
package com.etoak.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.servlet.ModelAndView;

/**
 * springmvc集成spring
 * @Controller 
 *    spring 实例化
 *    springmvc 实例化 服务端 接收请求
 */
@Controller
public class LoginController {
	
	/**
	 * @RequestMapping
	 *       接收请求  例如/login请求
	 * @RequestParam
	 *       接收请求参数
	 *       等价 request.getParameter("username") String
	 *       例如
	 *       	 String username = @RequestParam(value="username")
	 */
	@RequestMapping(value="/login")
	public ModelAndView login(
			@RequestParam(value="username") String username,
			@RequestParam(value="password") String password
			) {
		
		
		//输出业务信息
		System.out.println("用户名：" + username);
		System.out.println("密码：" + password);
		
		//定义跳转视图  springmvc视图类型
		ModelAndView mv = new ModelAndView();
		//跳转视图名称
		mv.setViewName("success");
		//默认等价request范围
		mv.addObject("msg", "第一个springmvc工程");
		
		return mv;
	}
	
	

}

```

```java
package com.etoak.controller;

import java.util.Date;
import java.util.List;

import javax.servlet.http.HttpServletRequest;

import org.springframework.format.annotation.DateTimeFormat;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

import com.etoak.bean.Register;

/**
 *@Controller
 *   实例化对象  服务端
 *   
 *   @RequestMapping
 *       作用域
 *          类      ： 命名空间  表示唯一      一般命名：类名  
 *          方法  ： 接收请求地址
 *          /register/register
 */
@RequestMapping(value="/register")
@Controller
public class RegisterController {
	
	/**
	 * @RequestMapping
	 *     value属性接收请求
	 *     method属性接收请求类型
	 *     
	 *     发送register.do请求,必须为post请求类型
	 *     
	 *     默认什么请求都接收
	 *     
	 * springmvc集成servlet
	 *     HttpServletRequest
	 *     HttpServletResponse
	 *     HttpSession    request.getSession
	 *     
	 * 跳转视图 有2种类型
	 *  String
	 *  ModelAndView
	 */
	@RequestMapping(value="/register",method=RequestMethod.POST)
	public String register(Register r,HttpServletRequest request) {
		
		/**********springmvc api**********************/
		System.out.println("用户名：" + r.getUsername());
		System.out.println("密码：" + r.getPassword());
		System.out.println("生日：" + r.getBirthDay());
		System.out.println("性别：" + r.getGender());
		
		List<String> hobbys = r.getHobby();
		if(hobbys != null && hobbys.size() > 0) {
			System.out.print("爱好：");
			for(String hobby : hobbys) {
				System.out.print(hobby + ",");
			}
			System.out.println();
		}
		/**********servlet api**********************/
		System.out.println("---------------------------");
		
		String username = request.getParameter("username");
		String password = request.getParameter("password");
		String birthDay = request.getParameter("birthDay");
		String gender   = request.getParameter("gender");
		
		String[] hobby = request.getParameterValues("hobby"); 
		
		System.out.println("用户名：" + username);
		System.out.println("密码：" + password);
		System.out.println("生日：" + birthDay);
		System.out.println("性别：" + gender);
		
		if(hobby.length > 0) {
			System.out.print("爱好：");
			for(String str : hobby) {
				System.out.print(str + ",");
			}
		}
		
		//返回视图
		request.setAttribute("msg", "springmvc接收模型驱动参数");
		return "success";
	}
	
	
	/*public String registerBAK(
			@RequestParam("username") String username,
			@RequestParam("password") String password,
			@RequestParam("gender") Integer gender,
			@RequestParam("hobby") List<String> hobby,
			@RequestParam("birthDay") @DateTimeFormat(pattern="yyyy-MM-dd") Date brithDay,
			HttpServletRequest request) {
		
		return null;
	}*/

}
```

#### Springmvc 的request域和session域&&乱码处理

- web.xml

````xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd" id="WebApp_ID" version="3.0">
  <display-name>springmvc01</display-name>

  
  <!-- 处理乱码 post请求 start -->
  <filter>
  	<filter-name>encoding</filter-name>
  	<filter-class>
  		org.springframework.web.filter.CharacterEncodingFilter
  	</filter-class>
  	<!-- 请求 默认编码 iso-8859-1 -->
  	<init-param>
  		<param-name>encoding</param-name>
  		<param-value>UTF-8</param-value>
  	</init-param>
  	<!-- 响应 默认编码 iso-8859-1 -->
  	<init-param>
  		<param-name>forceEncoding</param-name>
  		<param-value>true</param-value>
  	</init-param>
  </filter>
  
  <filter-mapping>
  	<filter-name>encoding</filter-name>
  	<url-pattern>/*</url-pattern>
  </filter-mapping>
  <!-- 处理乱码 post请求 end -->
  
  <!-- springmvc start -->
  <servlet>
  	<servlet-name>etoak</servlet-name>
  	<!-- 
  		默认加载springmvc配置文件路径为：
  				WEB-INF/[servlet-name]-serlvet.xml
  	 -->
  	<servlet-class>
  		org.springframework.web.servlet.DispatcherServlet
  	</servlet-class>
  </servlet>
  
  <servlet-mapping>
  	<servlet-name>etoak</servlet-name>
  	<url-pattern>*.do</url-pattern>
  </servlet-mapping>
  <!-- springmvc end -->
</web-app>
````

- springmvc的xml文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context-4.0.xsd
       http://www.springframework.org/schema/mvc
       http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd">


	<!-- 
		扫描ioc注解
			@Controller
		      spring 实例化
              springmvc 实例化 服务端
		    @Service @Repository @Autowired等
		
		当前扫描操作不是spring ，是springmvc
	 -->
	<context:component-scan base-package="com.etoak" />
	
	<!-- 
		多个bean元素 
		映射器(请求)、适配器（执行方法）、类型转换 （参数、json）
	 -->
	<mvc:annotation-driven />
	
	<!-- 
		视图解析器 
		/success.jsp
		forward:/success.jsp
		forward:/forwardView.do
	-->
	<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
	    <!-- 视图前缀 -->
		<property name="prefix" value="/"></property>
	    <!-- 视图后缀 -->
		<property name="suffix" value=".jsp"></property>
	</bean>
</beans>
```

- 前端  jsp 页面(发送请求)

  forwardAdnRedirect.jsp

```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
</head>
<body>

	<a href="${pageContext.request.contextPath }/forwardView.do">
		请求转发
	</a>
	
	<br/>
	
	<a href="${pageContext.request.contextPath }/forwardView2.do">
		请求转发后台访问后台请求
	</a>
	
	<br/>
	
	<a href="${pageContext.request.contextPath }/redirectView.do">
		重定向
	</a>
	
	<br/>
	
	<a href="${pageContext.request.contextPath }/redirectView2.do">
		重定向后台访问后台请求
	</a>

</body>
</html>
```

- **controller**

  GetSessionController

```java
package com.etoak.controller;

import javax.servlet.http.HttpSession;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.ui.ModelMap;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.SessionAttributes;

@SessionAttributes(value={"m_model","mm_modelmap"})
@Controller
public class GetSessionController {
	
	@RequestMapping(value="getSession")
	public String getSession(
			Model m,ModelMap mm,
			HttpSession session) {
		
		//model没有get方法获取信息
		//需要通过modelmap的get方法来获取model信息
		String m_model = (String)mm.get("m_model");
		
		String mm_modelmap = (String)mm.get("mm_modelmap");
		
		String servelt_session = 
				(String)session.getAttribute("servelt_session");
		
		System.out.println("通过modelmap获取model信息：" + m_model);
		System.out.println("获取modelmap信息：" + mm_modelmap);
		System.out.println("获取servlet，session信息：" + servelt_session);
		
		
		session.setAttribute("msg", "测试session范围");
		
		return "success";
	}
}
```

RequestController

```java
package com.etoak.controller;

import javax.servlet.http.HttpServletRequest;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.ui.ModelMap;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class RequestController {
	
	/**
	 * Springmvc 内置对象  缺省 等价 默认值
	 * Model ModelMap  默认为request范围
	 */
	@RequestMapping(value="/requestScope")
	public String requestScope(
			Model m,ModelMap mm,
			HttpServletRequest request) {
		
		
		/**********springmvc  api******************/
		
		m.addAttribute("m_model", "etoak");
		mm.addAttribute("mm_modelmap", "et1801");
		
		
		mm.addAttribute("msg", "测试modelmap-springmvc");
		m.addAttribute("msg", "测试model-springmvc");
		
		
		/**********servlet  api******************/
		
		request.setAttribute("req", "易途");
		request.setAttribute("msg", "测试-servlet");
	
		return "requestScope";
	}

}

```

SetSessionController

```java
package com.etoak.controller;

import javax.servlet.http.HttpSession;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.ui.ModelMap;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.SessionAttributes;

//将model和modelmap转换成session范围
@SessionAttributes(value={"m_model","mm_modelmap"})
@Controller
public class SetSessionController {
	
	/**
	 * ModelMap  Model 默认request范围
	 */
	@RequestMapping(value="/sessionScope")
	public String sessionScope(
			Model m,ModelMap mm,
			HttpSession session) {
		
		/***********springmvc api********************/
		
		m.addAttribute("m_model", "易途");
		mm.addAttribute("mm_modelmap", "et1803");
		
		
		/***********servlet api********************/
		
		session.setAttribute("servelt_session", "etoak");
		
		return "sessionScope";
		
	}

}

```

ForwardAndRedirectController

```java
package com.etoak.controller;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpSession;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class ForwardAndRedirectController {
	
	/**
	 * 测试请求转发
	 */
	@RequestMapping(value="/forwardView")
	public String forwardView(HttpServletRequest request) {
		
		request.setAttribute("msg", "测试请求转发");
		
		//forward:表示springmvc提供请求转发状态码  不能写错 固定写法
		return "forward:/success.jsp";
		
	}
	
	/**
	 * 请求转发后台访问后台请求
	 */
	@RequestMapping(value="forwardView2")
	public String forwardView2() {
		return "forward:/forwardView.do";
	}
	/*********************************************/
	
	/**
	 * 测试重定向
	 */
	@RequestMapping(value="/redirectView")
	public String redirectView(HttpSession session) {
		
		session.setAttribute("msg", "测试重定向");
		
		//redirect:表示springmvc提供重定向状态码  不能写错 固定写法
		//重定向不接收request范围信息
		return "redirect:/success.jsp";
		
	}
	
	/**
	 *	重定向后台访问后台请求
	 */
	@RequestMapping(value="/redirectView2")
	public String redirectView2() {
		return "redirect:/redirectView.do";
	}
	
}

```

- jsp 页面   （取出域中的数据）

```html
scope.jsp------------
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
</head>
<body>
   

	<a href="${pageContext.request.contextPath }/requestScope.do">
		request范围
	</a>
	
	<br/>
	
	<a href="${pageContext.request.contextPath }/sessionScope.do">
		session范围
	</a>

</body>
</html>
sessionScope.jsp------------
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
</head>
<body>
   

	<a href="${pageContext.request.contextPath }/requestScope.do">
		request范围
	</a>
	
	<br/>
	
	<a href="${pageContext.request.contextPath }/sessionScope.do">
		session范围
	</a>

</body>
</html>
success.jsp-------------
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
</head>
<body>
成功！！！！！
<br/>
${msg }
</body>
</html>
requestScope.jsp---------------
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
</head>
<body>
   springmvc api :
	    <br/>
	    ${m_model }
	    <br/>
	    ${mm_modelmap }
	    <br/>
   servelt api :
	    <br/>
	    ${req }
	    
	    <hr/>
	    <br/>
	    ${msg }
</body>
</html>
```



####  文件上传和下载

Web项目中，文件上传功能几乎是必不可少的，实现的技术有很多，今天我们来学习如何使用SpringMVC框架完成文件的上传以及下载。

##### 代码示例1：

单文件上传

1.底层使用的是Apache fileupload组件完成上传，SpringMVC只是进行了封装，让开发者使用起来更加方便，所以首先需要引入fileupload组件的依赖。

```
    <dependency>
        <groupId>commons-io</groupId>
        <artifactId>commons-io</artifactId>
        <version>1.3.2</version>
    </dependency>

    <dependency>
        <groupId>commons-fileupload</groupId>
        <artifactId>commons-fileupload</artifactId>
        <version>1.2.1</version>
    </dependency>
```

2.JSP

1.input的type设置为file。

2.form表单的method设置为post。（get请求只会将文件名传给后台）

3.form表单的enctype设置为multipart/form-data，以二进制的形式传输数据。

```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%
    String path = request.getContextPath();
    String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
    <form action="upload" method="post" enctype="multipart/form-data">
        <input type="file" name="img">
        <input type="submit" name="提交">
    </form><br /> 
    <c:if test="${filePath!=null }">
        <h1>上传的图片</h1><br /> 
        <img width="300px" src="<%=basePath %>${filePath}"/>
    </c:if>
</body>
</html>
```

如果上传成功，返回当前页面，展示上传成功的图片，这里需要使用JSTL标签进行判断。

pom文件中引入JSTL依赖。

```
    <dependency>
        <groupId>jstl</groupId>
        <artifactId>jstl</artifactId>
        <version>1.2</version>
    </dependency>
    <dependency>
        <groupId>taglibs</groupId>
        <artifactId>standard</artifactId>
        <version>1.1.2</version>
    </dependency>
```

3.业务方法，使用MultipartFile对象作为参数，接收前端发送过来的文件，并完成上传操作。

```
   @RequestMapping(value="/upload", method = RequestMethod.POST)
   public String upload(@RequestParam(value="img")MultipartFile img, HttpServletRequest request)
           throws Exception {
       //getSize()方法获取文件的大小来判断是否有上传文件
       if (img.getSize() > 0) {
          //获取保存上传文件的file文件夹绝对路径
          String path = request.getSession().getServletContext().getRealPath("file");
          //获取上传文件名
          String fileName = img.getOriginalFilename();
          File file = new File(path, fileName);
          img.transferTo(file);
          //保存上传之后的文件路径
          request.setAttribute("filePath", "file/"+fileName);
          return "upload";
        }
      return "error";
  }
```

4.springmvc.xml配置CommonsMultipartResolver。

```
    <!-- 配置CommonsMultipartResolver bean，id必须是multipartResolver -->
    <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
        <!-- 处理文件名中文乱码 -->
        <property name="defaultEncoding" value="utf-8"/>
        <!-- 设置多文件上传，总大小上限，不设置默认没有限制，单位为字节，1M=1*1024*1024 -->
        <property name="maxUploadSize" value="1048576"/>
        <!-- 设置每个上传文件的大小上限 -->
        <property name="maxUploadSizePerFile" value="1048576"/>
    </bean>

    <!-- 设置异常解析器 -->
    <bean class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">
        <property name="defaultErrorView" value="/error.jsp"/>
    </bean>
```

5.运行。

![img](https://mmbiz.qpic.cn/mmbiz_png/t4kgaKYXPLdrtoibY9DbnfVss0EsRuVKtceyNATlmsicfhiauYqgiaznUupVjiaJolgafjl0Dn1iasBViaAHyb1TubTqA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

![img](https://mmbiz.qpic.cn/mmbiz_png/t4kgaKYXPLdrtoibY9DbnfVss0EsRuVKtM0jnlF2N1icRhYKQt5dibNpW1DH8DdIBictZyl3Jkf0bDDal7lCGia2ckA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

多文件上传

1.JSP。

```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%
    String path = request.getContextPath();
    String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
    <form action="uploads" method="post" enctype="multipart/form-data">
        file1:<input type="file" name="imgs"><br />
        file2:<input type="file" name="imgs"><br /> 
        file3:<input type="file" name="imgs"><br />  
        <input type="submit" name="提交">
    </form>
    <c:if test="${filePaths!=null }">
        <h1>上传的图片</h1><br /> 
        <c:forEach items="${filePaths }" var="filePath">
            <img width="300px" src="<%=basePath %>${filePath}"/>
        </c:forEach>
    </c:if>
</body>
</html>
```

2.业务方法，使用MultipartFile数组对象接收上传的多个文件。

```
   @RequestMapping(value="/uploads", method = RequestMethod.POST)
   public String uploads(@RequestParam MultipartFile[] imgs, HttpServletRequest request)
           throws Exception {
       //创建集合，保存上传后的文件路径
       List<String> filePaths = new ArrayList<String>();
       for (MultipartFile img : imgs) {
           if (img.getSize() > 0) {
              String path = request.getSession().getServletContext().getRealPath("file");
              String fileName = img.getOriginalFilename();
              File file = new File(path, fileName);
              filePaths.add("file/"+fileName);
              img.transferTo(file);
            }
       }
       request.setAttribute("filePaths", filePaths);
       return "uploads";
   }
```

3.运行。

![img](https://mmbiz.qpic.cn/mmbiz_png/t4kgaKYXPLdrtoibY9DbnfVss0EsRuVKtRMcNpKNQaW72qlhO5hib3rxgqu9ervic8PQ6ad8l6hgsGKZm8E6LllVQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

![img](https://mmbiz.qpic.cn/mmbiz_png/t4kgaKYXPLdrtoibY9DbnfVss0EsRuVKtyO9X1JsTEcIcyRIibNy7ib254apm3krJVe8SEUib27jpO0VUY89iaBny1w/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

文件下载

1.JSP，使用超链接，下载之前上传的logo.jpg。

```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
    <a href="download?fileName=logo.jpg">下载图片</a>
</body>
</html>
```

2.业务方法。

```
  @RequestMapping("/download")
  public void downloadFile(String fileName,HttpServletRequest request,
       HttpServletResponse response){
     if(fileName!=null){
       //获取file绝对路径
       String realPath = request.getServletContext().getRealPath("file/");
       File file = new File(realPath,fileName);
       OutputStream out = null;
       if(file.exists()){
          //设置下载完毕不打开文件 
          response.setContentType("application/force-download");
          //设置文件名 
          response.setHeader("Content-Disposition", "attachment;filename="+fileName);
          try {
             out = response.getOutputStream();
             out.write(FileUtils.readFileToByteArray(file));
             out.flush();
          } catch (IOException e) {
             // TODO Auto-generated catch block
             e.printStackTrace();
          }finally{
              if(out != null){
                  try {
                     out.close();
                 } catch (IOException e) {
                     // TODO Auto-generated catch block
                     e.printStackTrace();
                 }
             }
          }                    
       }           
    }           
 }
```

3.运行。

![img](https://mmbiz.qpic.cn/mmbiz_png/t4kgaKYXPLdrtoibY9DbnfVss0EsRuVKtywMP11oSvsficuicg82nxEsxv44jJ9wBbbVVQ60UP0ZnFV25UMiaFlc1Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

##### 代码示例2：

- 配置文件

​         web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd" id="WebApp_ID" version="3.0">
  <display-name>springmvc01</display-name>

  
  <!-- 处理乱码 post请求 start -->
  <filter>
  	<filter-name>encoding</filter-name>
  	<filter-class>
  		org.springframework.web.filter.CharacterEncodingFilter
  	</filter-class>
  	<!-- 请求 默认编码 iso-8859-1 -->
  	<init-param>
  		<param-name>encoding</param-name>
  		<param-value>UTF-8</param-value>
  	</init-param>
  	<!-- 响应 默认编码 iso-8859-1 -->
  	<init-param>
  		<param-name>forceEncoding</param-name>
  		<param-value>true</param-value>
  	</init-param>
  </filter>
  
  <filter-mapping>
  	<filter-name>encoding</filter-name>
  	<url-pattern>/*</url-pattern>
  </filter-mapping>
  <!-- 处理乱码 post请求 end -->
  
  <!-- springmvc start -->
  <servlet>
  	<servlet-name>etoak</servlet-name>
  	<!-- 
  		默认加载springmvc配置文件路径为：
  				WEB-INF/[servlet-name]-serlvet.xml
  	 -->
  	<servlet-class>
  		org.springframework.web.servlet.DispatcherServlet
  	</servlet-class>
  </servlet>
  
  <servlet-mapping>
  	<servlet-name>etoak</servlet-name>
  	<url-pattern>*.do</url-pattern>
  </servlet-mapping>
  <!-- springmvc end -->
  
</web-app>
```

etoak-servlet.xml   (springmvc的配置文件)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context-4.0.xsd
       http://www.springframework.org/schema/mvc
       http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd">


	<!-- 
		扫描ioc注解
			@Controller
		      spring 实例化
              springmvc 实例化 服务端
		    @Service @Repository @Autowired等
		
		当前扫描操作不是spring ，是springmvc
	 -->
	<context:component-scan base-package="com.etoak" />
	
	<!-- 
		多个bean元素 
		映射器(请求)、适配器（执行方法）、类型转换 （参数、json）
	 -->
	<mvc:annotation-driven />
	
	<!-- 
		视图解析器 
		/success.jsp
		forward:/success.jsp
		forward:/forwardView.do
	-->
	<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
	    <!-- 视图前缀 -->
		<property name="prefix" value="/"></property>
	    <!-- 视图后缀 -->
		<property name="suffix" value=".jsp"></property>
	</bean>
	
	<!-- 
		上传解析器
		
		getBean("multipartResolver")
		注意事项 id必须为  id="multipartResolver"
		
	-->
	<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
		<!-- 默认编码为ISO-8859-1 -->
		<property name="defaultEncoding" 
					value="UTF-8"></property>
	    <!-- 设置上传大小  单位b  3M-5M  3*1024*1204-->
		<property name="maxUploadSize" 
		            value="3000000"></property>
	</bean>
	
</beans
```

- jsp页面

  upload.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
</head>
<body>
    
    <!-- 
       	 批量上传
    	ajaxFileUpload.js
    	jquery.uploadify.js
    	webupload.js   
    	jquery.zTree.js
     -->
	<form action="${pageContext.request.contextPath }/upload.do"
	      method="post"
	      enctype="multipart/form-data">
	
		<input type="file" name="f" />
		
		<input type="submit" value="上传" />
	
	</form>
	
	<form action="${pageContext.request.contextPath }/uploadFtp.do"
	      method="post"
	      enctype="multipart/form-data">
	
		<input type="file" name="f" />
		
		<input type="submit" value="上传ftp" />
	
	</form>
	
	

</body>
</html>
```

 download.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
</head>
<body>
	<!-- 
		# 空连接  会刷新页面 跳转当前页面
		javascript:void();  不会刷新页面
	 -->
	<a href="${pageContext.request.contextPath }/download.do?fileName=${fileName}">
		下载
	</a>
	
	<br/>
	
	<a href="${pageContext.request.contextPath }/downloadFtp.do?fileName=${fileName}">
		下载ftp
	</a>
</body>
</html>
```

- controller
- 上传

```java
package com.etoak.controller;

import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.util.UUID;

import javax.servlet.ServletContext;
import javax.servlet.http.HttpSession;

import org.apache.commons.io.FilenameUtils;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.multipart.MultipartFile;

import com.etoak.util.FtpUtil;

@Controller//实例化对象  服务端
public class UploadController {
	
	/**
	 * MultipartFile 上传使用的类型
	 * 上传文件保存有3种方式
	 * 1.将上传的文件信息保存到服务端（tomcat）
	 *    缺点：每次服务器重启  上传信息丢失
	 * 2.持久化 将上传文件保存到数据库表中
	 *      java      数据库表
	 *      byte[]    orcale:blob    mysql:longblob 
	 * 3.ftp   上传文件协议     服务器
	 *     在当前的pc端保存到另一个pc端
	 */
	@RequestMapping(value="/upload")
	public String upload(
			@RequestParam(value="f") MultipartFile f,
			HttpSession session) {
		
		System.out.println("上传文件的类型：" + f.getContentType());
		System.out.println("上传文件请求参数名称：" + f.getName());
		System.out.println("上传文件名称：" + f.getOriginalFilename());
		System.out.println("上传文件的大小" + f.getSize());
		//true为空   false不为空
		System.out.println("上传文件是否为空：" + f.isEmpty());
		
		//判断上传文件是否为空
		if(f != null && !f.isEmpty() && f.getSize() > 0) {
			
			//获取上传文件保存路径
			ServletContext sc = session.getServletContext();
			String path = sc.getRealPath("/files");
			
			//获取文件路径
			File file = new File(path);
			
			//判断路径是否存在
			if(!file.exists()) {
				//创建保存文件路径
				file.mkdirs();
			}
			
			//随机生成信息
			String uuid = UUID.randomUUID().toString();
			//获取上传文件后缀名称    et1803.txt   txt
			String suffix = 
					FilenameUtils.getExtension(f.getOriginalFilename());
			
			//组装上传文件名称
			String fileName = uuid + "." + suffix;
			
		/*	try {
				//将二进制信息上传到指定保存路径
				f.transferTo(new File(path,fileName));
			} catch (IllegalStateException | IOException e) {
				e.printStackTrace();
			}*/
			
			
			try(
				//获取上传二进制流信息
				InputStream is = f.getInputStream();
				//输出上传文件信息，并保存到指定路径
				OutputStream out = 
						new FileOutputStream(new File(path,fileName));
			) {
				//定义缓存信息   
				//is.available()上传文件的大小  缓冲的长度
				byte[] b = new byte[is.available()];
				
				//读取上传信息  等于-1表示读取到最后一个字节
				while(is.read(b) != -1) {
					//输出上传信息
					out.write(b);
				}
			}catch (Exception e) {
				e.printStackTrace();
			}
			
			//获取上传二进制字节信息
			//byte[] b = f.getBytes();
			
			
		}
		
		return "download";
	}
	
	
	@RequestMapping(value="/uploadFtp")
	public String uploadFtp(
			@RequestParam(value="f") MultipartFile f) {
		
	    //通过ftp上传文件信息
		//考虑压缩
		FtpUtil ftp = FtpUtil.getInstance();
		try {
		//true上传成功  false上传失败
		boolean b = ftp.upload(
					"127.0.0.1", 21, 
					"ay2853", "123456", 
					"D:/ftp", //服务器保存上传文件路径
					f.getOriginalFilename(), 
					f.getInputStream());
		
		//判断上传是否成功
		if(!b) {
			System.out.println("上传失败");
		}
		
		} catch (IOException e) {
			e.printStackTrace();
		}
		
		return "download";
	}	

}

```

- 下载

```java
package com.etoak.controller;

import java.io.File;
import java.io.FileInputStream;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.UnsupportedEncodingException;
import java.net.URLDecoder;

import javax.servlet.ServletContext;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;

import com.etoak.util.FtpUtil;

@Controller
public class DownloadController {
	
	@RequestMapping(value="/download")
	public void download(
			@RequestParam(value="fileName") String fileName,
			HttpSession session,
			HttpServletResponse response) {
		
		//获取下载附件路径
		ServletContext sc = session.getServletContext();
		String path = sc.getRealPath("/files");
		
		//获取下载附件原名称
		String originalFilename = 
				(String)session.getAttribute("originalFilename");
		
		//定义响应头信息   针对下载
		try {
			response.addHeader(
					"content-disposition", 
					"attachment;filename=" + 
					new String(originalFilename.getBytes("GB2312"),"ISO-8859-1"));
		} catch (UnsupportedEncodingException e1) {
			e1.printStackTrace();
		}
		
		try(
			//读取下载附件信息
			InputStream is = 
				new FileInputStream(new File(path,fileName));
			//响应下载信息对象
		    OutputStream out = response.getOutputStream();
		) {
			
			//定义缓存信息
			byte[] b = new byte[is.available()];
			
			//读取下载附件信息，并输出响应给浏览器
			while(is.read(b) != -1) {
				out.write(b);
			}
			
		}catch(Exception e) {
			e.printStackTrace();
		}
		
	}
	
	@RequestMapping(value="/downloadFtp")
	public void downloadFtp(
			@RequestParam(value="fileName") String fileName,
			HttpServletResponse response) {
		
		//decoder解密
		String originalFilename = null;
		try {
			originalFilename = URLDecoder.decode(
					URLDecoder.decode(fileName, "UTF-8"),
					"UTF-8");
		} catch (UnsupportedEncodingException e2) {
			e2.printStackTrace();
		}
		
		//定义响应头信息   针对下载
		try {
			response.addHeader(
					"content-disposition", 
					"attachment;filename=" + 
					new String(originalFilename.getBytes("GB2312"),"ISO-8859-1"));
		} catch (UnsupportedEncodingException e1) {
			e1.printStackTrace();
		}
		
		try(
			//读取下载附件信息
			InputStream is = 
				FtpUtil.getInstance()
				.download(
						"127.0.0.1", 21,
						"ay2853", "123456", 
						"D:/ftp", originalFilename);
			//响应下载信息对象
		    OutputStream out = response.getOutputStream();
		) {
			
			//定义缓存信息
			byte[] b = new byte[is.available()];
			
			//读取下载附件信息，并输出响应给浏览器
			while(is.read(b) != -1) {
				out.write(b);
			}
			
		}catch(Exception e) {
			e.printStackTrace();
		}
		
	}
}
```

######FtpUtil.java    (向ftp服务器上上传数据的工具类)

```java
package com.etoak.util;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.util.logging.Logger;

import org.apache.commons.net.ftp.FTPClient;
import org.apache.commons.net.ftp.FTPReply;

public class FtpUtil {
	
	private FtpUtil(){}
	//输出日志
	private static Logger logger = Logger.getLogger(FtpUtil.class.getName());
	
	//创建FTPClient对象
	private static FTPClient ftp = new FTPClient();
	
	//定义FtpUtil对象
	private static FtpUtil INSTANCE = null;
	
	//单例模式 创建FtpUtil对象 
	public static FtpUtil getInstance() {
		if(INSTANCE == null) {
			 synchronized (FtpUtil.class) {
				 if(INSTANCE == null) {
					 INSTANCE = new FtpUtil();
				 }
			}
		}
		return INSTANCE;
	}
	
	/**
	 * 上传
	 * @param hostname ip地址
	 * @param port     端口号  ftp默认为21
	 * @param username ftp用户名
	 * @param password ftp密码
	 * @param ftppath  磁盘保存路径
	 * @param filename 保存文件名称
	 * @param input    上传二进制信息
	 * @return
	 */
	public boolean upload(String hostname,int port,String username,String password,
						  String ftppath,String filename,InputStream input) {
         try {
        	//设置编码为UTF-8
 			ftp.setControlEncoding("UTF-8");
        	// 连接FTP服务器   ftp://localhost:21
			ftp.connect(hostname, port);
			// 登陆FTP服务器   ay2853 123456
			ftp.login(username, password);
			
			//判断是否连接 连接尝试后的查询
			if(disConnect()) {
				logger.info("上传未连接到FTP，关闭资源");
				return false;
			}
			
			//设置类型为二进制文件
			ftp.setFileType(FTPClient.BINARY_FILE_TYPE);
			
			//设置上传目录   转移到FTP服务器目录  
			ftp.enterLocalPassiveMode();  
            ftp.changeWorkingDirectory(ftppath);  
            
            //保存上传信息
            return ftp.storeFile(filename, input);           
			
		} catch (IOException e) {
			e.printStackTrace();
		} finally {
			try {
				//关闭上传信息
				input.close();
				//打开连接之后 退出登录
				logout();
			} catch (IOException e) {
				e.printStackTrace();
			}
		}
         
         return false;
	}
	
	/**
	 * 下载
	 * @param hostname ip地址
	 * @param port     端口号  ftp默认为21
	 * @param username ftp用户名
	 * @param password ftp密码
	 * @param ftppath  磁盘保存路径
	 * @param filename 保存文件名称
	 * @return
	 */
	public InputStream download(String hostname,int port,String username,String password,
						  String ftppath,String filename) {
         try {
        	//设置编码为UTF-8
 			ftp.setControlEncoding("UTF-8");
        	// 连接FTP服务器  
			ftp.connect(hostname, port);
			// 登陆FTP服务器  
			ftp.login(username, password);
			
			//判断是否连接 连接尝试后的查询
			if(disConnect()) {
				logger.info("下载未连接到FTP，关闭资源");
				return null;
			}
			
			//设置类型为二进制文件
			ftp.setFileType(FTPClient.BINARY_FILE_TYPE);
			
			//设置上传目录   转移到FTP服务器目录  
			ftp.enterLocalPassiveMode();  
            ftp.changeWorkingDirectory(ftppath);  
            
            //下载上传信息
           return ftp.retrieveFileStream(filename);      
			
		} catch (IOException e) {
			e.printStackTrace();
		} finally {
			try {
				//打开连接之后 退出登录
				logout();
			} catch (IOException e) {
				e.printStackTrace();
			}
		}
         
         return null;
	}
	
	/**
	 * 删除
	 * @param hostname ip地址
	 * @param port     端口号  ftp默认为21
	 * @param username ftp用户名
	 * @param password ftp密码
	 * @param ftppath  磁盘保存路径
	 * @param filename 保存文件名称
	 * @return
	 */
	public boolean remove(String hostname,int port,String username,String password,
						  String ftppath,String filename) {
         try {
        	//设置编码为UTF-8
 			ftp.setControlEncoding("UTF-8");
        	// 连接FTP服务器  
			ftp.connect(hostname, port);
			// 登陆FTP服务器  
			ftp.login(username, password);
			
			//判断是否连接 连接尝试后的查询
			if(disConnect()) {
				logger.info("删除未连接到FTP，关闭资源");
				return false;
			}
			
			//设置类型为二进制文件
			ftp.setFileType(FTPClient.BINARY_FILE_TYPE);
			
			//设置上传目录   转移到FTP服务器目录  
			ftp.enterLocalPassiveMode();  
            ftp.changeWorkingDirectory(ftppath);  
            
            //删除上传信息
           return ftp.deleteFile(filename);   
			
		} catch (IOException e) {
			e.printStackTrace();
		} finally {
			try {
				//打开连接之后 退出登录
				logout();
			} catch (IOException e) {
				e.printStackTrace();
			}
		}
         
         return false;
	}
	
	//连接尝试后的查询  true表示未连接 false表示已连接
	private boolean disConnect() throws IOException {
		boolean b = !FTPReply.isPositiveCompletion(ftp.getReplyCode());
		 if (b) {  
			 logger.info("未连接到FTP，用户名或密码错误。");
			 //关闭连接
             ftp.disconnect();  
         } else {
        	 logger.info("FTP连接成功。");  
         }
		 
		 return b;
	}
	
	//退出
	public void logout() throws IOException {
		//打开连接之后 退出登录
		if(ftp.isConnected()) {
			//ftp退出登录
			ftp.logout();
			logger.info("FTP退出成功。");  
		}
		
	}
	
	public static void main(String[] args) throws Exception {
		
		FtpUtil ftpUtil = FtpUtil.getInstance();
		//测试上传
		/*InputStream input = new FileInputStream("E:/1.txt");
		ftpUtil.upload("127.0.0.1", 21, "ay2853", "2853", "D:/server/ftp", "测试1.txt", input);*/
		
		//测试下载
		/*InputStream input = ftpUtil.download("127.0.0.1", 21, "ay2853", "2853", "D:/server/ftp", "测试1.txt");
		
		OutputStream out = new FileOutputStream("E:/测试1.txt");
		byte[] b = new byte[input.available()];
		while(input.read(b) != -1) {
			out.write(b);
		}*/
		
		//测试删除
		//ftpUtil.remove("127.0.0.1", 21, "ay2853", "2853", "D:/server/ftp", "测试1.txt");
		
		//out.flush();
		//out.close();
		//input.close();
		
		
	}

}
```

#### ajax  & json 

```
	<!-- 
  		springmvc 只兼容2种请求格式
  		1. *.do   *.action
  			禁止使用 *.html   不兼容json格式
  		2. 斜杠(/) 
  		        禁止使用/*
  		        使用/请求 表示springmvc集成restful
  		        使用/请求 默认不加载静态资源   js、css、<img>
  	-->
```



  1.导包

![1529581617532](C:\Users\ADMINI~1\AppData\Local\Temp\1529581617532.png)

2. 引入js文件

###### 代码示例1:

- web.xml          =>  要设置请求的url

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd" id="WebApp_ID" version="3.0">
  <display-name>springmvc01</display-name>

  
  <!-- 处理乱码 post请求 start -->
  <filter>
  	<filter-name>encoding</filter-name>
  	<filter-class>
  		org.springframework.web.filter.CharacterEncodingFilter
  	</filter-class>
  	<!-- 请求 默认编码 iso-8859-1 -->
  	<init-param>
  		<param-name>encoding</param-name>
  		<param-value>UTF-8</param-value>
  	</init-param>
  	<!-- 响应 默认编码 iso-8859-1 -->
  	<init-param>
  		<param-name>forceEncoding</param-name>
  		<param-value>true</param-value>
  	</init-param>
  </filter>
  
  <filter-mapping>
  	<filter-name>encoding</filter-name>
  	<url-pattern>/*</url-pattern>
  </filter-mapping>
  <!-- 处理乱码 post请求 end -->
  
  <!-- springmvc start -->
  <servlet>
  	<servlet-name>etoak</servlet-name>
  	<!-- 
  		默认加载springmvc配置文件路径为：
  				WEB-INF/[servlet-name]-serlvet.xml
  	 -->
  	<servlet-class>
  		org.springframework.web.servlet.DispatcherServlet
  	</servlet-class>
  	<!-- 自定义加载springmvc配置文件路径 -->
  	<init-param>
  		<param-name>contextConfigLocation</param-name>
  		<param-value>classpath:etoak-servlet.xml</param-value>
  	</init-param>
  </servlet>
  
  <servlet-mapping>
  	<servlet-name>etoak</servlet-name>
  	<!-- 
  		springmvc 只兼容2种请求格式
  		1. *.do   *.action
  			禁止使用 *.html   不兼容json格式
  		2. 斜杠(/) 
  		        禁止使用/*
  		        使用/请求 表示springmvc集成restful
  		        使用/请求 默认不加载静态资源   js、css、<img>
  	-->
  	<url-pattern>/</url-pattern>
  </servlet-mapping>
  <!-- springmvc end -->
  
</web-app>
```

-   etoak-servlet.xml     ( springmvc的配置文件) 

  > 在springmvc配置文件中设置，配置加载静态资源
  >
  > 
  > ​		
  >
  > 	<!-- 
  > 			/请求    加载静态资源 js css <img> 
  > 	
  > 			*.do请求不需要配置静态资源加载
  > 		location属性表示当前工程需要映射的路径
  > 		mapping属性表示映射加载location属性指定的路径，
  > 				加载静态资源文件 js css <img>
  > 		
  > 		*表示一层目录    /js/*    /js/js1/
  > 		**表示多层目录 /js/**   /js/js1/js2/js3/js4/
  > 	-->

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context-4.0.xsd
       http://www.springframework.org/schema/mvc
       http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd">


	<!-- 
		扫描ioc注解
			@Controller
		      spring 实例化
              springmvc 实例化 服务端
		    @Service @Repository @Autowired等
		
		当前扫描操作不是spring ，是springmvc
	 -->
	<context:component-scan base-package="com.etoak" />
	
	<!-- 
		多个bean元素 
		映射器(请求)、适配器（执行方法）、类型转换 （参数、json）
	 -->
	<mvc:annotation-driven />

	<!-- 
		/请求    加载静态资源 js css <img> 
		*.do请求不需要配置静态资源加载
		
		location属性表示当前工程需要映射的路径
		mapping属性表示映射加载location属性指定的路径，
				加载静态资源文件 js css <img>
		
		*表示一层目录    /js/*    /js/js1/
		**表示多层目录 /js/**   /js/js1/js2/js3/js4/
	-->
	<mvc:resources location="/js/" mapping="/js/**" />
	<mvc:default-servlet-handler />
	
	<!-- 
		视图解析器 
	-->
	<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
	    <!-- 视图前缀 -->
		<property name="prefix" value="/"></property>
	    <!-- 视图后缀 -->
		<property name="suffix" value=".jsp"></property>
	</bean>
	
	<!-- 
		上传解析器
		
		getBean("multipartResolver")
		注意事项 id必须为  id="multipartResolver"
		
	-->
	<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
		<!-- 默认编码为ISO-8859-1 -->
		<property name="defaultEncoding" 
					value="UTF-8"></property>
	    <!-- 设置上传大小  单位b  3M-5M  3*1024*1204-->
		<property name="maxUploadSize" 
		            value="3000000"></property>
	</bean>
	
</beans>
```

- ajax.jsp    =>前端页面发送ajax请求

> ```
> //请求发送json参数
> 		//响应接收json参数
> 		
> 		//js转换json
> 		//JSON.parse(str);   将json字符串转换成js对象
> 		//JSON.stringify(object); 将js对象转换成json字符串
> 		
> 		//java  JSONObject
> 		
> 		//js字面量 对象
> ```

```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">

<script type="text/javascript" 
		src="${pageContext.request.contextPath }/js/jquery-1.11.1.min.js"></script>

<script type="text/javascript">

	$(function(){
		
		//请求发送json参数
		//响应接收json参数
		
		//js转换json
		//JSON.parse(str);   将json字符串转换成js对象
		//JSON.stringify(object); 将js对象转换成json字符串
		
		//java  JSONObject
		
		//js字面量 对象
		var obj = {
			  id : 'et',
			  name : 'et1803',
			  age : 5
		};
		
		$.ajax({
			//后台访问请求地址
			url : '${pageContext.request.contextPath }/ajax',
			//请求参数内容类型  415 406
			contentType : "application/json",
			//请求参数
			data : JSON.stringify(obj),
			//请求类型
			type : 'post',
			//响应参数格式
			dataType : 'json',
			//响应成功之后执行success方法
			success : function(data) {
				console.log(data.success);
			}
		});
		
	});

</script>

</head>
<body>
</body>
</html>
```

- ​     AjaxContrller.java             controller 接收和处理请求          

>```java
>/**
> * 
> * springmvc 响应参数
> * 
> *    跳转视图2种类型
> *     String
> *     ModelAndView
> *     
> *     void  HttpServletResponse 下载  
> *     		 HttpServletRequest
> *    
> *     响应json类型  随意定义类型    String、ModelAndView除外 
> *     {key:value,key:value}   Map put(key) User key
> *     [ {key:value,key:value}, {key:value,key:value}] List<Map>
> *     [value,value,value]  List<String> List<Integer>
> *     
> *     @RequestBody接收请求json字符串，
> *                 再将json字符串转换成java对象
> *                 put("id","et")
> *                 put("name","et1803");
> *                 put("age",5);
> *                 
> *        request.getInputStream();
> *     @ResponseBody
> *                 将对象转换成json字符串
> *        response.getOutputStream();
> *        response.getWriter();
> */
>```

```java
package com.etoak.controller;

import java.util.HashMap;
import java.util.Map;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class AjaxController {
	
	/**
	 * 
	 * springmvc 响应参数
	 * 
	 *    跳转视图2种类型
	 *     String
	 *     ModelAndView
	 *     
	 *     void  HttpServletResponse 下载  
	 *     		 HttpServletRequest
	 *    
	 *     响应json类型  随意定义类型    String、ModelAndView除外 
	 *     {key:value,key:value}   Map put(key) User key
	 *     [ {key:value,key:value}, {key:value,key:value}] List<Map>
	 *     [value,value,value]  List<String> List<Integer>
	 *     
	 *     @RequestBody接收请求json字符串，
	 *                 再将json字符串转换成java对象
	 *                 put("id","et")
	 *                 put("name","et1803");
	 *                 put("age",5);
	 *                 
	 *        request.getInputStream();
	 *     @ResponseBody
	 *                 将对象转换成json字符串
	 *        response.getOutputStream();
	 *        response.getWriter();
	 */
	@RequestMapping(value="/ajax")
	@ResponseBody
	public Map<String,Object> ajax(
			@RequestBody Map<String,Object> map) {
		
		//获取参数信息
		String id = (String)map.get("id");
		String name = map.get("name").toString();
		String age = map.get("age").toString();
		//输出信息
		System.out.println("ID:" + id);
		System.out.println("NAME:" + name);
		System.out.println("AGE:" + age);
		
		//响应参数
		Map<String,Object> result = new HashMap<>();
		result.put("success", "响应参数：" + id + "," + name);
		//{success: "响应参数：" + id + "," + name}
		return result;
	}

}

```

###### 代码示例二：   拦截器和自定义注解 

- web.xml        => <!-- 自定义加载springmvc配置文件路径 -->

```xml
 <?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd" id="WebApp_ID" version="3.0">
  <display-name>springmvc01</display-name>

  
  <!-- 处理乱码 post请求 start -->
  <filter>
  	<filter-name>encoding</filter-name>
  	<filter-class>
  		org.springframework.web.filter.CharacterEncodingFilter
  	</filter-class>
  	<!-- 请求 默认编码 iso-8859-1 -->
  	<init-param>
  		<param-name>encoding</param-name>
  		<param-value>UTF-8</param-value>
  	</init-param>
  	<!-- 响应 默认编码 iso-8859-1 -->
  	<init-param>
  		<param-name>forceEncoding</param-name>
  		<param-value>true</param-value>
  	</init-param>
  </filter>
  
  <filter-mapping>
  	<filter-name>encoding</filter-name>
  	<url-pattern>/*</url-pattern>
  </filter-mapping>
  <!-- 处理乱码 post请求 end -->
  
  <!-- springmvc start -->
  <servlet>
  	<servlet-name>etoak</servlet-name>
  	<!-- 
  		默认加载springmvc配置文件路径为：
  				WEB-INF/[servlet-name]-serlvet.xml
  	 -->
  	<servlet-class>
  		org.springframework.web.servlet.DispatcherServlet
  	</servlet-class>
  	<!-- 自定义加载springmvc配置文件路径 -->
  	<init-param>
  		<param-name>contextConfigLocation</param-name>
  		<param-value>classpath:etoak-servlet.xml</param-value>
  	</init-param>
  </servlet>
  
  <servlet-mapping>
  	<servlet-name>etoak</servlet-name>
  	<!-- 
  		springmvc 只兼容2种请求格式
  		1. *.do   *.action
  			禁止使用 *.html   不兼容json格式
  		2. 斜杠(/) 
  		        禁止使用/*
  		        使用/请求 表示springmvc集成restful
  		        使用/请求 默认不加载静态资源   js、css、<img>
  	-->
  	<url-pattern>/</url-pattern>
  </servlet-mapping>
  <!-- springmvc end -->
  
</web-app>
```

- etoak-servlet.xml      => 加载静态资源，并配置拦截器

  ><!-- 
  >		配置拦截器 
  >		springmvc中拦截器 除了视图（jsp,html）不拦截 ,
  >						剩下的请求都会被拦截
  >	-->

  

```xml
 <?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context-4.0.xsd
       http://www.springframework.org/schema/mvc
       http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd">

	<!-- 
		扫描ioc注解
			@Controller
		      spring 实例化
              springmvc 实例化 服务端
		    @Service @Repository @Autowired等
		
		当前扫描操作不是spring ，是springmvc
	 -->
	<context:component-scan base-package="com.etoak" />
	
	<!-- 
		多个bean元素 
		映射器(请求)、适配器（执行方法）、类型转换 （参数、json）
	 -->
	<mvc:annotation-driven />

	<!-- 
		/请求    加载静态资源 js css <img> 
		*.do请求不需要配置静态资源加载
		
		location属性表示当前工程需要映射的路径
		mapping属性表示映射加载location属性指定的路径，
				加载静态资源文件 js css <img>
		
		*表示一层目录    /js/*    /js/js1/
		**表示多层目录 /js/**   /js/js1/js2/js3/js4/
	-->
	<mvc:resources location="/js/" mapping="/js/**" />
	<mvc:default-servlet-handler />
	
	<!-- 
		配置拦截器 
		springmvc中拦截器 除了视图（jsp,html）不拦截 ,
						剩下的请求都会被拦截
	-->
	<mvc:interceptors>
		<mvc:interceptor>
		    <!-- 需要拦截的请求地址 -->
			<!-- 	
			<mvc:mapping path="/login/login" />
			<mvc:mapping path="/login/logout" /> 
			<mvc:mapping path="/login/*" />
			-->
			<mvc:mapping path="/**" />
			<!-- 不需要拦截的请求地址 -->
			<!-- <mvc:exclude-mapping path="/login/logout" /> -->
			<mvc:exclude-mapping path="/js/**" />
			
			<!-- 
				配置拦截器对象，并实例化该对象 
				拦截器需要实现HandlerInterceptor接口
			-->
			<bean class="com.etoak.common.AuthInterceptor">
			</bean>
			<!-- <ref bean="authInterceptor" /> -->
		</mvc:interceptor>
	</mvc:interceptors>
	
	<!-- 
		视图解析器 
	-->
	<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
	    <!-- 视图前缀 -->
		<property name="prefix" value="/"></property>
	    <!-- 视图后缀 -->
		<property name="suffix" value=".jsp"></property>
	</bean>
	
	<!-- 
		上传解析器
		
		getBean("multipartResolver")
		注意事项 id必须为  id="multipartResolver"
		
	-->
	<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
		<!-- 默认编码为ISO-8859-1 -->
		<property name="defaultEncoding" 
					value="UTF-8"></property>
	    <!-- 设置上传大小  单位b  3M-5M  3*1024*1204-->
		<property name="maxUploadSize" 
		            value="3000000"></property>
	</bean>
	
</beans
```

- 前端页面   (发送请求)  login.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">

<script type="text/javascript" 
		src="${pageContext.request.contextPath }/js/jquery-1.11.1.min.js"></script>
</head>
<body>

<a href="${pageContext.request.contextPath }/login/login">
	登录
</a>

<br/>

<a href="${pageContext.request.contextPath }/login/logout">
	退出
</a>


</body>
</html>
------------   success.jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
success!!!
</body>
</html>
--------------  error.jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
当前操作没有权限！！！！
</body>
</html>
```

- **配置拦截器**  (springmvc实现拦截器操作 需要实现HandlerInterceptor接口)

```java
package com.etoak.common;

import java.lang.reflect.Method;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.web.method.HandlerMethod;
import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;


/**
 *  springmvc实现拦截器操作 需要实现HandlerInterceptor接口
 */
//@Component
public class AuthInterceptor implements HandlerInterceptor {

	/**
	 * 3.在跳转响应视图成功之后，触发afterCompletion方法
	 * */
	@Override
	public void afterCompletion(
			HttpServletRequest request, 
			HttpServletResponse response,
			Object object, Exception ex)
			throws Exception {
		System.out.println("执行afterCompletion方法！！！");
	}

	/**
	 * 2.在跳转响应视图之前，触发postHandle方法
	 * */
	@Override
	public void postHandle(
			HttpServletRequest request, 
			HttpServletResponse response, 
			Object object, 
			ModelAndView mv)
			throws Exception {
		System.out.println("执行postHandle方法！！！");
	}

	/**
	 * 1.在访问后台请求时 先触发preHandle方法
	 *   返回值返回true表示验证通过，可以执行后台请求方法
	 *   反之返回false验证不通过，不能执行后台请求方法
	 *   默认为false;
	 * */
	@Override
	public boolean preHandle(
			HttpServletRequest request, 
			HttpServletResponse response, 
			Object object) throws Exception {
		
		System.out.println("执行拦截器操作！！！");
		System.out.println("url：" + request.getRequestURL());
		System.out.println("uri：" + request.getRequestURI());
		
		//获取后台请求方法
		HandlerMethod hm = (HandlerMethod)object;
		//当前method对象表示 后台请求的方法
		Method method = hm.getMethod();
		//判断方法上是否存在@Etoak注解
		Etoak etoak = method.getAnnotation(Etoak.class);
		
		if(etoak != null) {
			//获取@Etoak注解et属性
			String et = etoak.et();
			if(et != null && "etoak".equals(et)) {
				return true;
			}
		}
		
		response.sendRedirect(request.getContextPath() + "/error.jsp");
		//默认为验证不通过
		return false;
	}

}

```

 ------- 自定义注解，用来判断权限

```java
package com.etoak.common;

import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

//作用域  该注解只能使用在方法上
@Target({ java.lang.annotation.ElementType.METHOD})
//运行时执行该注解
@Retention(RetentionPolicy.RUNTIME)
public @interface Etoak {

	public String et();
	
}

```

- controller
```java
package com.etoak.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

import com.etoak.common.Etoak;

@Controller
@RequestMapping(value="/login")
public class LoginController {
	
	
	@Etoak(et="etoak")
	@RequestMapping(value="/login")
	public String login() {
		
		System.out.println("登录操作！！！");
		
		return "success";
	}
	
	@Etoak(et="et1803")
	@RequestMapping(value="/logout")
	public String logout() {
		
		System.out.println("退出操作");
		
		return "success";
	}

}

```

