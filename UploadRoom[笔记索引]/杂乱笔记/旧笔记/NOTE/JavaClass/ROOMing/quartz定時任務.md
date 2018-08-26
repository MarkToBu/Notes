##  Quartz 学习

**1. 介绍**  

Quartz是OpenSymphony开源组织在Job scheduling领域又一个开源项目，是完全由java开发的一个开源的任务日程管理系统，“任务进度管理器”就是一个在预先确定（被纳入日程）的时间到达时，负责执行（或者通知）其他软件组件的系统。  Quartz用一个小Java库发布文件（.jar文件），这个库文件包含了所有Quartz核心功能。这些功能的主要接口(API)是Scheduler接口。它提供了简单的操作，例如：将任务纳入日程或者从日程中取消，开始/停止/暂停日程进度。 

​     Quartz是一个任务调度框架。 在某一个有规律的时间点干某件事。 

**2. 定时器种类**  Quartz 中五种类型的 Trigger：SimpleTrigger，CronTirgger，DateIntervalTrigger，NthIncludedDayTrigger和Calendar 类（ org.quartz.Calendar）。  最常用的：  SimpleTrigger：用来触发只需执行一次或者在给定时间触发并且重复N次且每次执行延迟一定时间的任务。  CronTrigger：按照日历触发，例如“每个周五”，每个月10日中午或者10：15分。  

**5. 核心类和关系**

1. 核心类 
   （1）核心类 
   **QuartzSchedulerThread** ：负责执行向QuartzScheduler注册的触发Trigger的工作的线程。 
   **ThreadPool**：Scheduler使用一个线程池作为任务运行的基础设施，任务通过共享线程池中的线程提供运行效率。 
   **QuartzSchedulerResources**：包含创建QuartzScheduler实例所需的所有资源（JobStore，ThreadPool等）。 
   **SchedulerFactory** ：提供用于获取调度程序实例的客户端可用句柄的机制。 
   **JobStore**： 通过类实现的接口，这些类要为org.quartz.core.QuartzScheduler的使用提供一个org.quartz.Job和org.quartz.Trigger存储机制。作业和触发器的存储应该以其名称和组的组合为唯一性。 
   **QuartzScheduler** ：这是Quartz的核心，它是org.quartz.Scheduler接口的间接实现，包含调度org.quartz.Jobs，注册org.quartz.JobListener实例等的方法。 
   **Scheduler** ：这是Quartz Scheduler的主要接口，代表一个独立运行容器。调度程序维护JobDetails和触发器的注册表。 一旦注册，调度程序负责执行作业，当他们的相关联的触发器触发（当他们的预定时间到达时）。 
   **Trigger** ：具有所有触发器通用属性的基本接口，描述了job执行的时间出发规则。 - 使用TriggerBuilder实例化实际触发器。 
   **JobDetail** ：传递给定作业实例的详细信息属性。 JobDetails将使用JobBuilder创建/定义。 
   **Job**：要由表示要执行的“作业”的类实现的接口。只有一个方法 void execute(jobExecutionContext context) 
   (jobExecutionContext 提供调度上下文各种信息，运行时数据保存在jobDataMap中) 
   Job有个子接口StatefulJob ,代表有状态任务。 
   有状态任务不可并发，前次任务没有执行完，后面任务处于阻塞等到。

## 1.2 Quartz特点

​       作为一个优秀的开源调度框架，Quartz 具有以下特点：

1. 强大的调度功能，例如支持丰富多样的调度方法，可以满足各种常规及特殊需求；
2. 灵活的应用方式，例如支持任务和调度的多种组合方式，支持调度数据的多种存储方式；
3. 分布式和集群能力，Terracotta 收购后在原来功能基础上作了进一步提升。

​      另外，作为 Spring 默认的调度框架，Quartz 很容易与 Spring 集成实现灵活可配置的调度功能。

​    **quartz调度核心元素**：

1. Scheduler:任务调度器，是实际执行任务调度的控制器。在spring中通过SchedulerFactoryBean封装起来。
2. Trigger：触发器，用于定义任务调度的时间规则，有SimpleTrigger,CronTrigger,DateIntervalTrigger和NthIncludedDayTrigger，其中CronTrigger用的比较多，本文主要介绍这种方式。CronTrigger在spring中封装在CronTriggerFactoryBean中。
3. Calendar:它是一些日历特定时间点的集合。一个trigger可以包含多个Calendar，以便排除或包含某些时间点。
4. JobDetail:用来描述Job实现类及其它相关的静态信息，如Job名字、关联监听器等信息。在spring中有JobDetailFactoryBean和 MethodInvokingJobDetailFactoryBean两种实现，如果任务调度只需要执行某个类的某个方法，就可以通过MethodInvokingJobDetailFactoryBean来调用。
5. Job：是一个接口，只有一个方法void execute(JobExecutionContext context),开发者实现该接口定义运行任务，JobExecutionContext类提供了调度上下文的各种信息。Job运行时的信息保存在JobDataMap实例中。实现Job接口的任务，默认是无状态的，若要将Job设置成有状态的，在quartz中是给实现的Job添加@DisallowConcurrentExecution注解（以前是实现StatefulJob接口，现在已被Deprecated）,在与spring结合中可以在spring配置文件的job detail中配置concurrent参数。

##  示例

这里面的例子都是基于Quartz 2.2.1

```
package com.test.quartz;

import static org.quartz.DateBuilder.newDate;
import static org.quartz.JobBuilder.newJob;
import static org.quartz.SimpleScheduleBuilder.simpleSchedule;
import static org.quartz.TriggerBuilder.newTrigger;

import java.util.GregorianCalendar;

import org.quartz.JobDetail;
import org.quartz.Scheduler;
import org.quartz.Trigger;
import org.quartz.impl.StdSchedulerFactory;
import org.quartz.impl.calendar.AnnualCalendar;

public class QuartzTest {

    public static void main(String[] args) {
        try {
            //创建scheduler
            Scheduler scheduler = StdSchedulerFactory.getDefaultScheduler();

            //定义一个Trigger
            Trigger trigger = newTrigger().withIdentity("trigger1", "group1") //定义name/group
                .startNow()//一旦加入scheduler，立即生效
                .withSchedule(simpleSchedule() //使用SimpleTrigger
                    .withIntervalInSeconds(1) //每隔一秒执行一次
                    .repeatForever()) //一直执行，奔腾到老不停歇
                .build();

            //定义一个JobDetail
            JobDetail job = newJob(HelloQuartz.class) //定义Job类为HelloQuartz类，这是真正的执行逻辑所在
                .withIdentity("job1", "group1") //定义name/group
                .usingJobData("name", "quartz") //定义属性
                .build();

            //加入这个调度
            scheduler.scheduleJob(job, trigger);

            //启动之
            scheduler.start();

            //运行一段时间后关闭
            Thread.sleep(10000);
            scheduler.shutdown(true);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

```
package com.test.quartz;

import java.util.Date;

import org.quartz.DisallowConcurrentExecution;
import org.quartz.Job;
import org.quartz.JobDetail;
import org.quartz.JobExecutionContext;
import org.quartz.JobExecutionException;

public class HelloQuartz implements Job {
    public void execute(JobExecutionContext context) throws JobExecutionException {
        JobDetail detail = context.getJobDetail();
        String name = detail.getJobDataMap().getString("name");
        System.out.println("say hello to " + name + " at " + new Date());
    }
}
```

这个例子很好的覆盖了Quartz最重要的3个基本要素：

- Scheduler：调度器。所有的调度都是由它控制。
- Trigger： 定义触发的条件。例子中，它的类型是SimpleTrigger，每隔1秒中执行一次（什么是SimpleTrigger下面会有详述）。
- JobDetail & Job： JobDetail 定义的是任务数据，而真正的执行逻辑是在Job中，例子中是HelloQuartz。 为什么设计成JobDetail + Job，不直接使用Job？这是因为任务是有可能并发执行，如果Scheduler直接使用Job，就会存在对同一个Job实例并发访问的问题。而JobDetail & Job 方式，sheduler每次执行，都会根据JobDetail创建一个新的Job实例，这样就可以规避并发访问的问题。

## 实例二 :  单独使用

业务类

```java
package com.etoak.job;

import java.util.Date;

import org.quartz.Job;
import org.quartz.JobExecutionContext;
import org.quartz.JobExecutionException;

//定义业务    该类需要实现job接口，才能实现一个定时的业务
public class Job1 implements Job {

	//execute方法执行业务逻辑
	//JobExecutionContext 定时的上下文对象  可以获取任务等信息
	//上下文对象 可以理解成一个全局的参数  一旦创建上下文对象，
	//在任何位置都可以获取上下文对象    类似HttpSession会话机制
	@Override
	public void execute(JobExecutionContext context) 
				throws JobExecutionException {
		
		System.out.println("job1:" + new Date());
		
	}

}
```

- 调用

```java
package com.etoak.test;

import org.quartz.CronTrigger;
import org.quartz.JobDetail;
import org.quartz.Scheduler;
import org.quartz.SchedulerFactory;
import org.quartz.impl.StdSchedulerFactory;

import com.etoak.job.Job1;

public class Test {

	public static void main(String[] args) throws Exception {
		
		//1.任务     任务调用业务
		//第一个参数表示任务名称        任务名称命名 唯一      spring默认id属性别名
		//第二个参数表示任务组名称     任务组名称命名 唯一  spring默认DEFAULT
		//第三个参数表示任务执行业务操作
		//     g_job1    g_jbo2
		//         job_1      job_1
		//         job_2      job_2
		//                    job_3
		JobDetail jobDetail = 
				new JobDetail("job_1", "g_job1", Job1.class);
		
		//2.触发器
		//第一个参数表示触发器名称    触发器名称唯一    spring默认id属性别名
		//第二个参数表示触发器组名称  触发器组名称唯一 spring默认DEFAULT
		CronTrigger cron = 
				new CronTrigger("trigger_1", "g_trigger1");
		
		//核心  cron表达式  设置在什么时间触发
		cron.setCronExpression("0/5 * * * * ?");
		
		//3.调度器  监听器
		SchedulerFactory sf = new StdSchedulerFactory();
		//Scheduler对象是调度器中的核心对象
		//通过调度器工厂对象获取调度器对象
		Scheduler s = sf.getScheduler();
		
		//设置任务和触发器
		s.scheduleJob(jobDetail, cron);
		
		//启动调度器  开启监听触发器操作
		//调度器调用触发器   触发器调用任务  任务调用业务
		s.start();
	}
}
```

##  实例三  :(xml版)与Spring整合

- 业务类

```
package com.etoak.job;

import java.util.Date;

public class Job2 {
   //自定义义务方法    不能定义方法参数  不能接受请求参数，接受请求和返回响应
	public void job() {
		System.out.println("自定义job：" + new Date());
	}
}

```

- 在web.xml中配置监听器

```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns="http://java.sun.com/xml/ns/javaee"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
	id="WebApp_ID" version="3.0">
	<display-name>Spring-quartz</display-name>
	<welcome-file-list>
		<welcome-file>index.html</welcome-file>
		<welcome-file>index.htm</welcome-file>
		<welcome-file>index.jsp</welcome-file>
	</welcome-file-list>
	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>classpath:applicationContext.xml</param-value>
	</context-param>
</web-app>
```

- 书写spring的配置文件   applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-4.0.xsd">
	
	<!-- 
		实例化业务对象
	 -->
	 <bean id="job2" class="com.etoak.job.Job2" />
	 
	 <!-- 
	 	1.任务     任务调用业务
	    
	         默认任务名称         id属性别名
	         默认任务组名称      DEFAULT
	  -->
	  <bean id="jobDetail" 
	        class="org.springframework.scheduling.quartz.MethodInvokingJobDetailFactoryBean">
	  	    <!-- 引入业务对象 -->
	  		<property name="targetObject" ref="job2"></property>
	  		<!-- 调用业务方法 -->
	  		<property name="targetMethod" value="job"></property>
	  		<!-- 任务名称 -->
	  		<property name="name" value="job_2"></property>
	  		<!-- 任务组名称 -->
	  		<property name="group" value="g_job2"></property>
	  		<!-- 
	  			解决并发操作 
	  			true ：异步操作
	  			false：同步操作
	  		-->
	  		<property name="concurrent" value="false"></property>
	  </bean>
	  
	<!-- 
		2.触发器   触发器调用任务
		默认触发器名称         id属性别名
		默认触发器组名称     DEFAULT
		
		g_trigger2
			trigger_2
			  	g_job2
			    	job_2
			    g_job3
			        job_3
	 -->
	 <bean id="cronTriggerBean" 
	 	   class="org.springframework.scheduling.quartz.CronTriggerBean">
	        <!-- 调用任务 -->
	 		<property name="jobDetail" ref="jobDetail"></property>
	        <!-- cron表达式   设置触发时间 -->
	        <property name="cronExpression" 
	        			value="0/5 * * * * ?"></property>
	        <!-- 触发器名称 -->
	        <property name="name" value="trigger_2"></property>
	        <!-- 触发器组名称 -->
	        <property name="group" value="g_trigger2"></property>
	 </bean>
	 
	 <!-- 
	 	3.调度器  监听器         调度器调用触发器（监听）
	 	
	 	实例化之后开启调度器（监听）
	  -->
	  <bean class="org.springframework.scheduling.quartz.SchedulerFactoryBean">
	  		<!-- 调用触发器，并监听触发器cron时间表达式 -->
	  		<property name="triggers">
	  			<array>
	  				<ref bean="cronTriggerBean" />
	  			</array>
	  		</property>
	  </bean>

</beans>
```

部署后自动执行，每间隔5秒

##  实例三（anno版）与spirng整合

 注意：注解版有其局限性，任务的名字无法设置。注意导入spring注解包， 

- 业务逻辑

```java
package com.etoak.job;

import java.util.Date;

import org.springframework.scheduling.annotation.Scheduled;
import org.springframework.stereotype.Component;

@Component
public class Job3 {
	
	//自定义业务方法  
	//不能定义方法参数 
	//不能接收请求和返回响应
	@Scheduled(cron="0/5 * * * * ?")//触发器 cron="0/5 * * * * ?
	public void job() {
		System.out.println("自定义job-注解：" + new Date());
	}

}
```

- 业务中的实体类  (stu)

```
package com.etoak.spring;

public class Student {
	
	private Integer id;
	private String name;
	private Integer age;
	
	public Integer getId() {
		return id;
	}
	public void setId(Integer id) {
		this.id = id;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public Integer getAge() {
		return age;
	}
	public void setAge(Integer age) {
		this.age = age;
	}

}

```

- 注解版的配置文件 （.java）

```java
package com.etoak.spring;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Scope;

/**
 * @Configuration将当前对象实例化
 *               替代xml配置文件
 *               等价xml配置文件中<beans>元素
 *      
 */
@Configuration
public class SpringConfig {
	
	/**
	 * @Bean等价xml配置文件中<bean>元素
	 *          方法的返回值就spring要实例化的对象 默认为单例
	 *          如果该方法不加@Bean注解，调用该方法时表示没有交给spring管理
	 *          每次都会实例化一个新的内存地址
	 *          
	 *          默认别名为方法名称   区分大小写 例如 getStu
	 *          自定义别名
	 *          例如  @Bean(name="stu")
	 *         
	 */
	@Scope("prototype")
	@Bean(name="stu")
	public Student getStu() {
		//实例化对象
		Student stu = new Student();
		
		//setter方法注入
		stu.setId(1);
		stu.setName("易途");
		stu.setAge(10);
		
		//spring通过返回值来实例化对象
		return stu;
	}

}
```

- spring的配置文件

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:task="http://www.springframework.org/schema/task"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context-4.0.xsd
       http://www.springframework.org/schema/task
       http://www.springframework.org/schema/task/spring-task-4.0.xsd">
	
	
	<!-- 
		扫描ioc注解
		@Controller @Service @Repository @Component
		@Autowired
	 -->
	<context:component-scan base-package="com.etoak" />
	
	<!-- 
		扫描任务调度注解 @Scheduled   调度器 监听
	-->
	<task:annotation-driven proxy-target-class="true" />
</beans>
       
```

- 测试

```java
package com.etoak.spring;

import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class SpringTest {

	public static void main(String[] args) {
		//spring容器 启动加载SpringConfig类中@Configuration、@Bean
		AnnotationConfigApplicationContext ac =
		new AnnotationConfigApplicationContext(SpringConfig.class);
		
		//获取实例化对象
		Student stu = (Student)ac.getBean("stu");
		
		//输出信息
		System.out.println(stu.getId());
		System.out.println(stu.getName());
		System.out.println(stu.getAge());
		
		//----------------------
		for(int i = 1; i <= 10; i++) {
			System.out.println(ac.getBean("stu"));
		}
	}

}

```

- xml配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd" id="WebApp_ID" version="3.0">
  <display-name>Spring-quartz</display-name>
 
 <listener>
 	<listener-class>
 		org.springframework.web.context.ContextLoaderListener
 	</listener-class>
 </listener>
 
 <context-param>
 	<param-name>contextConfigLocation</param-name>
 	<param-value>
 		classpath:applicationContext*.xml
 	</param-value>
 </context-param>
 
</web-app>
```







