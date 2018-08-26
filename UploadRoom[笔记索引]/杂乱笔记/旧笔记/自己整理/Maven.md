# Maven

## 1 Maven的目的

```
1 为Java提供的自动化构建工具---创建工程 管理jar包，不在需要导入jar包，需要从网上下载
2 maven基于jar包管理，基于多模块开发
```

## 2 Maven的配置流程

```
1 环境变量的配置
	新建系统变量名M2_HOME
		   变量值D:\yitupeixun\maven\apache-maven-3.3.9
			path %M2_HOME%\bin
2 测试maven是否配置成功
	打开cmd 输入 mvn -v
3 配置本地仓库
	存放下载的jar包
    maven首先找jar包 先去本地仓库查找
    再去网络（美国）下载jar包，下载后的jar在存放	       到本地仓库中
    在maven下新建一个文件夹 在maven下conf下的setting.xml中配置路径
    	<localRepository>D:/yitupeixun/maven/repo</localRepository>
4 配置镜像私服
	maven首先找jar包 先去本地仓库查找
                再去镜像私服下载jar包 然后存放到本地仓库
                私服没有找到jar包， 再去网络（美国）下载jar			包，下载后的jar在存放到本地仓库中
    在maven下conf下的setting.xml中配置路径
   	<mirror>
      <id>alimaven</id>
      <name>aliyun maven</name>
      <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
      <mirrorOf>central</mirrorOf>
    </mirror>
```

## 3 maven的核心数据

```
1 maven默认的是jdk1.5
2 maven的核心文件是pom.xml
3 src/main/java 核心业务代码
  src/main/resources 核心配置文件 xml properties
4 src/test/java 测试  junit单元测试
  src/test/resources 测试配置文件 xml properties
5 打包时 jar包、war包 
        test资源包下的代码不会打入包中
6  target 缓存信息
7 pom.xml    管理jar包
```

## 4 maven的范围作用域

```
1 compile：默认的是 当前的jar包 编译 运行的时候使用 打包的时候会存放到包中
2 provided：当前jar包 编译运行的时候使用 打包的时候不会存放到包中
```

## 5 maven的命令

```
1 (mvn) compile 编译下载jar包的时候使用 main
2 (mvn) test-compile 编译下载jar包的时候使用 test
3 (mvn) package 打包的时候使用 <packaging>war(jar)</packaging>
	是什么包就打成什么包 jar包 war包
4 (mvn) install 等价于 mvn compile 和 mvn package
	编译、下载jar包、将当前
                工程打包 jar、war存放到本地仓库中
5 (mvn) clean 清除缓存 target中的缓存
```

## 6 配置eclipse的maven

```
1 点击window下的preferences，进入preferences
2 点击Maven下的Installations，添加路径
3 User-settings 配置Setting.xml的路径，下面的自动生成
```

## 7 Maven中的依赖

```
1 maven中的依赖是指在maven创建的工程之间可以互相依赖，
2 jar可以依赖于jar，war可以依赖于jar，但是war不可以依赖于war
3 依赖工程 可以依赖多个 也可以理解为
		  A依赖B A依赖C B和C的顺序 谁在前面先执行
			A依赖B B依赖C  ABC
			A依赖D AD 
				谁的路程短谁先执行
```

## 8 Maven中的继承

```xml 
1 maven中的继承是单继承的，只能继承一个
2 父maven的类型只能是pom，子maven可以是jar和war
3 在父maven设置的自定义属性，在子maven中可以继承下来
	<!--dependencyManagement管理jar包 默认子工程不会被继承,可以自由的选择在子工程中继承那个--->
   <dependencyManagement>
	  <dependencies>
	  	<dependency>
		    <groupId>org.springframework</groupId>
		    <artifactId>spring-core</artifactId>
		    <version>${spring.vesion}</version>
		</dependency>
		
		<dependency>
		    <groupId>org.springframework</groupId>
		    <artifactId>spring-beans</artifactId>
		    <version>${spring.vesion}</version>
		</dependency>
	  </dependencies>
  </dependencyManagement>
--------------------------------子工程-------------------------------------------
  <dependencies>
  		<!-- 使用父工程jar包 只需要去除vesion元素即可 -->
  		<dependency>
		    <groupId>org.springframework</groupId>
		    <artifactId>spring-core</artifactId>
		</dependency>
  </dependencies>
```

## 9 Maven中的聚合

```xml
1 聚合的意思就是讲一个项目分为多个子项目，每个子项目都可以独立运行，把每个子项目结合在一起，合成一个大项目也		可运行
2 聚合时的必须有个工程是pom，其他的工程都继承这个pom
3 所有子工程的中，写核心文件的那个必须是war类型的
----------------------------------主工程--------------------------------------------
                  	<modules>
                      <module>et-bean</module>
                      <module>et-mapper</module>
                      <module>et-controller</module>
                    </modules>
-------------------------------------子工程-----------------------------------------------
                      <parent>
                        <groupId>com.etoak</groupId>
                        <artifactId>et-manager</artifactId>
                        <version>0.0.1-SNAPSHOT</version>
                      </parent>
                      <artifactId>et-mapper</artifactId>
---------------------------------------核心子工程---------------------------------------------
                       <parent>
                          <groupId>com.etoak</groupId>
                          <artifactId>et-manager</artifactId>
                          <version>0.0.1-SNAPSHOT</version>
                        </parent>
                        <artifactId>et-controller</artifactId>
  					    <packaging>war</packaging>
```

# Maven-Java工程

## 1 pro.xml配置

```xml
1 仓库坐标
    <!-- 仓库坐标 groupId + artifactId + version -->
    <!-- 组id -->
    <groupId>com.etoak</groupId>
    <!-- 工程名 -->
    <artifactId>testMaven</artifactId>
    <!-- 工程版本号 -->
    <version>1.0.0-SNAPSHOT</version>
2 工程的信息
    <!-- 描述工程信息 -->
    <name>test</name>
    <description>测试</description>
3 自定义属性
	<properties>
  		<spring.version>4.3.10.RELEASE</spring.version>
  		<!-- 本机环境变量 -->
  		<maven.compiler.source>1.7</maven.compiler.source>
  		<!-- 将项目jdk版本替换成本机环境变量 之后进行强制更新-->
  		<maven.compiler.target>1.7</maven.compiler.target>
  	</properties>
4 依赖管理jar包，下载jar包 也可以去除连带下载下来的jar包
	<dependencies>
  	<dependency>
	    <groupId>org.springframework</groupId>
	    <artifactId>spring-core</artifactId>
	    <version>${spring.version}</version>
	    <!-- 去除jar包 -->
	    <exclusions>
	    	<exclusion>
	    		<groupId>commons-logging</groupId>
	    		<artifactId>commons-logging</artifactId>
	    	</exclusion>
	    </exclusions>
	</dependency>
      <!--
		1 依赖工程 可以依赖多个 也可以理解为
		  A依赖B A依赖C B和C的顺序 谁在前面先执行
			A依赖B B依赖C  ABC
			A依赖D AD 
				谁的路程短谁先执行
		-->
    <dependency>
		<groupId>com.etoak</groupId>
  		<artifactId>et1803-2</artifactId>
  		<version>0.0.1-SNAPSHOT</version>
	</dependency>
	<dependency>
		<groupId>com.etoak</groupId>
  		<artifactId>et1803-1</artifactId> 
  		<version>0.0.1-SNAPSHOT</version>
	</dependency>
      
  </dependencies>
```

## 2 核心业务逻辑

### [1] testMaven

```java 
1 com/main/java 
	com.etoak.testmaven
	public static void main(String[] args) {
		Test t = new Test();
		t.test();
	}
```

### [2] et1803-1

```java
1 com/main/java 
	com.etoak.test
	public void test(){
		System.out.println("我是et1803-1工程");
	}
```

### [3] et1803-2

```java
1 com/main/java 
	com.etoak.test
	public void test(){
		System.out.println("我是et1803-2工程");
	}
```

# Maven-web工程

## 1 pro.xml配置

``` xml
1 自定义属性- 修改自带的jdk版本-两种方法 
  1):<!-- 自定义属性 -->
    <properties>
      <maven.compiler.source>1.7</maven.compiler.source>
      <maven.compiler.target>1.7</maven.compiler.target>
    </properties>
  2):<!-- 定义插件 -->
      <build>
            <!-- 打包最后的项目名称 -->
            <finalName>et1803</finalName>
            <!-- 插件 -->
            <plugins>
                <!-- compiler插件, 设定JDK版本 -->
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.1</version>
                    <configuration>
                        <source>1.7</source>
                        <target>1.7</target>
                    </configuration>
                </plugin>
            </plugins>
      </build>
2 依赖jar包管理-打包的时候根据范围作用域来进行
	<!-- 依赖jar包管理 -->
  <dependencies>
  	<dependency>
	    <groupId>javax.servlet</groupId>
	    <artifactId>servlet-api</artifactId>
	    <version>2.5</version>
	    <!-- 
	    	compile  ：默认 当前jar包 编译、运行时使用 打包时会存放到包中
	    	provided ：当前jar包 编译、运行时使用 打包时不会存放到包中 -->
	    <scope>provided</scope>
	</dependency>
    <dependency>
	    <groupId>com.etoak</groupId>
	    <artifactId>et1803</artifactId>
	    <version>1.5</version>
	    <!-- 
	    	 范围作用域使用system 必须使用<systemPath>引入第三方jar包
	    	   编译、运行时使用 打包时会存放到包中 
	    -->
	    <scope>system</scope>
	    <systemPath>${basedir}/src/main/webapp/WEB-INF/lib/commons-logging-1.1.3.jar</systemPath>
	</dependency>
  </dependencies>
```

## 2 生成web.xml文件

```
1 右击web工程名，点击最后的properties
2 进入properties，点击project Facets，先去掉Dynamic Web Module，修改Java的jdk，
		然后点击Dynamic Web Module
3 会在下边产生一个链接，点击进去生成web.xml文件
```

