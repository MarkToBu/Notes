# javascript

## 1.0 简介javascript

~Javascript~
~脚本语言~

最早由网景公司发布，原名livescript，之后与微软竞争桌面市场中的获胜，与SUN公司合作最终定名为JavaScript，但是Js与Java的关系就如同雷锋和雷峰塔一样，没有任何联系。目前这两个技术都隶属于甲骨文公司。
Javascript是一门运行在本地客户端浏览器的解释型的弱语言。
代码没有容错性，严格区分大小写，独立文件以.js结尾。

语法:
	基本数据类型
	Java:
		int byte char short float double long boolean 

 Js:
		基本数据类型
		String number boolean null undefined
	
复合数据类型
		Array Object
	
 注意:
	
   Java:
       int i;
		Js:
			var i = "";

 		流程控制 循环等于java
 		类似
 		一般来说通过一个事件激发绑定在事件上的函数

## 1.1 js引入和基本使用

```html
<!DOCTYPE html>
<html>
	<head>
		<title>1:如何引入Javascript</title>
		<meta charset="utf-8" />
		<!--
			Javascript

			jQuery ajax prototype YUI dojo
			ExtJs VUE angular EASYUI 等等

			从head标签内引入js脚本
			type:MIME数据类型
			这里可以省略
		-->
		<script>
			/*
				window:BOM七对象之一，表示
				整个窗口，是BOM七对象当中的顶层对象
				（父对象）
				onload:激发事件之一，表示
				整个页面成功载入

			*/
			window.onload = etoak;

			/*
				function 函数名(实参){ 
					
				}
			*/
			function etoak(){
				//弹出一个对话框
				window.alert("实业误国，地产兴邦!");
			}
		</script>
	</head>
	<body>

	</body>
</html>
```

#### 其它几种引入js的方式

```html
<!DOCTYPE html>
<html>
	<head>
		<title>3:其它几种引入js的方式</title>
		<meta charset="utf-8" />
		
	</head>
	<!--
		直接将激发事件书写在标签内
	
	<body 
	onload="javascript:alert('五一节即将到来！')">
	
	<body onload="alert('五一节即将到来！')">
	-->
	<body onload="etoak()">


		<script>
			function etoak(){
				//alert('五一节即将到来！');
				var season = 4;
				if(season>=1&&season<=3){
					alert("春天来了");
				}else if(season>=4&&season<=6){
					alert("夏天来了");
				}else if(season>=7&&season<=9){
					alert("秋天来了");
				}else if(season>=10&&season<=12){
					alert("冬天来了");
				}

				/*
					直接从页面输出
					document.write(要在页面上输出的内容)
					支持html标签
				*/
				var i = 10;
				var j = "etoak";
				var x = 1.1;
				var y = true;
				var z = null;

				document.write(i+"+"+j+"="+(i+j)+"<br />");
				document.write(i+"+"+x+"="+(i+x)+"<br />");
				document.write(i+"+"+y+"="+(i+y)+"<br />");
				document.write(i+"+"+z+"="+(i+z)+"<br />");
				document.write(x+"+"+y+"="+(x+y)+"<br />");
				document.write(x+"+"+z+"="+(x+z)+"<br />");
				document.write(z+"+"+y+"="+(z+y)+"<br />");
				document.write(i+"+"+z+"="+(i+z)+"<br />");

				/*
					使用document.write()
					从页面上输出九九乘法表
				*/
				var str = "";

				for(var a = 1;a<=9;a++){
					for(var b = 1;b<=a;b++){
						str += b+"*"+a+"="+(b*a)+"\t";
					}
					str += "<br />";
				}
				document.write(str);
			}
		</script>
	</body>
</html>
```





## 1.2  DOM文档对象模型

```html
<!DOCTYPE html>
<html>
	<head>
		<title>2:DOM文档对象模型</title>
		<meta charset="utf-8" />
		<script>
			/*
				1:BOM(Browse Object Model浏览器对象模型)
				window:表示整个窗口，全局变量，
				顶层对象
				document:表示正文（DOM文档对象模型）
				location:表示地址栏信息
				history:浏览器的缓存以及历史记录
				frame:表示frame框架（已淘汰）
				screen:表示打开这个窗口所使用的显示器信息
				navigator:表示浏览器信息

				window.alert()
				window.document.getElementById("");
				window.location.href = ****;
				~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
				
				2:js能够直接修改html结构吗?如果不可以
				为什么?

				html
				  |
				  |
				 DOM(文档对象模型)
				  |
				  |
				javascript

				浏览器从上到下解析页面，如果没有任何错误，解析成功，则创建一个文档对象模型，这个模型与当前解析完时页面的结构和样式完全你一致

				模型如下：
				document
					|
					|
		    --------|---------
		    |                |
		    |                |
           head             body
            |                |
         -------          --------
         |     |         |        |
        title meta     button     table
                                    |
                                    |
                                   tr
                                    |
                                   td

		模型创建完毕，全部封装进document对象
		js通过修改dom对象将模型进行结构和样式的
		渲染，渲染之后模型与页面不一致，浏览器重新
		刷新页面，使两者保持一致

		当我们将window.onload这个激发事件去掉之后
		js失效，因为模型还未创建完毕就拿取元素是无效的，但是我们可以将所有的js脚本放置在body标签关闭
		之前，这样不需要添加onload事件了


			*/
			window.onload = etoak;

			function etoak(){
				/*
					document:BOM七对象之一
					是window的第一个子对象
					表示页面的上下文
					getElementById(idName)
					根据id拿取唯一一个元素
				*/
				var dom_btn = 
				document.getElementById("mybtn");
			
				//当按钮被点击时
				/*
					1:绑定函数名的写法
					元素.事件 = 函数名

					function 函数名(实参){
	
					}
					~~~~~~~~~~~~~~~~~~~~~~~~~
					2:绑定匿名函数的写法
					元素.事件 = function(实参){
	
					}
				*/
				dom_btn.onclick = function(){
					/*alert("谁点我了呀？？！！");*/
					//根据id拿取table
					var dom_tb = document.getElementById("mytb");

					/*
						创建一个tr元素
						<tr></tr>
					*/
					var dom_tr = document.createElement("tr");
					/*
						修改元素内部的结构
						修改前:
							<tr></tr>
						修改后:
							<tr><td>我是添加的一行</td></tr>
						
						innerHTML:属性，向元素中添加
						超文本，如果元素中存在子元素，则其全部被覆盖	
				
					*/
					dom_tr.innerHTML = "<td>我是添加的一行</td>";
					/*
						将组装好的tr追加到table中子元素之后，不影响原来的子元素
					*/
					dom_tb.appendChild(dom_tr);
				}
			}
		</script>
	</head>
	<body>
		<button id="mybtn">添加一行</button>
		<table style="width:500px" border="1px"
		id="mytb">
			<tr><td>我是第一行</td></tr>
		</table>

		<script type="text/javascript">
			/*
				一般将js脚本放置在页面最下方
				这样不需要加载onload属性，同时
				优先加载结构和样式
			*/
		</script>
	</body>
</html>
```

## 1.3 js简单事件

```html
<!DOCTYPE html>
<html>
	<head>
		<title>4:各种测试</title>
		<meta charset="utf-8" />
		
	</head>
	<body>
		<!--
			onblur:失去焦点
			括号内的参数称之为实参
		-->
		测试1:<input type="text" 
		name="name" onblur="func3(100,'王超',true)" />
		<!--
			onfocus:获得焦点
		-->
		测试2:<input type="text" 
		name="test2" onfocus="showArray()" />
		
		<!--
			onmouseover:鼠标滑过
		-->
		<div style="width:200px;height:100px;background-color:pink"
		onmouseover="show()"></div>
		<!--
			src:引入外部的独立js文件，注意此标签
			要么自己书写，要么引入外部文件，
			两者二选一
		-->
		<script src="script/myjs1.js"></script>
		<script src="script/myjs2.js"></script>
		<script>
			function showArray(){
				//创建一个一维数组
				var ar = new Array();
				ar[0] = "etoak";
				ar[1] = 100;
				ar[2] = true;
				ar[3] = null;

				/*
					Java:
						String			length()
						Array           length
						List			size()
					Js:
										length
				*/
				for(var i = 0;i<ar.length;i++){
					alert("数组中数据是"+ar[i]);
				}				
			}

			function show(){
	
				var str = "http://www.etoak.com";
				alert("字符串长度是~~~~~》"+str.length);
				alert("将e替换为a~~~~~》"+str.replace("e","a"));
				alert("从左到右拿取第一个w的索引值~~~~~》"+str.indexOf("w"));
				alert("从左到右拿取最后一个w的索引值~~~~~》"+str.lastIndexOf("w"));
				alert("拿取特定索引值处的字符~~~~~》"+str.charAt(3));
				/*
					substring(x,y)
					x:表示起始截取的索引值
					y:截取到的索引值，但是不包含对应的字符
					substring(3,7)
					截取索引值3到索引值7的字符，但是不包含索引值对应的字符

					substring(x)
					
					substring(5)
				*/
				alert("截取字符串~~~~~》"+str.substring(3,7));

				/*
					分割字符串
					表示在.这个位置开始截取
				*/
				var knife = str.split(".");

				for(var i = 0;i<knife.length;i++){
					alert("分割的字符串是~~~》"
						+knife[i]);
				}
			}
			/*
				<div id="container">
					<h2>标题</h2>
					<img style="float:left" />
					<p>*****</p>
					<p>*****</p>
				</div>

				text-indent:2em;
				line-height:***px;
				
			*/
		</script>
	</body>
</html>
```

## 1.4  对象字面量与构造方法

```html
<!DOCTYPE html>
<html>
	<head>
		<title>5:对象字面量与构造方法</title>
		<meta charset="utf-8" />
	</head>
	<body>

		<script>
		/*
			1:使用对象字面量来创建一个对象
			var 对象名 = {
				属性名1:属性值1,
				属性名2:属性值2,
				属性名N:属性值N,
				方法名:function(){
					
				}
			}

			拿取属性值:
				对象名.属性名 = 属性值;
			调用方法
				对象名.方法名();
			删除属性
				delete 对象名.属性名
			
			var document = {
				title:***,
				url:***,
				getElementById:function(id){
					****
				},
				getElementsByTagName:funciton(name){
					****
				}

			}
 			{key:value,key2:value2}
        */	
		var stu = {
			name:"王东",
			age:18,
			hobby:["篮球","王者荣耀","音乐"],
			etoak:function(){
				alert(this.name+"非常喜欢"+this.hobby[1]+"与"+this.hobby[2]);
			}
		}
 		alert(stu.name);
		stu.etoak();
 		/*public User(){}

		User u = new User();

		public User(String name){}

		User u = new User("penny");*/

		/*
			2:使用构造方法来创建对象
			构造方法较为灵活，类似一个模板
			function 对象名(属性名1,属性2){
				this.属性名 = 属性名1;
				this.方法名 = function(){
	
				}
			}
		*/
		function student(name,age){
			this.name = name;
			this.age = age;
			this.etoak = function(){
				if(this.age<18){
					alert("18岁以下禁止访问本网站");
				}else{
					alert("欢迎您回来"+name);
				}
			};
		}

		var person = new student("王东",27);
		person.etoak();
		</script>
	</body>
</html>
```

## 1.5  处理页面元素

- main.html

```html
<!DOCTYPE html>
<html>
	<head>
		<title>6:页面元素控制</title>
		<meta charset="utf-8" />
		<link rel="stylesheet" type="text/css" 
		href="css/style.css" />
	</head>
	<body>
		<div id="container">
			<h3>~等待四年~</h3>
			<hr>
			<label id="mypic"></label>
			<button onclick="touch()">显示封面</button>
			<br>
			<a href="#" 
			onclick="buy()"
			id="mya">购买此专辑</a>
		</div>
		<script>
			function touch(){
				//根据id拿取label
				var dom_lb = 
				document.getElementById("mypic");
				
				//根据标签名拿取多个元素
				/*var dom_btn =
				document.getElementsByTagName("button")[0];

				dom_btn.onclick = functino(){

				}*/


				//修改label元素的结构
				/*
					修改前：
					<label></label>
					修改后：
					<label><img src='image/Cover.jpg' /></label>

				*/
				dom_lb.innerHTML = "<img src='image/Cover.jpg' />";

			}


			function buy(){
				/*
					if(window.confirm("显示的文本")){
						肯定分支
					}else{
						否定分支
					}
				*/
				if(confirm("您是否确定要购买此专辑")){
					//跳转到suc.html
					location.href = "suc.html";
				}else{
					//拿取a链接
					var dom_a = 
					document.getElementById("mya");
					//修改元素的属性href
					dom_a.href = "err.html";
				}
			}
		</script>
	</body>
</html>
```

- suc.html

  ```html
  <!DOCTYPE html>
  <html>
  	<head>
  		<title>购买页面</title>
  		<meta charset="utf-8" />
  		<style type="text/css">
  			div#msg{
  				width:300px;
  				height:60px;
  				border:solid 2px red;
  				background-color:#FFE4E1;
  				display:none;
  			}
  		</style>
  		<link rel="stylesheet" type="text/css" 
  		href="#" id="mylink" />
  	</head>
  	<body>
  		这里是购买页面
  		<a href="#" onclick="history.go(-1)">返回上一页</a>
  		<form action="#" method="get">
  			用户卡号:<input type="text" 
  			name="idcard" />
  			<br>
  			支付密码:<input type="password" 
  			name="pass" />
  			<br>
  			<!--
  				通过style属性修改元素的样式
  			-->
  			记住密码:<input type="checkbox" 
  			name="rem" 
  			onmouseover="document.getElementById('msg').style.display = 'block'"
  			onmouseout="document.getElementById('msg').style.display = 'none'" 
  			/>
  			<div id="msg">如果您在公共场合开启此功能，请谨慎使用，可能会对您的计算机账户造成严重的安全隐患</div>
  		</form>
  
  
  		<!--
  			样式:粉色背景，深蓝字体
  		-->
  		<button onclick="change('one')">切换主题1</button>
  		<!--
  			样式:浅蓝背景，紫色字体
  		-->
  		<button onclick="change('two')">切换主题2</button>
  
  		<script type="text/javascript">
  			var dom_link = 
  			document.getElementById("mylink");
  
  			function change(value){
  				if(value=='one'){
  					dom_link.href = "css/style1.css";	
  				}
  
  				if(value=='two'){
  					dom_link.href = "css/style2.css";
  				}
  			}
  			/*
  				根据id或者标签名拿取元素之后
  
  				1:修改结构
  					innerHTML
  				2:修改属性
  					对象.属性名=属性值
  			*/
  		</script>
  	</body>
  </html>
  ```

- err.html

  ```html
  <!DOCTYPE html>
  <html>
  	<head>
  		<title>挑选其它专辑</title>
  		<meta charset="utf-8" />
  	</head>
  	<body>
  		挑选其它专辑
  	</body>
  </html>
  ```



  ##  1.6 表单验证

  ```html
  <!DOCTYPE html>
  <html>
  	<head>
  		<title>8:表单验证</title>
  		<meta charset="utf-8" />
  	</head>
  	<body>
  		<!--
  			onsubmit:激发事件，表示表单提交时
  			return 后面的函数如果返回true则表单可以提交，否则表单点击提交无效
  		-->
  		<form action="wow7.html" method="get"
  		onsubmit="return checkAll()">
  			<!--
  				1:单行文本输入框
  					只允许用户名书写四位到八位
  			-->
  			用户姓名:<input type="text" 
  			name="name" placeholder="请填写用户名" 
  			onblur="checkName()" id="name" 
  			style="width:138px" />
  			<label id="name_msg"></label>
  			<br>
  			<!--
  				2:单行文本密码框
  					只允许用户密码书写四到八位
  				this:书写在哪里，就指代本元素
  			-->
  			用户密码:<input type="password" 
  			name="pass" placeholder="请填写密码" 
  			onblur="checkPass(this.value)" id="pass" 
  			style="width:138px"/>
  			<span id="pass_msg"></span>
  			<br>
  			<input type="submit" value="提交" />
  			<input type="reset" value="取消" />
  		</form>
  
  		<script>
  
  			var flagName = false;
  			var flagPass = false;
  
  			function checkName(){
  				//根据id拿取input元素
  				var dom_input = 
  				document.getElementById("name");
  
  				//根据id拿取label
  				var dom_lb = 
  				document.getElementById("name_msg");
  
  				if(dom_input.value.length<4||dom_input.value.length>8){
  					dom_lb.innerHTML="<img src='image/wrong.png'><font color='red'>用户名不能小于4位或者大于8位</font>";
  					flagName = false;
  					return;
  				}
  				dom_lb.innerHTML="<img src='image/right.png'><font color='green'>用户名符合要求</font>";
  
  				flagName = true;
  			}
  
  			function checkPass(flag){
  				//拿取span
  				var dom_sp = 
  				document.getElementById("pass_msg");
  			
  				if(flag.length<4||flag.length>8){
  					dom_sp.innerHTML = "<img src='image/wrong.png'/><font color='red'>用户密码不能小于4位或者大于8位</font>";
  					flagPass = false;
  					return;
  				}
  				dom_sp.innerHTML = "<img src='image/right.png'/><font color='green'>用户密码符合要求</font>";	
  				flagPass = true;
  			}
  
  			function checkAll(){
  				return flagName&&flagPass;
  			}
  
  
  
  		</script>
  	</body>
  </html>
  ```

- 表单验证2

```html
<!DOCTYPE html>
<html>
	<head>
		<title>9:表单验证</title>
		<meta charset="utf-8" />
	</head>
	<body>
		<!--
			1:单选框
				只能选择女性
		-->
		性别:<input type="radio" 
		name="sex" value="0" 
		onclick="checkSex()" />女

		<input type="radio" 
		name="sex" value="1" 
		onclick="checkSex()" />男
		<label id="sex_msg"></label>
		<br>

		<!--
			2:多选框
				爱好不能少于3个
		-->
		爱好:<input type="checkbox" 
		name="hobby" value="soccer" 
		onclick="checkHobby()" />足球
		
		<input type="checkbox" 
		name="hobby" value="kendo" 
		onclick="checkHobby()" />剑道
		
		<input type="checkbox" 
		name="hobby" value="game" 
		onclick="checkHobby()" />游戏
		
		<input type="checkbox" 
		name="hobby" value="travel" 
		onclick="checkHobby()" />旅行
		
		<input type="checkbox" 
		name="hobby" value="camera" 
		onclick="checkHobby()" />摄影
		
		<label id="hobby_msg"></label>
		<br>

		<!--
			3:多行文本输入框
				最多输入140个字

				onkeyup:键盘键位抬起
				onkeydown:键盘键位落下
		-->
		您有什么新鲜事告诉大家:
		<textarea cols="30" rows="5"
		name="weibo" 
		onkeyup="check(this.value)"
		oncopy="return false"
		oncut="return false"
		onpaste="return false"></textarea>
		<div id="msg"></div>

		<script>
			function checkSex(){
				//拿取label
				var dom_lb = 
				document.getElementById("sex_msg");

				//根据name属性拿取所有的input元素
				var dom_input =
				document.getElementsByName("sex");

				for(var i=0;i<dom_input.length;i++){
					//判断第i个元素是否被选中
					if(dom_input[i].checked){
						if(dom_input[i].value=="0"){
							dom_lb.innerHTML="<font color='green'>欢迎女士注册</font>";
							return;
						}
						dom_lb.innerHTML="<font color='red'>男士谢绝登录</font>";
					}
				}
			}

			function checkHobby(){
				//拿取label
				var dom_lb = 
				document.getElementById("hobby_msg");
				//根据name拿取多个元素
				var dom_hobby=
				document.getElementsByName("hobby");

				var count = 0;

				for(var i = 0;i<dom_hobby.length;i++){
					if(dom_hobby[i].checked){
						count++;
					}

					if(count<3){
						dom_lb.innerHTML=
						"<font color='red'>爱好不能少于3个</font>";
					}else{
						dom_lb.innerHTML=
						"<font color='green'>爱好广泛</font>";
					}
				}
			}

			function check(value){
				var dom_div = 
				document.getElementById("msg");

				if(value.length<=140){
					dom_div.innerHTML=
					"<font color='green'>您还可以输入"+(140-value.length)+"个字符</font>";
					return;
				}
				dom_div.innerHTML=
					"<font color='red'>您已经超过"+(value.length-140)+"个字符</font>";
			}

			/*
				用户姓名:4~8
				用户密码:4~8
				重复密码:必须和密码保持一致
				性别:不能不填
				爱好:不能少于3个
				个人介绍:不得多于140个字符
				以上表单项全部书写正确，则表单可以提交
				否则表单提交无效
				以上表达书写在一个七行四列的表格中
			*/
 
		</script>
	</body>
</html>



```



  ##  1.7 :简易相册

  ```java
  <!DOCTYPE html>
  <html>
  	<head>
  		<title>7:简易相册</title>
  		<meta charset="utf-8" />
  		<link rel="stylesheet" type="text/css" 
  		href="css/mystyle.css" />
  	</head>
  	<body>
  		<div id="container">
  			<table>
  				<caption><img src="image/logo.png"></caption>
  				<tr>
  					<td><input type="image" 
  					src="image/left.gif"
  					onclick="change('left')"></td>
  					<td><img src="image/1.jpg"
  						id="mypic"></td>
  					<td><input type="image" 
  					src="image/right.gif"
  					onclick="change('right')"></td>
  				</tr>
  			</table>
  		</div>
  		<script>
  			var currentPage = 1;
  			function change(value){
  				if(value=='left'){
  					if(currentPage>1){
  						currentPage--;
  					}else{
  						alert("已经是首页了");
  					}
  				}	
  
  				if(value=='right'){
  					if(currentPage<6){
  						currentPage++;
  					}else{
  						alert("已经是末页了");
  					}
  				}
  
  				var dom_img = document.getElementById("mypic");
  
  				//修改元素的属性
  				dom_img.src=
  				"image/"+currentPage+".jpg";
  			}
  		</script>
  	</body>
  </html>
  ```
##  1.8 js覆盖

- myjs.1

```javascript
var str = "etoak";
var count = 10;

function func1(){
	count++;
	return count;
}

function func2(){
	var obj = func1();
	alert(obj);
	alert(str);
}





```

- myjs.2

  ```javascript
  function func3(){
  	alert("我是空参的func3~~~");
  }
  
  function func3(flag){
  	alert("我是有参的func3~~~");
  }
  
  /*
  	js中没有override 和 overload
  	如果出现相同的函数名，则执行最后一个，
  	与内部传参没有任何关系
  	括号内传递的为形参
  */
  function func3(etoak1,etoak2,etoak3){
  	func1();
  	func2();
  	alert(etoak1+"\n"+etoak2+"\n"+etoak3);
  }
  
  ```

  

##  1.9 js 数组

Js中创建数组的方法				

```javascript
				1：var ar1 = new Array();
					ar1[0]= ***
					ar1[1]= ***
					***
				2: var ar2 = new Array(数组长度)
				
				3：var ar3 = new Array([第0个元素],
				[第1个元素],[第2个元素***]);
					var a = new Array(["b"], [2], ["a"], [4]);  
				
				4：var ar4 =  ["b", 2, "a", 4]; 
					创建数组并赋值的简写
```
## 1.10 JS 常见功能实现

```javascript
1. 将彻底屏蔽鼠标右键
oncontextmenu=”window.event.returnValue=false”

< table border oncontextmenu=return(false)>< td>no< /table> 可用于 Table
2. 取消选取、防止复制
< body onselectstart=”return false”>
3.JS不允许粘贴
onpaste=”return false”
4. JS防止复制
oncopy=”return false;” oncut=”return false;”
5. IE 地址栏前换成自己的图标
< link rel=”Shortcut Icon” href=”favicon.ico”>
在文件的根目录放进去这个图片，后缀修改成ico就可以了
6.可以在收藏夹中显示出你的图标
< link rel=”Bookmark” href=”favicon.ico”>
7.关闭输入法
< input style=”ime-mode:disabled”>
8. 永远都会带着框架
< script language=”JavaScript”>< !–

if (window == top)top.location.href = “frames.htm”; //frames.htm 为框架网页

// –>< /script>
9. 防止被人 frame
< SCRIPT LANGUAGE=JAVASCRIPT>< !–

if (top.location != self.location)top.location=self.location;

// –>< /SCRIPT>
10. 网页将不能被另存为
< noscript>< iframe src=*.html>< /iframe>< /noscript>
11. < input type=button value=查看网页源代码
onclick=”window.location = “view-source:”+ “http://www.pconline.com.cn””>
12. 删除时确认
< a href=”javascript:if(confirm(” 确 实 要 删 除 吗 ?”))location=”boos.asp?&areyou= 删 除

&page=1″”>删除< /a>
13. 取得控件的绝对位置
//Javascript

< script language=”Javascript”>

function getIE(e){

var t=e.offsetTop;

var l=e.offsetLeft;

while(e=e.offsetParent){

t+=e.offsetTop;

l+=e.offsetLeft;

}

alert(“top=”+t+”/nleft=”+l);

}

< /script>

//VBScript

< script language=”VBScript”>< !–

function getIE()

dim t,l,a,b

set a=document.all.img1

t=document.all.img1.offsetTop

l=document.all.img1.offsetLeft

while a.tagName< >”BODY”

set a = a.offsetParent

t=t+a.offsetTop

l=l+a.offsetLeft

wend

msgbox “top=”&t&chr(13)&”left=”&l,64,”得到控件的位置”

end function

–>< /script>
14. 光标是停在文本框文字的最后
< script language=”javascript”>

function cc()

{

var e = event.srcElement;

var r =e.createTextRange();

r.moveStart(“character”,e.value.length);

r.collapse(true);

r.select();

}

< /script>

< input type=text name=text1 value=”123″ onfocus=”cc()”>
15. 判断上一页的来源
javascript:

document.referrer
16. 最小化、最大化、关闭窗口
< object id=hh1 classid=”clsid:ADB880A6-D8FF-11CF-9377-00AA003B7A11″>

< param name=”Command” value=”Minimize”>< /object>

< object id=hh2 classid=”clsid:ADB880A6-D8FF-11CF-9377-00AA003B7A11″>

< param name=”Command” value=”Maximize”>< /object>

< OBJECT id=hh3 classid=”clsid:adb880a6-d8ff-11cf-9377-00aa003b7a11″>

< PARAM NAME=”Command” VALUE=”Close”>< /OBJECT>

< input type=button value=最小化 onclick=hh1.Click()>

< input type=button value=最大化 onclick=hh2.Click()>

< input type=button value=关闭 onclick=hh3.Click()>
本例适用于 IE
17.屏蔽功能键 Shift,Alt,Ctrl
< script>

function look(){

if(event.shiftKey)

alert(“禁止按 Shift 键!”); //可以换成 ALT CTRL

}

document.onkeydown=look;

< /script>
18. 网页不会被缓存
< META HTTP-EQUIV=”pragma” CONTENT=”no-cache”>

< META HTTP-EQUIV=”Cache-Control” CONTENT=”no-cache, must-revalidate”>

< META HTTP-EQUIV=”expires” CONTENT=”Wed, 26 Feb 1997 08:21:57 GMT”>

或者< META HTTP-EQUIV=”expires” CONTENT=”0″>
19.怎样让表单没有凹凸感？
< input type=text style=”border:1 solid #000000″>

或

< input type=text style=”border-left:none; border-right:none; border-top:none; border-bottom:

1 solid #000000″>< /textarea>
20.< div>< span>&< layer>的区别？
< div>(division)用来定义大段的页面元素，会产生转行

< span>用来定义同一行内的元素，跟< div>的唯一区别是不产生转行

< layer>是 ns 的标记，ie 不支持，相当于< div>
21.让弹出窗口总是在最上面:
< body onblur=”this.focus();”>
22.不要滚动条?
让竖条没有:
< body style=”overflow:scroll;overflow-y:hidden”>

< /body>
让横条没有:
< body style=”overflow:scroll;overflow-x:hidden”>

< /body>
两个都去掉？更简单了
< body scroll=”no”>

< /body>
23.怎样去掉图片链接点击后，图片周围的虚线？
< a href=”#” onFocus=”this.blur()”>< img src=”logo.jpg” border=0>< /a>
24.电子邮件处理提交表单
< form name=”form1″ method=”post” action=”mailto:****@***.com” enctype=”text/plain”>

< input type=submit>

< /form>
25.在打开的子窗口刷新父窗口的代码里如何写？
window.opener.location.reload()
26.如何设定打开页面的大小
< body onload=”top.resizeTo(300,200);”>

打开页面的位置< body onload=”top.moveBy(300,200);”>
27.在页面中如何加入不是满铺的背景图片,拉动页面时背景图不动
< STYLE>

body

{background-image:none; background-repeat:no-repeat;

background-position:center;background-attachment: fixed}

< /STYLE>
28. 检查一段字符串是否全由数字组成
< script language=”Javascript”>< !–

function checkNum(str){return str.match(//D/)==null}

alert(checkNum(“1232142141”))

alert(checkNum(“123214214a1”))

// –>< /script>
29. 获得一个窗口的大小
document.body.clientWidth; document.body.clientHeight
30. 怎么判断是否是字符
if (/[^/x00-/xff]/g.test(s)) alert(“含有汉字”);

else alert(“全是字符”);
31.TEXTAREA 自适应文字行数的多少
< textarea rows=1 name=s1 cols=27 onpropertychange=”this.style.posHeight=this.scrollHeight”>

< /textarea>
32. 日期减去天数等于第二个日期
< script language=Javascript>

function cc(dd,dadd)

{

//可以加上错误处理

var a = new Date(dd)

a = a.valueOf()

a = a – dadd * 24 * 60 * 60 * 1000

a = new Date(a)

alert(a.getFullYear() + “年” + (a.getMonth() + 1) + “月” + a.getDate() + “日”)

}

cc(“12/23/2002”,2)

< /script>
33. 选择了哪一个 Radio
< HTML>< script language=”vbscript”>

function checkme()

for each ob in radio1

if ob.checked then window.alert ob.value

next

end function

< /script>< BODY>

< INPUT name=”radio1″ type=”radio” value=”style” checked>Style

< INPUT name=”radio1″ type=”radio” value=”barcode”>Barcode

< INPUT type=”button” value=”check” onclick=”checkme()”>

< /BODY>< /HTML>
34. 脚本永不出错
< SCRIPT LANGUAGE=”JavaScript”>

< !– Hide

function killErrors() {

return true;

}

window.onerror = killErrors;

// –>

< /SCRIPT>
35. ENTER 键可以让光标移到下一个输入框
< input onkeydown=”if(event.keyCode==13)event.keyCode=9″>
36. 检测某个网站的链接速度：
把如下代码加入< body>区域中:
< script language=Javascript>

tim=1

setInterval(“tim++”,100)

b=1

var autourl=new Array()

autourl[1]=”www.njcatv.net”

autourl[2]=”javacool.3322.net”

autourl[3]=”www.sina.com.cn”

autourl[4]=”www.nuaa.edu.cn”

autourl[5]=”www.cctv.com”

function butt(){

document.write(“< form name=autof>”)

for(var i=1;i< autourl.length;i++)

document.write(“< input type=text name=txt”+i+” size=10 value=测试中……> =》< input

type=text

name=url”+i+” size=40> =》< input type=button value=GO

onclick=window.open(this.form.url”+i+”.value)>

“)

document.write(“< input type=submit value=刷新>< /form>”)

}

butt()

function auto(url){

document.forms[0][“url”+b].value=url

if(tim>200)

{document.forms[0][“txt”+b].value=”链接超时”}

else

{document.forms[0][“txt”+b].value=”时间”+tim/10+”秒”}

b++

}

function run(){for(var i=1;i< autourl.length;i++)document.write(“< img

src=http://”+autourl+”/”+Math.random()+”

width=1 height=1

onerror=auto(“http://”+autourl+””)>”)}

run()< /script>
37. 各种样式的光标
auto ：标准光标
default ：标准箭头
hand ：手形光标
wait ：等待光标
text ：I 形光标
vertical-text ：水平 I 形光标
no-drop ：不可拖动光标
not-allowed ：无效光标
help ：?帮助光标
all-scroll ：三角方向标
move ：移动标
crosshair ：十字标
e-resize
n-resize
nw-resize
w-resize
s-resize
se-resize
sw-resize
38. 页面进入和退出的特效
进入页面< meta http-equiv=”Page-Enter” content=”revealTrans(duration=x, transition=y)”>
推出页面< meta http-equiv=”Page-Exit” content=”revealTrans(duration=x, transition=y)”>
这个是页面被载入和调出时的一些特效。duration 表示特效的持续时间，以秒为单位。
transition 表示使用哪种特效，取值为
1-23:
0 矩形缩小
1 矩形扩大
2 圆形缩小
3 圆形扩大
4 下到上刷新
5 上到下刷新
6 左到右刷新
7 右到左刷新
8 竖百叶窗
9 横百叶窗
10 错位横百叶窗
11 错位竖百叶窗
12 点扩散
13 左右到中间刷新
14 中间到左右刷新
15 中间到上下
16 上下到中间
17 右下到左上
18 右上到左下
19 左上到右下
20 左下到右上
21 横条
22 竖条
23 以上 22 种随机选择一种
39. 在规定时间内跳转
< META http-equiv=V=”REFRESH” content=”5;URL=http://www.51js.com”>
40. 网页是否被检索
< meta name=”ROBOTS” content=”属性值”>
其中属性值有以下一些:
属性值为”all”: 文件将被检索，且页上链接可被查询；
属性值为”none”: 文件不被检索，而且不查询页上的链接；
属性值为”index”: 文件将被检索；
属性值为”follow”: 查询页上的链接；
属性值为”noindex”: 文件不检索，但可被查询链接；
属性值为”nofollow”: 文件不被检索，但可查询页上的链接。
41. 回车
用客户端脚本在页面添加document 的onkeydown事件,让页面在接受到回车事件后,进行Tab
键的功能,即只要把 event 的 keyCode 由 13 变为 9
Javascript 代码如下:
<script language=”javascript” for=”document” event=”onkeydown”>

<!–

if(event.keyCode==13)

event.keyCode=9;

–>

</script>
这样的处理方式,可以实现焦点往下移动,但对于按钮也起同样的作用,一般的客户在输入完
资料以后,跳到按钮后,最好能直接按”回车”进行数据的提交.因此,对上面的方法要进行一下
修改,应该对于”提交”按钮不进行焦点转移.而直接激活提交.
因此我对上面的代码进行了一个修改,即判断事件的”源”,是否为提交按钮,代码如下:
<script language=”javascript” for=”document” event=”onkeydown”>

<!–

if(event.keyCode==13 && event.srcElement.type!=’button’ &&

event.srcElement.type!=’submit’ && event.srcElement.type!=’reset’ &&

event.srcElement.type!=’textarea’ && event.srcElement.type!=”)

event.keyCode=9;

–>

</script>
判断是否为 button, 是因为在 HTML 上会有 type=”button”
判断是否为 submit,是因为 HTML 上会有 type=”submit”
判断是否为 reset,是因为 HTML 上的”重置”应该要被执行
判断是否为空,是因为对于 HTML 上的”<a>链接”也应该被执行，这种情况发生的情况不多，可以使用”tabindex=-1″的方式来取消链接获得焦点。

```

## 1.11 收藏写法

```html
<html>

<head>
	<title>收藏的写法</title>
	<script language="javascript">
		function addBookmark(url,title) {
			if (window.sidebar) {
				window.sidebar.addPanel(title, url,"");
			}
			else if(document.all) {
				window.external.AddFavorite( url, title);
			}
			else if( window.opera && window.print ) {
				return true;
			}
		}
		function touch(){
			document.myform.submit();
			//document.myform.reset();
		}
	</script>
</head>
<body>
	<!-- 需要挂靠上面的函数 -->
	<a href="javascript:addBookmark('http://www.icbc.com.cn','中国工商银行');">加入收藏</a>
	<!-- 可以直接使用 -->
	<a href="javascript:window.external.AddFavorite('http://localhost:8080/')">加入收藏</a>
	
	
	<!-- 使用链接来提交表单 -->
	<form action="base1.html" method="get" name="myform" >
		测试：<input type="text" name="myname" />
	</form>
	
	
	<a href="#" onClick="touch()" >提交表单</a>
	
	<input type="password" name="***" />
</body>
</html> 
```

![](G:\document\易途\官方课件\阶段3[javaWeb+js+Ajax+jQuery+dom4J]\js\JavaScriptDaysOfficial\attachment\BOM和DOM关系示意图.jpg)

## 1.12  DOM对象

HTML DOM 对象参考手册

请点击下面的链接，学习更多有关对象及其集合、属性、方法和事件的知识。其中包含大量实例！


对象

描述

Document 代表整个 HTML 文档，可被用来访问页面中的所有元素 
Anchor 代表 <a> 元素 
Area 代表图像映射中的 <area> 元素 
Base 代表 <base> 元素 
Body 代表 <body> 元素 
Button 代表 <button> 元素 
Event 代表某个事件的状态 
Form 代表 <form> 元素 
Frame 代表 <frame> 元素 
Frameset 代表 <frameset> 元素 
Iframe 代表 <iframe> 元素 
Image 代表 <img> 元素 
Input button 代表 HTML 表单中的一个按钮 
Input checkbox 代表 HTML 表单中的复选框 
Input file 代表 HTML 表单中的文件上传 
Input hidden 代表 HTML 表单中的隐藏域 
Input password 代表 HTML 表单中的密码域 
Input radio 代表 HTML 表单中的单选按钮 
Input reset 代表 HTML 表单中的重置按钮 
Input submit 代表 HTML 表单中的确认按钮 
Input text 代表 HTML 表单中的文本输入域（文本框） 
Link 代表 <link> 元素 
Meta 代表 <meta> 元素 
Object 代表 <Object> 元素 
Option 代表 <option> 元素 
Select 代表 HTML 表单中的选择列表 
Style 代表单独的样式声明 
Table 代表 <table> 元素 
TableData 代表 <td> 元素 
TableRow 代表 <tr> 元素 
Textarea 代表 <textarea> 元素

## 1.13 dom基本操作

![](G:\document\易途\官方课件\阶段3[javaWeb+js+Ajax+jQuery+dom4J]\js\JavaScriptDaysOfficial\attachment\dom基本操作.gif)

##  1.14  js  BOM 对象简介（了解）

```
Javascript BOM

	BOM是browser object model的缩写，简称浏览器对象模型
	BOM提供了独立于内容而与浏览器窗口进行交互的对象
	由于BOM主要用于管理窗口与窗口之间的通讯，因此其核心对象是window
	BOM由一系列相关的对象构成，并且每个对象都提供了很多方法与属性
	BOM缺乏标准，JavaScript语法的标准化组织是ECMA，DOM的标准化组织是W3C
	BOM最初是Netscape浏览器标准的一部分

window：
	window对象是BOM的顶层(核心)对象，所有对象都是通过它延伸出来的，也可以称为window的子对象

	由于window是顶层对象，因此调用它的子对象时可以不显示的指明window对象，例如下面两行代码是一样的：

	document.write("www.dreamdu.com");
	window.document.write("www.dreamdu.com");
	全局的window对象
	JavaScript中的任何一个全局函数或变量都是window的属性

	var sTest="dreamdu";
	document.write(sTest==window.sTest);

	结果为true

	常用函数：

		window.alert("要显示的对话框");
		
		window.confirm("可选择式对话框");
		
		window.prompt(str1,str2);
		str1:表示要显示在消息对话框中的文本，不可修改
		str2:文本框中的内容，可以修改

		窗体控制函数
		JavaScript moveBy() 函数
		JavaScript moveTo() 函数
		JavaScript resizeBy() 函数
		JavaScript resizeTo() 函数

		窗体滚动轴控制函数
		JavaScript scrollTo() 函数
		JavaScript scrollBy() 函数

		窗体焦点控制函数
		JavaScript focus() 函数
		JavaScript blur() 函数

		新建窗体函数
		JavaScript open() 函数
		JavaScript close() 函数
		JavaScript opener 属性

		对话框函数
		JavaScript alert() 函数
		JavaScript confirm() 函数
		JavaScript prompt() 函数

		状态栏属性
		JavaScript window.defaultStatus 属性
		JavaScript window.status 属性

		时间等待与间隔函数
		JavaScript setTimeout() 函数
		JavaScript clearTimeout() 函数
		JavaScript setInterval() 函数
		JavaScript clearInterval() 函数
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
document:
	document是BOM中最重要对象之一
	document对象是window对象的属性
	document对象包含一个节点对象，此对象包含每个单独页面的所有HTML元素，这就是W3C的DOM对象

	由于document代表HTML文档的内容，因此可以通过它表示文档中加载的一些元素，这些元素全部通过集合访问。
	anchors -- 文档中所有锚(a name="aname")的集合
	applets -- 文档中所有applet标签表示的内容的集合
	embeds -- 文档中所有embed标签表示的内容的集合
	forms -- 文档中所有form标签表示的内容的集合
	images -- 文档中所有image标签表示的内容的集合
	links -- 文档中所有a(链接)标签表示的内容的集合

	document函数
	JavaScript write() 函数
	JavaScript writeln() 函数
	JavaScript document.open() 函数
	JavaScript document.close() 函数

	示例
	<form name="form1"><a href="http://www.dreamdu.com/xhtml/" name="a1">xhtml</a></form>
	<form name="form2"><a href="http://www.dreamdu.com/css/" name="a2">css</a></form>
	<form name="form3"><a href="http://www.dreamdu.com/javascript/" name="a3">javascript</a></form>

	<input type="button" value="显示第二个表单的名称" onclick="alert(document.forms[1].name)" />
	<input type="button" value="显示第二个表单的名称第二种方法" onclick="alert(document.forms['form2'].name)" />
	<input type="button" value="显示第三个链接的名称" onclick="alert(document.links[2].name)" />
	<input type="button" value="显示第三个链接的名称第二种方法" onclick="alert(document.links['a3'].name)" />
	<input type="button" value="显示第三个链接href属性的值" onclick="alert(document.links[2].href)" />

	表示第二个表单的方法：document.forms[1]或document.forms["form2"]

	表示第三个链接的方法：document.links[2]或document.links["a3"]

	表示第三个链接href属性的方法：document.links[2].href
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
frames:
	frames，中文"框架"frames对象是window对象的属性
	如果页面使用框架，将产生一个框架集合frames，在集合中可用数字从0开始，从左到右，逐行索引)或名字索引框架
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
history:
	history，中文"历史"
	history对象是window对象的属性
	浏览者通常可以使用浏览器的前进与后退按钮访问曾经浏览过的页面。JavaScript的history对象记录了用户曾经浏览过的页面，并可以实现浏览器前进与后退相似的导航功能
	可以通过back函数后退一个页面，forward函数前进一个页面，或者使用go函数任意后退或前进页面，还可以通过length属性查看history对象中存储的页面数
	history对象函数
	JavaScript history.go() 函数 -- 前进或后退指定的页面数
	JavaScript history.back() 函数 -- 后退一页
	JavaScript history.forward() 函数 -- 前进一页

	history对象属性
	JavaScript length 属性 -- history对象中缓存了多少个URL
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
location:
	location用于获取或设置窗体的URL，并且可以用于解析URL，是BOM中最重要的对象之一,location，中文"位置"
location既是window对象的属性又是document对象的属性
location包含8个属性，其中7个都是当前窗体的URL的一部分，剩下的也是最重要的一个是href属性，代表当前窗体的URL,location的8个属性都是可读写的，但是只有href与hash的写才有意义。例如改变location.href会重新定位到一个URL，而修改location.hash会跳到当前页面中的anchor(<a id="name">或者<div id="id">等)名字的标记(如果有)，而且页面不会被重新加载

document.write(window.location==document.location);

其它见示意图

location.href = "要跳转的页面地址";
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
navigator:
	navigator对象通常用于检测浏览器与操作系统的版本
	navigator，中文"导航器"
	navigator对象是window对象的属性
	由于navigator没有统一的标准，因此各个浏览器都有自己不同的navigator版本
	常用的navigator属性
	appCodeName -- 浏览器代码名的字符串表示
	appName -- 官方浏览器名的字符串表示
	appVersion -- 浏览器版本信息的字符串表示
	cookieEnabled	-- 如果启用cookie返回true，否则返回false
	javaEnabled -- 如果启用java返回true，否则返回false
	platform -- 浏览器所在计算机平台的字符串表示
	plugins -- 安装在浏览器中的插件数组
	taintEnabled -- 如果启用了数据污点返回true，否则返回false
	userAgent	-- 用户代理头的字符串表示

	navigator中最重要的是userAgent属性，返回包含浏览器版本等信息的字符串，其次cookieEnabled也很重要，使用它可以判断用户浏览器是否开启cookie。
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
screen:
	screen对象用于获取用户的屏幕信息
	screen对象是window对象的属性
	screen对象属性
	JavaScript availHeight 属性 -- 窗口可以使用的屏幕高度，单位像素
	JavaScript availWidth 属性 -- 窗口可以使用的屏幕宽度，单位像素
	JavaScript colorDepth 属性 -- 用户浏览器表示的颜色位数，通常为32位(每像素的位数)
	JavaScript pixelDepth 属性 -- 用户浏览器表示的颜色位数，通常为32位(每像素的位数)（IE不支持此属性）
	JavaScript height 属性 -- 屏幕的高度，单位像素
	JavaScript width 属性 -- 屏幕的宽度，单位像素

	availWidth与availHeight属性非常有用，例如：可以使用下面的代码填充用户的屏幕：

	示例
	window.moveTo(0,0);
	window.resizeTo(screen.availWidth, screen.availHeight);
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
DOM和BOM的不同？


Document Object Model（文档对象模型），就是把「文档」当做一个「对象」来看待。
Browser Object Model（浏览器对象模型）,即把「浏览器」当做一个「对象」来看待。
区别：DOM描述了处理网页内容的方法和接口，BOM描述了与浏览器进行交互的方法和接口。

在 DOM 中，文档中的各个组件（component），可以通过 object.attribute 这种形式来访问。一个 DOM 会有一个根对象，这个对象通常就是 document。而 BOM 除了可以访问文档中的组件之外，还可以访问浏览器的组件，比如问题描述中的 navigator（导航条）、history（历史记录）等等。在这种 「XOM」的模型中，最应该理解的就是 Object Model。Object Model 就表示你可以通过像操作对象一样，来操作这个 X。再解释一下什么是对象（Object）。在编程领域中，对象就是指的一种拥有具体数据（Data）并且具有（并不总是）特定行为（Behavior）的东西。例如，一个人 ，就可以看做一个对象。人的年龄、性别、身高、体重就是上文说的具体「数据」，通常将对象拥有的具体数据称作对象的「属性（Attribute）」；而人吃饭，睡觉，行走等能力，就是上文所说的「行为」，通常，我们把对象的行为称作对象的「方法（Method）」。另外，对象是可以嵌套的，也就是是说，一个对象的属性也可以是对象。上文所说的「像操作对象一样」，最主要就是指访问对象的属性和调用对象的方法。对象的属性可以通过 object.attribute 这种形式来访问，对象的方法可以通过 object.method(arguments) 这种形式来调用。对应到 DOM 中，document 这个根对象就有很多属性，例如 title 就是 document 的一个属性，可以通过 document.title 访问；document 对象也有很多方法，例如 getElementById，可以通过 document.getElementById(nodeId) 调用。


1. DOM 是 W3C 的标准； [所有浏览器公共遵守的标准]
2. BOM 是 各个浏览器厂商根据 DOM
在各自浏览器上的实现;[表现为不同浏览器定义有差别,实现方式不同]
3. window 是 BOM 对象，而非 js 对象；
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
window
window.close(); //关闭窗口 
 
window.alert("message"); //弹出一个具有OK按钮的系统消息框，显示指定的文本 
 
window.confirm("Are you sure?"); //弹出一个具有OK和Cancel按钮的询问对话框，返回一个布尔值 
 
window.prompt("What's your name?", "Default"); //提示用户输入信息，接受两个参数，即要显示给用户的文本和文本框中的默认值，将文本框中的值作为函数值返回 
 
window.status //可以使状态栏的文本暂时改变 
 
window.defaultStatus //默认的状态栏信息，可在用户离开当前页面前一直改变文本 
 
window.setTimeout("alert('xxx')", 1000); //设置在指定的毫秒数后执行指定的代码，接受2个参数，要执行的代码和等待的毫秒数 
 
window.clearTimeout("ID"); //取消还未执行的暂停，将暂停ID传递给它 
 
window.setInterval(function, 1000); //无限次地每隔指定的时间段重复一次指定的代码，参数同setTimeout()一样 
 
window.clearInterval("ID"); //取消时间间隔，将间隔ID传递给它 
 
window.history.go(-1); //访问浏览器窗口的历史，负数为后退，正数为前进 
 
window.history.back(); //同上 
 
window.history.forward(); //同上 
 
window.history.length //可以查看历史中的页面数  
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
document

document对象：实际上是window对象的属性，document == window.document为true，是唯一一个既属于BOM又属于DOM的对象 
 
document.lastModified //获取最后一次修改页面的日期的字符串表示 
 
document.referrer //用于跟踪用户从哪里链接过来的 
 
document.title //获取当前页面的标题，可读写 
 
document.URL //获取当前页面的URL，可读写 
 
document.anchors[0]或document.anchors["anchName"] //访问页面中所有的锚 
 
document.forms[0]或document.forms["formName"] //访问页面中所有的表单 
 
document.images[0]或document.images["imgName"] // 访问页面中所有的图像 
 
document.links [0]或document.links["linkName"] //访问页面中所有的链接 
 
document.applets [0]或document.applets["appletName"] //访问页面中所有的Applet 
 
document.embeds [0]或document.embeds["embedName"] //访问页面中所有的嵌入式对象 
 
document.write(); 或document.writeln(); //将字符串插入到调用它们的位置 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
location

location对象：表示载入窗口的URL，也可用window.location引用它 
 
location.href //当前载入页面的完整URL，如http://www.somewhere.com/pictures/index.htm 
 
location.portocol //URL中使用的协议，即双斜杠之前的部分，如http 
 
location.host //服务器的名字，如www.wrox.com 
 
location.hostname //通常等于host，有时会省略前面的www 
 
location.port //URL声明的请求的端口，默认情况下，大多数URL没有端口信息，如8080 
 
location.pathname //URL中主机名后的部分，如/pictures/index.htm 
 
location.search //执行GET请求的URL中的问号后的部分，又称查询字符串，如?param=xxxx 
 
location.hash //如果URL包含#，返回该符号之后的内容，如#anchor1 
 
location.assign("http:www.baidu.com"); //同location.href，新地址都会被加到浏览器的历史栈中 
 
location.replace("http:www.baidu.com"); //同assign()，但新地址不会被加到浏览器的历史栈中，不能通过back和forward访问 
 
location.reload(true | false); //重新载入当前页面，为false时从浏览器缓存中重载，为true时从服务器端重载，默认为false 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
navigator

`navigator`对象：包含大量有关Web浏览器的信息，在检测浏览器及操作系统上非常有用，也可用window.navigator引用它 
 
`navigator.appCodeName` //浏览器代码名的字符串表示 
 
navigator.appName //官方浏览器名的字符串表示 
 
navigator.appVersion //浏览器版本信息的字符串表示 
 
navigator.cookieEnabled //如果启用cookie返回true，否则返回false 
 
navigator.javaEnabled //如果启用java返回true，否则返回false 
 
navigator.platform //浏览器所在计算机平台的字符串表示 
 
navigator.plugins //安装在浏览器中的插件数组 
 
navigator.taintEnabled //如果启用了数据污点返回true，否则返回false 
 
navigator.userAgent //用户代理头的字符串表示  
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
screen

screen对象：用于获取某些关于用户屏幕的信息，也可用window.screen引用它 
 
screen.width/height //屏幕的宽度与高度，以像素计 
 
screen.availWidth/availHeight //窗口可以使用的屏幕的宽度和高度，以像素计 
 
screen.colorDepth //用户表示颜色的位数，大多数系统采用32位 
 
window.moveTo(0, 0); 
 
window.resizeTo(screen.availWidth, screen.availHeight); //填充用户的屏幕  

```

## 1.15  js常见的事件

![](G:\document\易途\官方课件\阶段3[javaWeb+js+Ajax+jQuery+dom4J]\js\JavaScriptDaysOfficial\attachment\JavaScript的常用事件 .bmp)

## 11.6  （拓展）js的   Trim方法（）

```javascript

//删除字符串左边的空格 
function ltrim(str) { 
if(str.length==0) 
		return(str); 
	else { 
		var idx=0; 
		//在索引处找到空格
		while(str.charAt(idx).search("\s")==0) 
		idx++; 
		return(str.substr(idx)); 
	} 
} 

//删除字符串右边的空格 
function rtrim(str) { 
if(str.length==0) 
return(str); 
else { 
var idx=str.length-1; 
while(str.charAt(idx).search(/\s/)==0) 
idx--; 
return(str.substring(0,idx+1)); 
} 
} 
\s 匹配任何空白字符，包括空格、制表符、换页符等等。等价于 [ \f\n\r\t\v]。 
\S 匹配任何非空白字符。等价于 [^ \f\n\r\t\v]。 

search()方法用于从字符串中寻找指定值的位置
返回与正则表达式查找内容匹配的第一个子字符串的位置,没有匹配返回-1
ltrim(rtrim(str))
 "etoak"
```



## 11.7 (拓展)js常用校验

```javascript
js中删除空格和一些校验
关键字: 删除空格和校验 

匹配除 "\n" 之外的任何单个字符。要匹配包括 '\n' 在内的任何字符，请使用象 '[.\n]' 的模式。 
\s 匹配任何空白字符，包括空格、制表符、换页符等等。等价于 [ \f\n\r\t\v]。 
\S 匹配任何非空白字符。等价于 [^ \f\n\r\t\v]。 

search()方法用于从字符串中寻找指定值的位置

//是否为空校验 
function isEmpty(s) { 
var lll=trim(s); 
if( lll == null || lll.length == 0 ) 
return true; 
else 
return false; 
} 

//删除字符串左边的空格 
function ltrim(str) { 
if(str.length==0) 
return(str); 
else { 
var idx=0; 
while(str.charAt(idx).search(/\s/)==0) 
idx++; 
return(str.substr(idx)); 
} 
} 

//删除字符串右边的空格 
function rtrim(str) { 
if(str.length==0) 
return(str); 
else { 
var idx=str.length-1; 
while(str.charAt(idx).search(/\s/)==0) 
idx--; 
return(str.substring(0,idx+1)); 
} 
} 

//删除字符串左右两边的空格 
function trim(str) { 
return(rtrim(ltrim(str))); 
} 

/*日期相比较*/ 
function compareDate(date1, date2) { 
if (trim(date1) == trim(date2))  
return 0; 
if (trim(date1) > trim(date2))  
return 1; 
if (trim(date1) < trim(date2))  
return -1; 
} 

//校验是否是Email 
function isEmail(eml) { 
if(trim(eml)!='') { 
  var re=new RegExp("@[\\w]+(\\.[\\w]+)+$"); 
  return(re.test(eml)); 
} 
else 
  return(true); 
} 

//是否是电话号 
function isTel(tel) { 
var charcode; 
for (var i=0; i<tel.length; i++) 
{ 
charcode = tel.charCodeAt(i); 
if (charcode < 48 && charcode != 45 || charcode > 57) 
return false; 
} 
return true; 
} 

//校验是否是实数 
function isnumber(num) { 
var re=new RegExp("^-?[\\d]*\\.?[\\d]*$"); 
if(re.test(num)) 
return(!isNaN(parseFloat(num))); 
else 
return(false); 
} 

//校验是否是整数 
function isinteger(num) { 
var re=new RegExp("^-?[\\d]*$"); 
if(re.test(num)) 
return(!isNaN(parseInt(num))); 
else 
return(false); 
} 

```

