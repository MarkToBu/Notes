#    **jQuery**

## 0.0 介绍

```text
~jQuery~
~Write Less Do More~

由John Resig 发布了一种JavaScript类库，也是JavaScript
众多类库中应用最为广泛的一个，使用了简单的选择器机制
使用代码链 基本解决了不同浏览器的差异性问题。jQuery是
众多js类库中使用最广泛的一个。

使用jQuery只需要从官网下载类库文件，引入到本页面即可

1.XXX
	兼容性较高支持老旧浏览器，对新型浏览器支持较差
2.XXX
	针对新型的浏览器进行设置，无法在老的浏览器上面使用
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
```

## 0.1  JS元素和JQuery元素是同一个元素吗？

```面试题
/*
			#js元素（dom元素）和jQuery元素是同一种
			元素吗？如果不是为什么？两者如何转换？
			
			<label id="test">测试</label>
			
			Js:
				var dom_lb = 
		        document.getElementById("test");
				
				<label id="test">测试</label>
			
			jQuery:
				var $jq_lb = 
				$("#test");
			
				dom_lb != $jq_lb
			
				[<label id="test">测试</label>]
			
			jQuery元素是对js元素一个轻度的封装
			jQuery元素只能使用类库中自己的方法 属性
			函数
			js元素只能使用js自己的函数 方法 属性
			两者不能互相使用对方的任何技术
			jq_input.innerHTML = ***
			$jq_input.html(***)
			
			转换
			jQuery~~~>Js
			$jq_input[0] = dom_input
			$jq_input.get(0) = dom_input
			
			Js~~~~>jQuery
			$(dom_input) = $jq_input
```







##1. 选择器

jQuery元素时对js元素的轻度封装。js效率更高。

> 转换
>
> jQuery  --->Js
>
> $jp_input[0] = dom_input;
>
> $jp_input.get(0) = dom_input;

>    js  --->jQuery
>
>   $(dom_input) = jp_input

### 1.1  基本选择器  

​     $("tagName")

​    $(".className")

​    $("#idName")

   $ ("tagName.className")

   $("sel1,sel2,sel3,sel4,seln")    

​      $("*")                                       全选

- ### 基本

  - [#id](id.html)  
  - [element](element.html)  
  - [.class](class.html)  
  - [*](all.html)  
  - [selector1,selector2,selectorN](multiple.html)     将每一个选择器匹配到的元素合并后一起返回

- ### 层级

  - [ancestor descendant](descendant.html)       定的祖先元素下匹配所有的后代元素
  - [parent > child](child.html)                 在给定的父元素下匹配所有的子元素
  - [prev + next](next_1.html)                     所有紧接在 prev 元素后的 next 元素
  - [prev ~ siblings](siblings_1.html)                匹配 prev 元素之后的所有 siblings 元素

- ### 基本

  - [:first](first_1.html)  
  - [:last](last_1.html)  
  - [:not(selector)](not_1.html)  
  - [:even](even.html)  
  - [:odd](odd.html)  
  - [:eq(index)](eq_1.html)  
  - [:gt(index)](gt.html)  
  - [:lt(index)](lt.html)  
  - [:header](header.html)  
  - [:animated](animated.html)  
  - [:focus](focus-1.html)1.6+ 

- ### 内容

  - [:contains(text)](contains.html)  
  - [:empty](empty_1.html)  
  - [:has(selector)](has_1.html)  
  - [:parent](parent_1.html) 

### 1.2 内容选择器

```javascript
//用来设置元素中的文本，类似  innerText();不支持超文本
	$("li:contains('我是列表9')").text("我是修改之后的文本"); 
     //html()在js中innerHTML,支持超文本
	$("li:empty").html("<font color='red'>我是添加的元素</font>");
  
```
- :contains

- :empty
- :has
- :parent

可见性：

​    :hidden

​    :visible 

- ### 属性

  - [[attribute\]](attributeHas.html)  
  - [[attribute=value\]](attributeEquals.html)  
  - [[attribute!=value\]](attributeNotEqual.html)  
  - [[attribute^=value\]](attributeStartsWith.html)  
  - [[attribute$=value\]](attributeEndsWith.html)  
  - [[attribute*=value\]](attributeContains.html)  
    - [[attrSel1\][attrSel2][attrSelN]

  

  

  ####  选择器总结：

  > 基本选择器
  >
  > $("#myELement")    选择id值等于myElement的元素，id值不能重复在文档中只能有一个id值是myElement所以得到的是唯一的元素  
  >
  > $("div")           选择所有的div标签元素，返回div元素数组  
  >
  > $(".myClass")      选择使用myClass类的css的所有元素  
  >
  > $("*")             选择文档中的所有的元素，可以运用多种的选择方式进行联合选择：例如$("#myELement,div,.myclass")  
  >
  >   
  >
  > 层叠选择器：  
  >
  > $("form input")         选择所有的form元素中的input元素  
  >
  > $("#main > *")          选择id值为main的所有的子元素  
  >
  > $("label + input")     选择所有的label元素的下一个input元素节点，经测试选择器返回的是label标签后面直接跟一个input标签的所有input标签元素  
  >
  > $("#prev ~ div")       同胞选择器，该选择器返回的为id为prev的标签元素的所有的属于同一个父元素的div标签  
  >
  >   
  >
  > 基本过滤选择器：  
  >
  > $("tr:first")               选择所有tr元素的第一个  
  >
  > $("tr:last")                选择所有tr元素的最后一个  
  >
  > $("input:not(:checked) + span")    
  >
  >   
  >
  > 过滤掉：checked的选择器的所有的input元素  
  >
  >   
  >
  > $("tr:even")               选择所有的tr元素的第0，2，4... ...个元素（注意：因为所选择的多个元素时为数组，所以序号是从0开始）  
  >
  >   
  >
  > $("tr:odd")                选择所有的tr元素的第1，3，5... ...个元素  
  >
  > $("td:eq(2)")             选择所有的td元素中序号为2的那个td元素  
  >
  > $("td:gt(4)")             选择td元素中序号大于4的所有td元素  
  >
  > $("td:ll(4)")              选择td元素中序号小于4的所有的td元素  
  >
  > $(":header")  
  >
  > $("div:animated")  
  >
  > 内容过滤选择器：  
  >
  >   
  >
  > $("div:contains('John')") 选择所有div中含有John文本的元素  
  >
  > $("td:empty")           选择所有的为空（也不包括文本节点）的td元素的数组  
  >
  > $("div:has(p)")        选择所有含有p标签的div元素  
  >
  > $("td:parent")          选择所有的以td为父节点的元素数组  
  >
  > 可视化过滤选择器：  
  >
  >   
  >
  > $("div:hidden")        选择所有的被hidden的div元素  
  >
  > $("div:visible")        选择所有的可视化的div元素  
  >
  > 属性过滤选择器：  
  >
  >   
  >
  > $("div[id]")              选择所有含有id属性的div元素  
  >
  > $("input[name='newsletter']")    选择所有的name属性等于'newsletter'的input元素  
  >
  >   
  >
  > $("input[name!='newsletter']") 选择所有的name属性不等于'newsletter'的input元素  
  >
  >   
  >
  > $("input[name^='news']")         选择所有的name属性以'news'开头的input元素  
  >
  > $("input[name$='news']")         选择所有的name属性以'news'结尾的input元素  
  >
  > $("input[name*='man']")          选择所有的name属性包含'news'的input元素  
  >
  >   
  >
  > $("input[id][name$='man']")    可以使用多个属性进行联合选择，该选择器是得到所有的含有id属性并且那么属性以man结尾的元素  
  >
  >   
  >
  > 子元素过滤选择器：  
  >
  >   
  >
  > $("ul li:nth-child(2)"),$("ul li:nth-child(odd)"),$("ul li:nth-child(3n + 1)")  
  >
  >   
  >
  > $("div span:first-child")          返回所有的div元素的第一个子节点的数组  
  >
  > $("div span:last-child")           返回所有的div元素的最后一个节点的数组  
  >
  > $("div button:only-child")       返回所有的div中只有唯一一个子节点的所有子节点的数组  
  >
  >   
  >
  > 表单元素选择器：  
  >
  >   
  >
  > $(":input")                  选择所有的表单输入元素，包括input, textarea, select 和 button  
  >
  >   
  >
  > $(":text")                     选择所有的text input元素  
  >
  > $(":password")           选择所有的password input元素  
  >
  > $(":radio")                   选择所有的radio input元素  
  >
  > $(":checkbox")            选择所有的checkbox input元素  
  >
  > $(":submit")               选择所有的submit input元素  
  >
  > $(":image")                 选择所有的image input元素  
  >
  > $(":reset")                   选择所有的reset input元素  
  >
  > $(":button")                选择所有的button input元素  
  >
  > $(":file")                     选择所有的file input元素  
  >
  > $(":hidden")               选择所有类型为hidden的input元素或表单的隐藏域  
  >
  >   
  >
  > 表单元素过滤选择器：  
  >
  >   
  >
  > $(":enabled")             选择所有的可操作的表单元素  
  >
  > $(":disabled")            选择所有的不可操作的表单元素  
  >
  > $(":checked")            选择所有的被checked的表单元素  
  >
  > $("select option:selected") 选择所有的select 的子元素中被selected的元素

  示例1:

  ```javascript
  <script type="text/javascript">
  		/*
  			#js元素（dom元素）和jQuery元素是同一种
  			元素吗？如果不是为什么？两者如何转换？
  			
  			<label id="test">测试</label>
  			
  			Js:
  				var dom_lb = 
  		        document.getElementById("test");
  				
  				<label id="test">测试</label>
  			
  			jQuery:
  				var $jq_lb = 
  				$("#test");
  			
  				dom_lb != $jq_lb
  			
  				[<label id="test">测试</label>]
  			
  			jQuery元素是对js元素一个轻度的封装
  			jQuery元素只能使用类库中自己的方法 属性
  			函数
  			js元素只能使用js自己的函数 方法 属性
  			两者不能互相使用对方的任何技术
  			jq_input.innerHTML = ***
  			$jq_input.html(***)
  			
  			转换
  			jQuery~~~>Js
  			$jq_input[0] = dom_input
  			$jq_input.get(0) = dom_input
  			
  			Js~~~~>jQuery
  			$(dom_input) = $jq_input
  			~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  			Selector 选择器
  			$("sel")
  			在文档中将sel表示所有选择器的统称
  			
  			1):$("tagName")
  			直接使用标签名拿取元素
  			2):$(".className")
  			直接使用类名拿取元素
  			3):$("#idName")
  			直接使用id名拿取元素
  			4):$("tagName.className")
  			$("tagName#idName")
  			拿取特定标签内部含有id或者class的元素
  			两个条件缺一不可
  			5):$("sel1,sel2,sel3,selN")
  			只要符合其中任意一个就可以成功拿取
  			6):$("sel1 sel2 selN")
  			根据从左到右排列具有家族关系的元素
  			拿取子元素 左祖先右后代
  			7):$("*")
  			表示全选元素
  			~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  			8):$("sel1 > sel2")
  			拿取指定元素的子元素
  			仅仅拿取子元素
  			css("css属性名","css属性值")
  		*/
  		$("div#container > label")
  		.css("color","orange");
  			
  		/*
  			9):$("sel1 + sel2")
  				A):向下选取
  				B):必须紧邻
  				C):互为兄弟
  				以上三个条件缺一不可
  		*/	
  		$("p#para + label")
  		.css("background-color","lightblue");
  		/*
  			10):$("sel1 ~ sel2")
  				A):向下选取		
  				B):互为兄弟
  		*/
  		$("p#para ~ label")
  		.css("font-style","italic");
  			
  	</script>
  ```

  - 示例二

  ```html
  <%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
  <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
  <html>
    <head>
      <title>基本选择器2</title>
    </head>
    <body>
  	<div id="container">
  		<ul>
  			<li>列表1</li>
  			<li>列表2</li>
  			<li>列表3</li>
  			<li>列表4</li>
  			<li>列表5</li>
  			<li>列表6</li>
  			<li>列表7</li>
  		</ul>	
  		<h2>我是二级标题</h2>
  		<h3>我是三级标题</h3>
  		<h5>我是五级标题</h5>
  		
  		性别:<input type="radio" 
  		name="sex" value="0" />女
  		<input type="radio"
  		name="sex" value="1" 
  		checked />男
  		<input type="radio"
  		name="sex" value="2" />其它
  		
  		
  	</div>
  	<script type="text/javascript"
  	src="script/jquery-1.10.2.js"></script>
  	<script type="text/javascript">
  		/*
  			1):$(":first")
  			拿取第一个元素，一般冒号前面还要书写具体
  			哪种元素
  			2):$(":last")
  			拿去最后一个元素
  			3):$(":eq(index)")
  			拿取索引值是index的元素，index从0开始
  			4):$(":gt(index)")
  			拿取大于索引值
  			5):$(":lt(index)")
  			拿取小于索引值
  		*/	
  		$("li:first").css("color","red");
  		$("li:last").css("color","navy");
  		$("li:eq(3)").css("color","pink");
  		$("li:gt(3)").css("color","gold");
  		$("li:lt(3)").css("color","black");
  		/*
  			6):拿取索引值是奇数的元素
  			$(":odd")
  			7):拿去索引值是偶数的元素
  			$(":even")
  		*/
  		$("li:odd").css("background-color","lightblue");
  		$("li:even").css("background-color","pink");
  		/*
  			8):$(":header")
  			拿取标题元素
  		*/
  		$(":header").css("border","solid 2px black");
  		/*
  			9):$("not:(sel)")
  			拿取无法被sel选择的元素
  		*/
  		alert($("input:not(':checked')").length);
  		
  	</script>
    </body>
  </html>
  ```

  

## 2 常用方法



 #### 方法示例：

```html
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
    <title>方法测试</title>
    <style type="text/css">
    	.red{
    		background-color:red;
    	}
    </style>
  </head>
  <body>
  	<div id="outter"><label>测试1</label><label>测试2</label></div>
	
	<input type="button" value="我是一个按钮" />
	
	<button>添加一行</button>
	
	<table style="width:600px" border="1px"
	id="mytable">
		<tr>
			<td>我是默认存在的一行</td>
		</tr>
	</table>
	
	<label style="background-color:blue">我是label</label>
	<hr>
	<span style="background-color:red">我是span</span>
	
	<div>我是一个默认存在的div！！</div>
	
	<ul id="myul">
		<li>列表1</li>
		<li>列表2</li>
		<li>列表3</li>
		<li>列表4</li>
		<li>列表5</li>
	</ul>
	
	<div id="showclone"></div>
	
	
	<script type="text/javascript"
	src="script/jquery-1.10.2.js"></script>
	<script type="text/javascript">
		/*
			常用方法测试：
			1):css("属性名","属性值")
			设置元素的样式
			2):
			text():不传递参数表示拿取文本，
			如果多个文本则合并为一个文本
			html():不传递参数表示拿取超文本
			-----------------------------
			text(要放置在元素中的文本)
			html(要放置元素中的超文本)
			如果元素中原先存在内容，则被覆盖
			它们的前身为
			innerHTML
			innerText
		*/		
		alert($("div[id='outter']").html());
		/*
			3):设置或者拿取元素属性
				attr("属性名","属性值")
				修改元素属性
				attr("属性名")
				拿取元素属性
		*/	
		$("input[type='button']")
		.attr("disabled",true);
		alert($("input[type='button']")
		.attr("type"));
		/*
			4):
				A.append(B)
				AB	A为父元素B为子元素，
				A内部原先的子元素不受影响，
				B追加在原先的子元素之后
				
				A.appendTo(B)
				BA	B为父元素A为子元素
				其它与append完全相反
		*/
		$("button:contains('添加一行')")
		.click(function(){
			$("table#mytable")
			.append($("<tr><td>我是之后又添加的一行</td></tr>"));
		});
		/*
			5):
				A.before(B)
				BA
				A的前面是B，两者互为兄弟
				A.after(B)
				AB
				A的后面是B，两者互为兄弟
				
				雨天太滑了 我骑着自行车差点滑倒
				一把把把把住了
				请问 把是什么意思		
		*/
		$("label:contains('我是label')")
		.before($("span:contains('我是span')"));
		/*
			6):
				A.replaceWith(B)
				A被B替代，A不存在了
			
				A.replaceAll(B)
				A替代了B，B不存在了
		*/		
		$("div:contains('默认存在')")
		.replaceWith($("<p style='color:red'>我是替代的段落</p>"));
		
		/*
			7):
				sel.each(function(index){
					遍历
					sel:一般拿取循环体
					index:表示索引值从0开始
				});
		*/
		$("ul#myul li").each(function(index){
			if(index==3){
				/*
					this:表示已经通过选择器
					拿取的元素
					addClass()添加类
					removeClass()删除类
					toggleClass()如果存在某个类
					删除，反之添加
				*/
				$(this).addClass("red");
			}
		});
		
		/*
			8):clone()
			复制一个元素，注意被复制的元素具有
			相同的结构 样式 动作
		*/
		
		$("div#showclone")
		.html($("table#mytable").clone());
		alert($("div#showclone").html());
 	
	</script>
  </body>
</html>
```





  ## 3  事件

​    $(document).ready(function(){  // 在这里写你的代码...});

```
$('#foo').bind('click', function() {
  alert('User clicked on "foo."');
});
```

#### 示例一： 

```html
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
    <title>jquery3:事件</title>
    <style type="text/css">
    	div#mydiv{
    		border:solid 3px black;
    		width:200px;
    		height:100px;
    	}
    	.red{
    		background-color:red;
    	}
    	.blue{
    		background-color:blue;
    	}
    </style>
  </head>
  <body>
  	<button>我是一个按钮</button>
  	
  	<input type="button" value="我又是一个按钮" />
  	
  	用户姓名:<input type="text" name="name" />
  	<label id="name_msg"></label>
  	
  	<div id="mydiv">
  		测试
  	</div>
  	
  	<button>我还是一个按钮</button>
  	
  	<label id="show"></label>
  	
  	
  	
  	
	<script type="text/javascript"
	src="script/jquery-1.10.2.js"></script>
	<script type="text/javascript">
		/*
			#jQuery四大核心函数
			
			1):$(sel context)
			在括号内书写选择器，如果存在第二个参数
			则表示从此参数设置的范围中根据选择器选取
			，如果没有第二个参数，则从全文中选取
			2):$(html)
			直接在括号内书写html标签，配合部分方法
			执行
			3):$(dom)
			内部传输dom元素，用来转换成jQuery
			$(document.getElementById("idName"));
			4):$(document).ready(function(){
				此函数表示全文结构加载完毕
				一般如果涉及到绑定事件，则所有的函数
				都放放置在此函数中，类似Java的主方法
			});
		
			简略写法：
			$(function(){
				
			});
			-----------------------------------------
			#ready()函数与onload激发事件有什么不同?
			1)onload全文只执行一次，
			如果存在多个执行最后一个
			ready()从上到下依次执行
			2)ready()表示全文结构加载完毕即可
			onload一切要素都必须加载完毕
			~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~	
		*/		
		$(document).ready(function(){
			/*
				1):为元素绑定多个事件
				$(sel).bind("事件1 事件2 事件N",function(){
					只要满足其中任意一个事件就可以激发后面的
					匿名函数
				});
			*/
			$("button:contains('我是一个按钮')")
			.bind("click mouseout",function(){
				$(this).attr("disabled",true);
			});
			/*
				2):为元素绑定固定的事件
				$(sel).事件(function(){
					
				});
			*/
			$("input[type$='tton']").click(function(){
				alert("谁点我了啊~~~~~");
			});
			
			$(document).dblclick(function(){
				alert("谁双击我了呀~~~~~~");
			});
			
			$(":text").blur(function(){
				/*
					val():拿取元素的value属性
				*/
				if($(this).val().length<4){
					$("label#name_msg")
					.html("<font color='red'>用户名不能小于4位</font>");
				}
			});
			
			$("div#mydiv").hover(
				//鼠标滑过执行此函数
				function(){
					$(this).removeClass();
					$(this).addClass("red");
				},
				//鼠标滑出执行此函数
				function(){
					$(this).removeClass();
					$(this).addClass("blue");
				}
			);
			/*
				$(sel).one("事件1 事件2 事件N",function(){
					只要满足其中任意一个事件就可以激发后面的
					匿名函数,仅仅激发一次
				});
			*/
			var count = 0;
			
			$("button:contains('我还是一个')")
			.one("click",function(){
				count++;
				$("label#show")
				.html("激发了"+count+"次");
			});
		});
	</script>
  </body>
</html>
```





## 4 .  Jquery中的四个核心函数

````javascript
jQuery的四个核心函数。 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
jQuery(expression,[context])

这个函数接收一个包含 CSS 选择器的字符串，然后用这个字符串去匹配一组元素。jQuery的核心功能都是通过这个函数实现的。 jQuery中的一切都基于这个函数，或者说都是在以某种方式使用这个函数。这个函数最基本的用法就是向它传递一个表达式（通常由 CSS 选择器组成），然后根据这个表达式来查找所有匹配的元素。

默认情况下, 如果没有指定context参数，$()将在当前的 HTML 文档中查找 DOM 元素；如果指定了 context 参数，如一个 DOM 元素集或 jQuery 对象，那就会在这个 context 中查找。

参数：

expression (String) : 用来查找的字符串

context (Element, jQuery) : (可选) 作为待查找的 DOM 元素集、文档或jQuery对象。

示例

找到所有p元素，并且这些元素都必须是 div 元素的子元素。

HTML代码:

<p>one</p> <div><p>two</p></div> <p>three</p>

jQuery代码:

$("div > p");

结果:

[ <p>two</p> ]

在文档的第一个表单中，查找所有的单选按钮(即: type 值为 radio 的 input 元素)。

jQuery代码:

$("input:radio", document.forms[0]);

在一个由AJAX返回的 XML 文档中，查找所有的div元素。

jQuery代码:

$("div", xml.responseXML);

jQuery(html,[ownerDocument])

根据提供的原始 HTML 标记字符串，动态创建由 jQuery 对象包装的DOM元素。你可以传递一个手写的 HTML 字符串，或者由某些模板引擎或插件创建的字符串，也可以是通过 AJAX 加载过来的字符串。但是在你创建 input 元素的时会有限制，可以参考第二个示例。

当然这个字符串可以包含斜杠 (比如一个图像地址)，还有反斜杠。当你创建单个元素时，请使用闭合标签或 XHTML 格式。例如，创建一个 span ，可以用 $("<span/>") 或 $("<span></span>") ，但不推荐 $("<span>")。在jQuery 中，这个语法等同于$(document.createElement("span")) 。

参数：

html (String) : 用于动态创建DOM元素的HTML标记字符串

ownerDocument (Document) : 可选，创建DOM元素所在的文档

示例：

动态创建一个 div 元素（以及其中的所有内容），并将它追加到 body 元素中。在这个函数的内部，是通过临时创建一个元素，并将这个元素的 innerHTML 属性设置为给定的标记字符串，来实现标记到 DOM 元素转换的。所以，这个函数既有灵活性，也有局限性。
jQuery代码:
$("<div><p>Hello</p></div>").appendTo("body")
创建一个 <input> 元素必须同时设定 type 属性。因为微软规定 <input> 元素的 type 只能写一次。
jQuery代码
// 在 IE 中无效:
 $("<input>").attr("type", "checkbox");
 // 在 IE 中有效:
 $("<input type='checkbox'>");
jQuery(elements)
将一个或多个DOM元素转化为jQuery对象。这个函数也可以接收XML文档和Window对象（虽然它们不是DOM元素）作为有效的参数。
参数
elements (Element, Array<Element>) : 用于封装成jQuery对象的DOM元素

示例：
设置页面背景色。
jQuery代码:
$(document.body).css( "background", "black" );
隐藏一个表单中所有元素。
jQuery代码:
$(myForm.elements).hide()
jQuery(callback)

$(document).ready()的简写。允许你绑定一个在DOM文档载入完成后执行的函数。这个函数的作用如同$(document).ready()一样，只不过用这个函数时，需要把页面中所有需要在 DOM 加载完成时执行的$()操作符都包装到其中来。从技术上来说，这个函数是可链接的－－但真正以这种方式链接的情况并不多。

你可以在一个页面中使用任意多个$(document).ready事件。

参数：
callback (Function) : 当DOM加载完成后要执行的函数
示例：
当DOM加载完成后，执行其中的函数。
jQuery 代码:
$(function(){
 // 文档就绪
 });
使用$(document).ready() 的简写，同时内部的jQuery代码依然使用 $ 作为别名，而不管全局的 $ 为何。
jQuery 代码:
jQuery(function($) {
 // 你可以在这里继续使用$作为别名...
 });

````

## 5.   Jquery破坏性操作

```
破坏性操作:
						当用户执行某个方法时，
						执行之后拿取的元素与之前通过
						选择器拿取的元素发生了变动
						则我们称这个方法为破坏性操作
						
						children("元素名"):
							破坏性操作，拿取元素的特定子元素
						siblings():破坏性操作，拿取元素除自己
						以外的兄弟元素
					----------------------------
					show():显示
					end():返回执行破坏性操作之前的元素列表
					如果没有破坏性操作返回null
					slideDown():卷帘显示
					slideUp():卷帘隐藏
					
					sel.show(3000,function(){
						脚本
					});
```

##### 示例一：

```html
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
    <title>jQuery5:破坏性操作(制作一个tree)</title>
    <style type="text/css">
    	#menu{
    		width:300px;
    	}
		.has_children{
			background-color:lightblue;
			color:navy;
			cursor:hand;
		}
		.highlight{
			background-color:orange;
			color:red;
		}
		div a{
			float:left;
			width:300px;
			background-color:silver;
			display:none;
		}
    </style>
  </head>
  <body>
	<div id="menu">
		<div class="has_children">
			<span>一:第一章</span>
			<a>第1.1节</a>
			<a>第1.2节</a>
			<a>第1.3节</a>
			<a>第1.4节</a>
		</div>
		<div class="has_children">
			<span>二:第二章</span>
			<a>第2.1节</a>
			<a>第2.2节</a>
			<a>第2.3节</a>
			<a>第2.4节</a>
		</div>
	</div>
	<script type="text/javascript"
	src="script/jquery-1.10.2.js"></script>
	<script type="text/javascript">
		$(function(){
			$("div.has_children").click(function(){
				/*
					破坏性操作:
						当用户执行某个方法时，
						执行之后拿取的元素与之前通过
						选择器拿取的元素发生了变动
						则我们称这个方法为破坏性操作
						
						children("元素名"):
							破坏性操作，拿取元素的特定子元素
						siblings():破坏性操作，拿取元素除自己
						以外的兄弟元素
					----------------------------
					show():显示
					end():返回执行破坏性操作之前的元素列表
					如果没有破坏性操作返回null
					slideDown():卷帘显示
					slideUp():卷帘隐藏
					
					sel.show(3000,function(){
						脚本
					}); 
				*/
				$(this).addClass("highlight")
				.children("a").slideDown().end().siblings()
				.removeClass("highlight").children("a")
				.slideUp();
			});
		});	
	</script>
  </body>
</html>
```
