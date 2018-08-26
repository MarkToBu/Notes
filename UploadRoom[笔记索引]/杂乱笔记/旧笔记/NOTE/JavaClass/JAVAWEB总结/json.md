Json 介绍

##  1.0 介绍

```
  * 	JavaScrip Object Notation
		 *  JSON
		 *  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~
		 *  被称之为Java的常用的第十种数据类型
		 *  一般在服务器端创建封装数据，在页面解析数据
		 *  Json其实是一种特殊格式的字符串，可以直接打印
		 *  MINE:text/plain
		 *  Json在2006年加入ajax技术体系之后替代了
		 *  xml，专门用来封装数据
		 *  JSON存在两种格式
		 *  
		 *  1):{key:value,key:value,key:value***}
		 *  key:必须是字符串
		 *  value:
		 *  	八种基本数据类型
		 *  	字符串
		 *  	自定义数据类型
		 *  	数组
		 *  	集合
		 *  	null
		 *  	json
		 *  2):[value,value,value***]
		 *  value:
		 *  	八种基本数据类型
		 *  	字符串
		 *  	自定义数据类型
		 *  	数组
		 *  	集合
		 *  	null
		 *  	json
		 * 
		 * 
		 * 封装方式1：
		 * 		可以封装任意数据类型
				封装之后呈现格式1的json
		 * */
```

## 2.0 简单实用示例

```java
package com.etoak.test;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.HashSet;
import java.util.List;
import java.util.Map;
import java.util.Set;

import net.sf.json.JSONArray;
import net.sf.json.JSONObject;
import net.sf.json.JsonConfig;

import com.etoak.po.Game;

public class TestJson {
	public static void main(String[] args) {
	
		int i = 10;
		boolean flag = true;
		String str = "etoak";
		String[] strAr = {"大明湖","趵突泉","千佛山"};
		
		JSONObject jo = new JSONObject();
		jo.put("key1",i);
		jo.put("key2",flag);
		jo.put("key3",str);
		jo.put("key4",strAr);
		//{"key4":["大明湖","趵突泉","千佛山"],"key3":"etoak","key2":true,"key1":10}
		System.out.println(jo);
		
		JSONObject jo2 = new JSONObject();
		jo2.put("mygame",
				new Game("炉石传说",0,"暴雪"));
		//{"mygame":{"total":0,"name":"炉石传说","seller":"暴雪"}}
		System.out.println(jo2);
		///////////////////////////////////////////////
		//封装方式2
		//专门用来封装自定义数据类型和Map
		Map<Integer,String> map =
				new HashMap<Integer,String>();
		map.put(1, "山东鲁能");
		map.put(2, "广州恒大");
		map.put(3, "上海上港");
		map.put(4, "北京国安");
		JSONObject js1 = JSONObject.fromObject(map);
		//{"3":"上海上港","2":"广州恒大","1":"山东鲁能","4":"北京国安"}
		System.out.println(js1);
		
		JSONObject js2 = JSONObject.fromObject(
				new Game("堡垒之夜",98,"腾讯"));
		//{"total":98,"name":"堡垒之夜","seller":"腾讯"}
		System.out.println(js2);
		
		//对自定义数据类型进行有选择的封装
		JsonConfig jc = new JsonConfig();
		//设置不封装的属性名
		jc.setExcludes(new 
		String[]{"total","seller"});
		JSONObject js3 = JSONObject.fromObject(
		new Game("守望先锋",128,"暴雪"),jc);
		//{"name":"守望先锋"}
		System.out.println(js3);
		//////////////////////////////////////////
		//封装方式3
		//用来封装数组 List 和 Set
		//封装之后呈现格式2的json
		JSONArray ja = JSONArray.fromObject(strAr);
		//["大明湖","趵突泉","千佛山"]
		System.out.println(ja);
		
		List<String> list = new ArrayList<String>();
		list.add("Java");
		list.add("C");
		list.add("C++");
		list.add("Php");
		JSONArray ja2 = JSONArray.fromObject(list);
		//["Java","C","C++","Php"]
		System.out.println(ja2);
		
		Set<String> set = new HashSet<String>();
		set.add("济南");
		set.add("青岛");
		set.add("淄博");
		set.add("济宁");
		JSONArray ja3 = JSONArray.fromObject(set);
		//["青岛","济宁","淄博","济南"]
		System.out.println(ja3);
		
	}
} 
```

## json过滤

```java
我们通常对一个Json串和java对象进行互转时，经常会有选择性的过滤掉一些属性值，而json-lib包中的JsonConfig为我们提供了这种功能，具体实现方法有以下几种。(1)建立JsonConfig实例，并配置属性排除列表,(2)用属性过滤器,(3)写一个自定义的 JsonBeanProcessor.

1. 实现JSONString接口的方法 

public class Person implements JSONString {  
private String name;  
private String lastname;  
private Address address;  

// getters & setters  

public String toJSONString() {
return "{name:'"+name+"',lastname:'"+lastname+"'}";
}
}


2.第二种方法通过jsonconfig实例，对包含和需要排除的属性进行方便的添加或删除 

public class Person {  
private String name;  
private String lastname;  
private Address address;  

// getters & setters  
}  

JsonConfig jsonConfig = new JsonConfig();  
jsonConfig.setExclusions( new String[]{"address"});  
Person bean = new Person("jack","li");  
JSON json = JSONSerializer.toJSON(bean, jsonConfig);  

3. 使用propertyFilter可以允许同时对需要排除的属性和类进行控制，这种控制还可以是双向的，也可以应用到json字符串到java对象 

public class Person {  
private String name;  
private String lastname;  
private Address address;  

// getters & setters  
}  

JsonConfig jsonConfig = new JsonConfig();  
jsonConfig.setJsonPropertyFilter( new PropertyFilter(){  

public boolean apply(Object source/* 属性的拥有者 */ , String name /*属性名字*/ , Object value/* 属性值 */ ){  
// return true to skip name  
return source instanceof Person && name.equals("address");  
}  
});  
Person bean = new Person("jack","li"); 
JSON json = JSONSerializer.toJSON( bean, jsonConfig )  

4. 最后来看JsonBeanProcessor,这种方式和实现JsonString很类似，返回一个代表原来的domain类的合法JSONObject 

public class Person {  
private String name;  
private String lastname;  
private Address address;  

// getters & setters  
}  

JsonConfig jsonConfig = new JsonConfig();  
jsonConfig.registerJsonBeanProcessor( Person.class, new JsonBeanProcessor(){  

public JSONObject processBean( Object bean, JsonConfig jsonConfig ){  
if(!(bean instanceof Person)){  
return new JSONObject(true);  
}  
Person person = (Person) bean;  
return new JSONObject() .element( "name", person.getName()) .element( "lastname", person.getLastname());  
}  
});  

Person bean = new Person("jack","li"); 
JSON json = JSONSerializer.toJSON( bean, jsonConfig );
```

```
.rows.\[\d+\]\.bookpics,.rows.\[\d+\]\.category.books
```