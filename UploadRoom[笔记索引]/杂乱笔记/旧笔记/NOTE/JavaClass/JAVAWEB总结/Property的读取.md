```java
InputStream inStream = 
			Prop.class
			 .getClassLoader()
			 .getResourceAsStream("com/etoak/prop/props.properties");
	
	//读取properties配置文件
	p.load(inStream);
	
	//读取配置文件key值 通过key获取value信息
	String etoak = p.getProperty("etoak");
	String et = p.getProperty("et");
	
	String et1803 = p.getProperty("et1803","etoak");
	
	//设置key value
	p.setProperty("key", "value");
	
	//输出信息
	System.out.println(etoak);
	System.out.println(et);
	System.out.println(et1803);
	System.out.println(p.getProperty("key"));
```