# Quartz

## 1 Quartz的含义

```
1 Quartz的意思是任务调度(定时任务)
2 定时任务是在指定的时间执行指定的业务逻辑
3 流程：
	1):任务调用核心的业务逻辑
	2):触发器在到规定的时间的时候调用任务
	3):调度器(监听器)监听触发器，在触发器的时间到了之后，调用触发器
4 注意：定时任务不能接收请求和响应操作
5 应用场景:定时任务一般用来和数据库打交道，用来同步数据的
```

## 2 Quartz的简单案例

### [1] 简单的Java工程

#### [1] 业务层

```Java
//定义一个业务类 这个类需要实现Job接口，才能实现一个定时的业务
public class Job1 implements Job{
	//execute 方法执行业务逻辑
  	//JobExecutionContext 定时的上下文对象  可以获取任务等信息
	//上下文对象 可以理解成一个全局的参数  一旦创建上下文对象，
	//在任何位置都可以获取上下文对象    类似HttpSession会话机制
    @Override
  	public void execute(JobExecutionCOntext context)throws Exception{
    	System.out.println("job1:" + new Date());
  	}
}
```

#### [2] 测试类

```Java
public class Test{
  public static void main(String[] args){
   	/*
   	  1 任务 通过任务调用业务
   		第一个参数表示任务的名称，任务的名称是唯一的
   		第二个参数表示任务组的名称，任务组的名称也是唯一的
   		第三个表示任务执行业务的操作
   	*/ 
    JobDetail jobDetail = new JobDetail("job_1","g_job1",Job.class);
    /*
      2 触发器
      	第一个参数表示触发器的名称，触发器的名称是唯一的
      	第二个参数表示触发器组的名称，触发器组的名称是唯一的
    */
    CronTrigger cron = new CronTrigger("trigger_1","g_trigger1");
    //核心 Cron表达式 设置时间在什么时候触发，每五秒触发一次
    cron.setCronExpression("0/5 * * * *　？");
    /*
      3 调度器(监听器)
    */
    SchedulerFactory sf = new SchedulerFactory();
    //Scheduler对象是调度器的核心对象
    //通过调度器工厂对象获取调度器对象
    Scheduler s = sf.getScheduler();
    //设置任务和触发器
    s.ScheduleJob(jobDetail,cron);
    //启动调度器 开启监听触发器的操作
    //调度器调用触发器 触发器调用任务 任务调用业务
    s.start();
  }
}
```

### [2] 和spring结合

#### [1] 业务层

```Java
//自定义业务
public class Job2 {
	//自定义业务方法  
	//不能定义方法参数 
	//不能接收请求和返回响应
	public void job() {
		System.out.println("自定义job：" + new Date());	
	}
}
```

#### [2] applicationContext.xml配置

```xml
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
	 <bean id="cronTriggerBean" class="org.springframework.scheduling.quartz.CronTriggerBean">
	        <!-- 调用任务 -->
	 		<property name="jobDetail" ref="jobDetail"></property>
	        <!-- cron表达式   设置触发时间 -->
	        <property name="cronExpression" value="0/5 * * * * ?"></property>
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
```

#### [3] web.xml配置

```xml
//配置监听器，当Tomcat开启的时候，加载applicationContext.xml配置文件
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
```

### [3] 和Spring结合(使用注解)

#### [1] 业务层

```Java
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

#### [2] applicationContext.xml配置

```xml
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
```

#### [3] web.xml配置

```xml
//配置监听器，当Tomcat开启的时候，加载applicationContext.xml配置文件
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
```

## 3 Quartz中的调度器的表达式

#### [1] 字段 允许值 允许特殊字符

```
秒 0-59 , - * /     
分 0-59 , - * /
小时 0-23 , - * /
日期 1-31 , - * ? / L W C
月份 1-12 或者 JAN-DEC , - * /
星期 1-7 或者 SUN-SAT , - * ? / L C #
年（可选） 留空, 1970-2099 , - * /
```

#### [2] 特殊字符的含义

```
“*”字符被用来指定所有的值。如："*"在分钟的字段域里表示“每分钟”。
“?”字符只在日期域和星期域中使用。这两个只有一个可以设置"?",不能同时出现,它被用来指定“非明确的值”。当你需要通过在这两个域中的一个来指定一些东西的时候，它是有用的。看下面的例子你就会明白。
月份中的日期和星期中的日期这两个元素时互斥的一起应该通过设置一个问号来表明不想设置那个字段。

“-”字符被用来指定一个范围。如：“10-12”在小时域意味着“10点、11点、12点”。

“,”字符被用来指定另外的值。如：“MON,WED,FRI”在星期域里表示”星期一、星期三、星期五”。

“/”字符用于指定增量。如：“0/15”在秒域意思是每分钟的0，15，30和45秒。“5/15”在分钟域表示每小时的5，20，35和50。符号“*”在“/”前面（如：*/10）等价于0在“/”前面（如：0/10）。记住一条本质：表达式的每个数值域都是一个有最大值和最小值的集合，如：秒域和分钟域的集合是0-59，日期域是1-31，月份域是1-12。字符“/”可以帮助你在每个字符域中取相应的数值。如：“7/6”在月份域的时候只有当7月的时候才会触发，并不是表示每个6月。

L是‘last’的省略写法可以表示day-of-month和day-of-week域，但在两个字段中的意思不同，例如day-of-month域中表示一个月的最后一天。如果在day-of-week域表示‘7’或者‘SAT’，如果在day-of-week域中前面加上数字，它表示一个月的最后几天，例如‘6L’就表示一个月的最后一个星期五。

字符“W”只允许日期域出现。这个字符用于指定日期的最近工作日。例如：如果你在日期域中写 “15W”，表示：这个月15号最近的工作日。所以，如果15号是周六，则任务会在14号触发。如果15号是周日，则任务会在周一也就是16号触发。如果是在日期域填写“1W”即使1号是周六，那么任务也只会在下周一，也就是3号触发，“W”字符指定的最近工作日是不能够跨月份的。字符“W”只能配合一个单独的数值使用，不能够是一个数字段，如：1-15W是错误的。

“L”和“W”可以在日期域中联合使用，LW表示这个月最后一周的工作日。

字符“#”只允许在星期域中出现。这个字符用于指定本月的某某天。例如：“6#3”表示本月第三周的星期五（6表示星期五，3表示第三周）。“2#1”表示本月第一周的星期一。“4#5”表示第五周的星期三。

字符“C”允许在日期域和星期域出现。这个字符依靠一个指定的“日历”。也就是说这个表达式的值依赖于相关的“日历”的计算结果，如果没有“日历”关联，则等价于所有包含的“日历”。如：日期域是“5C”表示关联“日历”中第一天，或者这个月开始的第一天的后5天。星期域是“1C”表示关联“日历”中第一天，或者星期的第一天的后1天，也就是周日的后一天（周一）。
```

#### [3] 表达式举例

```
"0 0 12 * * ?" 每天中午12点触发
"0 15 10 ? * *" 每天上午10:15触发
"0 15 10 * * ?" 每天上午10:15触发
"0 15 10 * * ? *" 每天上午10:15触发
"0 15 10 * * ? 2005" 2005年的每天上午10:15触发
"0 0 14 * * ?" 在每天下午2点到下午2:59期间的每1分钟触发
"0 0/5 14 * * ?" 在每天下午2点到下午2:55期间的每5分钟触发
"0 0/5 14,18 * * ?" 在每天下午2点到2:55期间和下午6点到6:55  60期间的每5分钟触发
"0 0-5 14 * * ?" 在每天下午2点到下午2:05期间的每1分钟触发
"0 10,44 14 ? 3 WED" 每年三月的星期三的下午2:10和2:44触发
"0 15 10 ? * MON-FRI" 周一至周五的上午10:15触发
"0 15 10 15 * ?" 每月15日上午10:15触发
"0 15 10 L * ?" 每月最后一日的上午10:15触发
"0 15 10 ? * 6L" 每月的最后一个星期五上午10:15触发
"0 15 10 ? * 6L 2002-2005" 2002年至2005年的每月的最后一个星期五上午10:15触发
"0 15 10 ? * 6#3" 每月的第三个星期五上午10:15触发
```

