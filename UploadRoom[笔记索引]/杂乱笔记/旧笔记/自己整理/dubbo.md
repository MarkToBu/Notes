# dubbo

## 1 dubbo的含义

```
1 dubbo是阿里巴巴提供的高性能的，分布式框架，dubbo通过rpc协议进行输入(请求)和输出(响应)，soap的治理方案，		spring整合
		soap [soa xml] 简单对象访问协议，具有单一性，通过客户端访问服务端
		rcp 远程过程调用协议，具有netty 是异步的，不需要考虑线程
2 dubbo的目的，通过rcp协议进行远程调用(客户端)，解决高延迟,高并发，单一性，通过负载均衡，和容错进行异步通信
	负载均衡：是指当客户端通过IP地址和端口号组成的路径时，访问服务端的时候，如果第一个地址不行的话，那么就使		用第二个，第二个也不行，第三个，也不行的话就说明这个服务端挂掉了
	容错：是指当客户端通过IP地址和端口号组成的路径时，访问服务端的时候。设置每个地址的容错率，当使用这个地址		的时候，如果不超过这个容错率，就继续访问这个地址，超过的话访问下一个。
```

## 2 dubbo客户端和服务端

```
		调用方客户端       				提供方（写接口） 服务端
           dubbo               				dubbo
           webservice          				webservice
           httpclient          				http springmvc
	  	   httpok
           resttemplate
```

## 3 dubbo的流程

```
1 服务端将IP地址和端口号自动拼接成URL地址，注册到zookeeper这个第三方软件当中
	1):开启zookeeper服务
		开启服务器
		开启窗口-通过这个可以查找暴露在zookeeper中的接口和路径 ls/ 
	2):开启服务端的配置，将继承的接口暴露在zookeeper之中
2 客户端获取zookeeper中的URL地址
	1):客户端去zookeeper之中去寻找着接口
3 然后通过这个地址进行远程服务代理，调用接口
	1):调用接口-相当于调用实现类，
```

## 4 dubbo中三个方面的含义

```
1 zookeeper			服务端开启
2 生产者(客户端)	   将服务注册到zookeeper中
3 消费者(服务端)	   获取zookeeper中的地址再去调用生产者
```

## 5 dubbo的简单配置

### [1] 接口的配置

#### [1] 创建实体类

```java
//通信传参数  二进制
//序列化将对象转换成byte[]  
//反序列化将byte[]转换成对象
public class User implements Serializable {
	private static final long serialVersionUID = 4135257031886156479L;
	private String username;
	private String password;
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
	public static long getSerialversionuid() {
		return serialVersionUID;
	}
}
```

#### [2] 创建接口

创建一个简单的接口

### [2] 服务端的配置

#### [1] pom.xml的配置

```xml
1 依赖jar包管理，将所需的jar包，下载下来
<!-- 依赖jar包管理 -->
  <dependencies>
    <!-- dubbo    当前工程生产者（服务端） -->
  	<dependency>
  		<groupId>com.alibaba</groupId>
    	<artifactId>dubbo</artifactId>
    	<version>2.5.8</version>
  	</dependency>
  	<!-- 
  		zookeeper客户端  第三方的插件 
  		生产者通过zk将接口请求地址注册到 zk服务端进行管理 
  		方便消费者调用
  	-->
  	<dependency>
    	<groupId>org.apache.zookeeper</groupId>
   	 	<artifactId>zookeeper</artifactId>
    	<version>3.4.6</version>
	</dependency>
  	<dependency>
   	 	<groupId>com.github.sgroschupf</groupId>
    	<artifactId>zkclient</artifactId>
    	<version>0.1</version>
	</dependency>
	<!-- 依赖接口 将接口依赖过来 -->
	<dependency>
		<groupId>com.etoak</groupId>
  		<artifactId>dubbo-interface</artifactId>
  		<version>0.0.1-SNAPSHOT</version>
	</dependency>
  </dependencies>
```

#### [2] 实现接口

```Java
//生产者 提供服务 必须实现接口
//@Service//实例化对象  发布服务端
public class UserServiceImpl implements UserService {
	public String add(User user){
		System.out.println("生产者接收消费者请求参数!!!");
		System.out.println("用户名:" + user.getUsername());
		System.out.println("密码:" + user.getPassword());	
		return "生产者响应消费者信息：" + user.getUsername();
	}
}
```

#### [3] 服务端的xml文件的配置

```xml
 	  <!-- 去除当前配置文件中 dubbo:service  直接使用@Service-->
      <dubbo:annotation package="com.etoak" /> 
        <!--
        	 提供方应用信息，用于计算依赖关系  
        	 生产者
        	 为了监控当前服务是否处于正常运行状态
        	 一般命名  工程名叫什么应用名称就叫什么即可
        -->  
       <dubbo:application name="dubbo-provider" />
       <!-- 
       		使用zookeeper注册中心暴露服务地址
       	    dubbo
       	    	com.etoak.service.UserService
       				dubbo://localhost:20880?com.etoak.service.UserService
        		com.etoak.service.UserService1
       				dubbo://localhost:20880?com.etoak.service.UserService1
        -->
       <dubbo:registry address="zookeeper://127.0.0.1:2181" />
       <!-- 
       		用dubbo协议在20880端口暴露服务 
       		name属性表示设置协议  默认协议为dubbo协议  使用netty
       		port属性表示设置端口号  默认端口号为20880
       		dubbo:protocol底层自动获取本机host地址 （ip地址）
       		              只有生产者需要配置protocol元素
       		dubbo://localhost:20880
       -->  
       <dubbo:protocol name="dubbo" port="20882" />
        <!-- 
        	声明需要暴露的服务接口 
        	interface属性表示生产者需要暴露的接口，
        	                   暴露的接口，消费者就可以调用
        	ref属性表示发布生产者服务的实现类  ，
        		实现类不会暴露给消费者，
        		只有接口暴露给消费者
           	暴露接口
            com.etoak.service.UserService
        -->
        <dubbo:service interface="com.etoak.service.UserService" ref="userServiceImpl" /> 
        <!-- 实例化生产者实现类对象 -->
        <bean id="userServiceImpl"  class="com.etoak.service.UserServiceImpl" ></bean>
```

#### [4] 测试类(开启服务端)

```Java
public static void main(String[] args) {
		//启动spring 容器发布生产者服务
		ApplicationContext ac = new ClassPathXmlApplicationContext("provider.xml");
		((ClassPathXmlApplicationContext)ac).start();
		try {
			System.in.read();
		} catch (Exception e) {
			// TODO: handle exception
			e.printStackTrace();
		}
	}
```

### [3] 客户端的配置

#### [1] pom.xml的配置

```xml
1 客户端也需要依赖管理，下载和服务端相同的jar包，
<!-- 依赖jar包管理 -->
  <dependencies>
    <!-- dubbo    当前工程消费者（客户端） -->
  	<dependency>
  		<groupId>com.alibaba</groupId>
    	<artifactId>dubbo</artifactId>
    	<version>2.5.8</version>
  	</dependency>
  	<!-- 
  		消费者通过接口信息，去zookeeper服务端获取调用的url请求地址
  	-->
  	<dependency>
    	<groupId>org.apache.zookeeper</groupId>
   	 	<artifactId>zookeeper</artifactId>
    	<version>3.4.6</version>
	</dependency>
  	<dependency>
   	 	<groupId>com.github.sgroschupf</groupId>
    	<artifactId>zkclient</artifactId>
    	<version>0.1</version>
	</dependency>
	<!-- 生成者暴露接口是什么样   消费者接口也得是什么样 依赖接口 -->
	<dependency>
		<groupId>com.etoak</groupId>
  		<artifactId>dubbo-interface</artifactId>
  		<version>0.0.1-SNAPSHOT</version>
	</dependency>
  </dependencies>
```

#### [2] 服务端的xml文件配置

```xml
<!-- 去除当前配置文件中 dubbo:reference 直接使用@Reference-->
      <dubbo:annotation package="com.etoak" /> 
      <!-- 
      	消费方应用名，用于计算依赖关系，不是匹配条件，不要与提供方一样 
        	消费者
        	 为了监控当前消费者是否处于正常运行状态
        	 一般命名  工程名叫什么应用名称就叫什么即可
      -->
      <dubbo:application name="dubbo-consumer" />
      <!-- 
      		使用zookeeper注册中心暴露发现服务地址 
      		消费者通过接口去zookeeper服务中获取接口url请求地址
      		com.etoak.service.UserService
      			dubbo://localhost:20880?com.etoak.service.UserService
      -->    
      <dubbo:registry address="zookeeper://127.0.0.1:2181" />
      <!-- 
      		生成远程服务代理，可以和本地bean一样
      		interface属性表示 消费者调用生产者服务接口
       -->
      <dubbo:reference interface="com.etoak.service.UserService"  id="userService" timeout="5000" /> 
```

#### [3] 测试类(开启客户端)

```Java
public class ConsumerTest {
	public static void main(String[] args) {
		//启动spring容器 获取接口信息
		ApplicationContext ac = new ClassPathXmlApplicationContext("consumer.xml");
		//获取zookeeper中的url请求地址
		//获取生产者发布的接口
		UserService us = (UserService)ac.getBean("userService");
		//消费者调用生产者接口方法  动态代理
		User user = new User();
		user.setUsername("etoak");
		user.setPassword("et1803");
		String result = us.add(user);
		//输出返回响应信息
		System.out.println(result);
	}
}
```

