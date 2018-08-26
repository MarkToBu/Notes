##  DOM4J

## 1.0 简介

一种由第三方提供的使用Java代码来读取xml文件或者创建xml文件的
技术，由于其简单易用，取代了Java的官方组件，被各种框架使用
使用dom4j，可以读取已经存在xml文件中内容，也可以通过运行
Java代码来创建一个新的xml文件，将数据封装进xml

Dom4j并不是Java官方组件，必须导入第三方Jar包



## 1.1 使用

### 读XML     --ReadXml.java

```java
package com.etoak.test;


import java.io.File;
import java.util.List;

import org.dom4j.Attribute;
import org.dom4j.Document;
import org.dom4j.Element;
import org.dom4j.io.SAXReader;

public class ReadXml {

	public static void main(String[] args) {
		//使用Java代码来读取一个已经存在xml文件
		//1:拿取Dom4j中的解析器
		/*
		 * Sample API Xml
		 * API:application program interface
		 * 应用程序接口
		 * */
		try{
			SAXReader sax = new SAXReader();
			//2:使用解析器读取xml文档，拿取文档对象模型
			Document doc = sax.read(new File("etoak1.xml"));
			//3:拿取根元素
			Element root = doc.getRootElement();
			System.out.println("根元素是~~~~~》"
			+root.getName());
			//4:拿取所有的一级子元素
			List<Element> firstChild = 
					root.elements();
			//5:遍历所有的一级子元素
			for(Element firstChildEle:firstChild){
				System.out.println("一级子元素是~~~~~》"
						+firstChildEle.getName());
			
				//6:拿取每一个一级子元素的所有属性
				List<Attribute> firstChildAttr = 
						firstChildEle.attributes();
			
				//7:遍历所有的属性
				for(Attribute firstChildAttrEle:firstChildAttr){
					System.out.println("一级子元素属性名是~~~》"
							+firstChildAttrEle.getName());
					System.out.println("一级子元素属性值是~~~》"
							+firstChildAttrEle.getValue());
				}
				
				//8:使用每一个一级子元素拿取二级子元素
				List<Element> secondChild = 
						firstChildEle.elements();
				
				for(Element secondChildEle:secondChild){
					System.out.println("二级子元素名是~~~~~》"
							+secondChildEle.getName());
					System.out.println("二级子元素内部嵌套的值是~~~~~》"
							+secondChildEle.getText());
				}
				/*
				 * 	elements():返回List 父元素拿取所有子元素得集合
				 *  attributes():返回List 元素拿取所有属性的集合
				 *  
				 *  getName():拿取名字
				 *  getValue():拿取值
				 *  getText():拿取标签内嵌套的文本
				 * 
				 * */		
			
			}
			
		}catch(Exception ex){
			ex.printStackTrace();
		}
	}
}
```

###  写xml文档  	WriteXml.java

```java
package com.etoak.test;

import java.io.FileOutputStream;
import java.io.OutputStream;

import org.dom4j.Document;
import org.dom4j.DocumentHelper;
import org.dom4j.Element;
import org.dom4j.io.OutputFormat;
import org.dom4j.io.XMLWriter;

public class WriteXml {

	public static void main(String[] args) throws Exception {
		//使用Java代码来创建一个xml文件
		//1:创建一个文档对象模型
		Document doc = DocumentHelper.createDocument();
		//2:创建根元素
		/*
		 * 	<root>
		 * 	</root>
		 * 
		 * */
		Element root = doc.addElement("root");
		//3:创建一级子元素
		/*
		 * 	<root>
		 * 		<student></student>	
		 *  </root>
		 * 
		 * */
		Element student = root.addElement("student");
		//4:像创建的一级子元素中添加属性
		/*
		 * 	<root>
		 * 		<student id="et001" name="Penny">
		 * 		</student>	
		 *  </root>
		 * 
		 * */
		student.addAttribute("id","et001");
		student.addAttribute("name","Penny");
		//5:添加二级子元素
		/*
		 * 	<root>
		 * 		<student id="et001" name="Penny">
		 * 			<email>et01@etoak.com</email>
		 * 			<phone>11111</phone>
		 * 		</student>	
		 *  </root>
		 * 
		 * */
		Element email = student.addElement("email");
		email.setText("et01@etoak.com");
		Element phone = student.addElement("phone");
		phone.setText("11111");
		//设置输出流
		OutputStream os = new FileOutputStream("etoak2.xml");
		//设置输出格式
		//使用自动换行的xml输出格式
		OutputFormat format = OutputFormat
				.createPrettyPrint();
		//设置编码
		format.setEncoding("utf-8");
		//输出文件
		XMLWriter xw = new XMLWriter(os,format);
		//将组装好的模型按照设置的格式编码输出到指定目录
		xw.write(doc);
		xw.flush();
		xw.close();
	}
}
```

