# HTML

## 1 .0  HTML简介

​    ~Hyper Text Markup Language~
~HTML~
~超文本标记语言~

1993年由W3C互联网组织发布了第一版，专门用来渲染页面的
结构和样式（之后样式功能被css取代），由于第一版较为混乱
之后第二版正式被网页定为默认的规范，页面上所有的要素都被
标签（Tag）来渲染，至今已经是5.1版本


### HTML 5 的改进

　　2007年HTML 5草案被W3C接纳，并成立了新的HTML工作团队。 　　2008年1月22日第一份正式HTML 5草案发布。 　　HTML5增加了更多样化的API，提供了嵌入音频、视频、图片的函数、客户端数据存储，以及交互式文档。其他特性包括新的页面元素，比如 <header>, <section>, <footer>, 以及 <figure>。 　　HTML 5 通过制定如何处理所有 HTML 	元素以及如何从错误中恢复的精确规则，改进了互操作性，并减少了开发成本。一些新的元素和属性，反映典型的现代用法网站。其中有些是技术上类似<div>和<span>标签，但有一个含义，例如<nav>（网站导航块）和<footer>。这种标签将有利於搜索引擎的索引整理、小萤幕装置和视障人士使用。同时为其他浏览要素提供了新的功能，通过一个标准介面，如<audio>和<video>标记。 　　一些过时的HTML4标记将取消。其中包括纯粹显示效果的标记，如<font>和<center>，因为它们已经被CSS取代。 　　HTML 5草案的前身名为Web Applications 1.0，是在2004年由WHATWG提出，再于2007年获W3C接纳，并成立了新的HTML工作团队。在2008年1月22日，第一份正式草案发布。[1]WHATWG表示该规范是目前仍在进行的工作，仍须多年的努力。[2]目前Firefox、Chrome、Opera、Safari（版本4以上）及Internet Explorer 9（Platform Preview）已支援HTML5技术。HTML 5的标准草案目前已进入W3C制定标准5大程序的第1步。负责编纂标准格式文件的Google代表Ian Hickson预期，可能得等到2012年才会推出建议候选版（W3C Candidate Recommendation）。 　　目前IE还不支持HTML5，火狐firefox,Chrome内核都已经支持HTML的渲染

###  1.1 h5 扩展知识

1、新的文档类型声明(DTD)
文档类型声明
HTML 5的DTD声明为:
    <!doctype html>
    <!DOCTYPE html >等也是正确的，因为HTML语法是不区分大小写的。
在编写HTML5文档时，要求指定文档类型，以确保浏览器能在Html5的标准模式下进行渲染。
2、新增的HTML5标签
a，新增的HTML5标签-结构标签
结构标签：(块状元素) 有意义的div
 <article>  标记定义一篇文章
 <header>  标记定义一个页面或一个区域的头部
  <nav>   标记定义导航链接
 <section>  标记定义一个区域
 <aside>  标记定义页面内容部分的侧边栏
 <hgroup>  标记定义文件中一个区块的相关信息
 <figure>  标记定义一组媒体内容以及它们的标题 
 <figcaption>  标签定义 figure 元素的标题。
 <footer>  标记定义一个页面或一个区域的底部
 <dialog>  标记定义一个对话框(会话框)类似微信
新的结构标签带来的是网页布局的改变及提升对搜索引擎的友好

b，新增的HTML5标签-多媒体标签
多媒体交互标签
 <video> 标记定义一个视频
 <audio> 标记定义音频内容
  <source> 标记定义媒体资源
 <canvas> 标记定义图片
 <embed> 标记定义外部的可交互的内容或插件 比如flash

HTML5的多媒体标签的出现意味着富媒体的发展以及支持不使用插件的情况下即可操作媒体文件，极大地提升了用户体验
c，新增的HTML5标签-Web应用标签
Web应用标签
<menu>命令列表
<menuitem>menu命令列表标签 FF（嵌入系统）
<command> menu标记定义一个命令按钮
<meter>状态标签(实时状态显示:气压、气温)C、O
<progress>状态标签 (任务过程:安装、加载) C、F、O
<datalist> 为input标记定义一个下拉列表,配合option F、O
<details> 标记定义一个元素的详细内容 ，配合dt、dd   C
d，新增的HTML5标签-其他标签
注释标签
<ruby> 标记定义 注释或音标
<rp> 告诉那些不支持 Ruby元素的浏览器如何去显示
 <rt> 标记定义对ruby的注释内容文本
其他标签
<keygen> 标记定义表单里一个生成的键值(加密信息传送)O、F
<mark> 标记定义有标记的文本 (黄色选中状态)
<output> 标记定义一些输出类型,计算表单结果配合oninput事件

<time> 标记定义一个日期/时间 目前所有主流浏览器都不支持
3、删除的HTML标签
纯表现的元素：
basefont，big，center，font, s，strike，tt，u；
对可用性产生负面影响的元素：
frame，frameset，noframes；
产生混淆的元素：
acronym ，applet，isindex，dir。
4、重新定义的HTML标签
HTML元素 HTML5中的意义
<b> 代表内联文本，通常是粗体，没有传递表示重要的意思
<i> 代表内联文本，通常是斜体，没有传递表示重要的意思
<dd> 可以同details与figure一同使用，定义包含文本，dialog也可用
<dt> 可以同details与figure一同使用，汇总细节，dialog也可用
<hr> 表示主题结束，而不是水平线，虽然显示相同
<menu> 重新定义用户界面的菜单，配合commond或者menuitem使用
<small> 表示小字体，例如打印注释或者法律条款
<strong> 表示重要性而不是强调符号

### h5 和h4的区别

Html5新标签解释及用法 
HTML 5 是一个新的网络标准，目标在于取代现有的 HTML 4.01, XHTML 1.0 and DOM Level 2 HTML 标准。它希望能够减少浏览器对于需要插件的丰富性网络应用服务（plug-in-based rich internet application，RIA)，如Adobe Flash, Microsoft Silverlight, 与 Sun JavaFX 的需求。
HTML 5 提供了一些新的元素和属性，反映典型的现代用法网站。其中有些是技术上类似 <div> 和 <span> 标签，但有一定含义，例如 <nav>（网站导航块）和 <footer>。这种标签将有利于搜索引擎的索引整理、小屏幕装置和视障人士使用。同时为其他浏览要素提供了新的功能，通过一个标准接口，如 <audio> 和 <video> 标记。

一些过时的 HTML 4 标记将取消，其中包括纯粹用作显示效果的标记，如 <font> 和 <center>，因为它们已经被 CSS 取代。还有一些透过 DOM 的网络行为（via）。

###  meta关键字

Meta Tag 的简介

一般常用的格式如下：

<Title>All Products Online</title>
这虽说不是meta的一部份，但是也不可忽略，总长度不要超过85个Character (约10个字)。


<meta http-quive="content-type" content="text/html; charset="iso-8859-1">
说明网站的格式及编码方式，另外这个功能也可以拿来说明网站的名称。


<meta name="keyword" contents="关键字一, 关键字二, 关键字三, …..">
铲明整个网站的关键字，关键字间用逗点隔开，总长度最好不要超过1000个Character (约44个字)。


<meta name="description" contents="整个网站的描述….">
铲明整个网站为何吸引人的地方，可用逗点隔开，总长度最好不要超过200个Character (约15个字)，若文章真的太长，可以切割成两个部分，较不重要的部分置入下一个Description。


<meta name="robots" content=" ALL, NONE, INDEX, NOINDEX, FOLLOW, NOFOLLOW">
此功能是要给搜寻引擎使用的，是要用来告诉Spider哪些网页是要去撷取的或不用去撷取的，一般都设定成All(内定值)。 

##  01 . h5基本标记

 Tag标签的种类
	1):开闭合标签
		<标签名>需要被渲染的文本</标签名>

​      <i>需要被倾斜的文本</i>
2):整合标签
		<标签名 /> <标签名>

#### 页面常用标记

```html
<!--
	html文件以.html 或者 .htm为后缀结尾
	全文不区分大小写，这是你所学的内容中唯一不区分
	大小写的技术
	html具有较高的容错性。一般由浏览器打开从上到下
	进行解析，不同的浏览器存在浏览器差异性

	<!DOCTYPE html>:html5的DTD信息
	所谓DTD全称为Document Type Definition
	文档类型定义，规定了使用何种规范，可以使用哪些标签
-->
<!DOCTYPE html>
<!--
	html:表示html文件的开始，是全文的根元素
	html和xml两种技术都只有一个根元素
	注意内部的元素称之为子元素，子元素在书写时
	应该在父元素内部向右一个制表符或者根据最新的
	google规范向右2个空格
-->
<html>
	<!--
		head:表示全文的头信息
		一般用来引入外部css js信息
		或者设置当前页面的编码
	-->
	<head>
		<!--
			title:设置页面的标题
		-->
		<title>我的第一个html</title>
		<!--
			meta:用来设置编码 关键字 网页描述
		-->
		<meta charset="utf-8" />
	</head>
	<!--
		body:表示页面的正文
			bgcolor:设置背景色
	-->
	<body bgcolor="yellow">
		<!--1:
			h1~h6:标题共分为六级
			随着序号的不断增大字体不断减小，
			自动换行
		-->
		<h1>济南南部山区失火啦！！！</h1>
		<h2>我是二级标题</h2>
		<!--2:
			center:强制居中对齐，嵌套在内部的
			内容会被强制居中对齐，此标签在html5
			规范中已经不推荐使用
		-->
		<center><h3>我是三级标题</h3></center>
		<h4>我是四级标题</h4>
		<h5>我是五级标题</h5>
		<h6>我是六级标题</h6>

		<!--
			3:
				font:设置字体,此标签
				通过属性设置字体颜色大小，html5当中
				已经不推荐使用，但是此标签在后续开发中
				使用较多
				<标签 属性名="属性值">文本</标签>
				face:设置字体
				color:设置颜色
				size:设置字体大小
		-->
		<font face="楷体" color="#FF8C69"
		size="20px">济南的夏天就要到来了</font>
		<!--
			4:
				一组渲染样式的标签，以下标签仅作为
				了解，已经全部被css取代
		-->
		<b>加粗</b>
		<strong>加粗</strong>
		<i>倾斜</i>
		<em>倾斜</em>
		<ins>下划线</ins>
		<del>贯穿线</del>
		<!--
			5:
				一组用来设置文本区域的标签
				段落自带换行，并且空一行
				div自带换行，没有空一行
				label与span多配合js使用不换行也没有空行，单独使用没有任何效果
		-->
		<p>我是一个段落</p>
		<div>我是一个div层</div>
		<label>我是一个label</label>
		<span>我是一个span</span>
		<!--
			6:
				转义字符
				在html中以下字符不能直接书写
				必须使用转义字符
				> < 空格
		-->
		大于号:&gt;
		小于号:&lt;
		空格:&nbsp;
 
	</body>
</html>




```

#### 超链接标记

```html
<!DOCTYPE html>
<html>
	<head>
		<title>2:链接，列表，表格</title>
		<meta charset="utf-8" />
	</head>
	<body>
		<!--
			1:
				a:链接
					href:表示链接到的目的地
					默认情况下链接自带下划线
					target="_blank":目的地和原页面
					同时展示在浏览器上
		-->
		<a href="base1.html">我是一个链接</a>
		<a href="base1.html"
		target="_blank">我是一个链接</a>
		<!--
			2:
				img:展示一张图片
					src：source的简写表示图片的源
		-->
		<img src="etoak.jpg" 
		width="200px">
		<img src="image/etoak2.jpg"
		width="200px">
		<img src="image/image2/etoak3.jpg" 
		width="200px">
		<!--
			../:表示跳出当前目录
		<img src="../etoak.jpg" 
		width="200px" />
		<img src="../image/etoak2.jpg"
		width="200px" />
		<img src="../image/image2/etoak3.jpg"
		width="200px" />
		-->

		<a href="base1.html">
			<img src="etoak.jpg" 
			width="100px" />
		</a>
		<!--
			3:
				列表
				ul:无序列表
				ol:有序列表
		-->
		<ul>
			<li><a href="base1.html">列表1</a></li>
			<li>列表2</li>
			<li>列表3</li>
			<li><img src="etoak.jpg" 
			width="50px"></li>
		</ul>
		<ol>
			<li>列表1</li>
			<li>列表2</li>
			<li>列表3</li>
			<li>列表4</li>
		</ol>

		<!--
			4:
				table:表格，table表示表格的开始
				tr表示一行，td表示一列，
				table tr td 缺一不可
				caption:表格的标题 不是必须
				width:宽度
				height:高度
				border:显示表格边框
				cellspacing:设置表格边框和单元格
				边框之间的距离，如果设置0则表示
				两个边框重合
				bordercolor:设置边框颜色
		-->
		<table width="500px" height="400px"
		border="1px"
		cellspacing="0"
		bordercolor="red">
			<caption>我是表格标题</caption>
			<tr bgcolor="lightblue">
				<!--
					align:水平对齐
						right left center
				-->
				<td align="right">列1</td>
				<td align="left">列2</td>
				<td align="center">列3</td>
				<td>列4</td>
			</tr>
			<tr>
				<!--
					valign:垂直对齐
					top bottom middle
				-->
				<td valign="top">列1</td>
				<td valign="bottom">列2</td>
				<td valign="middle">列3</td>
				<td>列4</td>
			</tr>
			<!--
				上面一行是多少列，下面一行
				必须多少列
				如果想改变列数可以通过合并单元格的方式
			-->
			<tr>
				<td colspan="4">列1</td>
			</tr>
			<tr>
				<td>列1</td>
				<td>列2</td>
				<td>
					<!--
						表格嵌套在表格的td中可以再次
						嵌套一个table表格
					
						width:设置百分比，100%
						表示占满整个td
						height:设置百分比存在严重的
						浏览器差异性，部分浏览器设置
						百分比无效
					-->
					<table border="1px"
					width="100%" height="100%">
						<tr>
							<td>内部列1</td>
							<td>内部列2</td>
						</tr>
						<tr>
							<td>内部列1</td>
							<td>内部列2</td>
						</tr>
					</table>
				</td>
				<td bordercolor="red">列4</td>
			</tr>
		</table>
 
	</body>
</html>
```

####  form表单

```html
<!DOCTYPE html>
<html>
	<head>
		<title>3:表单</title>
		<meta charset="utf-8" />
	</head>
	<body>
		<!--
			form:表示表单的开始，表单是一种高级的
			链接，可以通过表单项采集用户输入的数据，
			提交到action属性设置的静态或者动态页面
			action:表单数据提交到的目的地
			method:提交的方式 有get和post两种

			#表单提交时，get和post有什么区别?
			get:速度快，安全性低，最多传递256个字符
			不支持中文，通过浏览器地址栏进行传递
			格式:
			?key1=value1&key2=value2&keyN=valueN
			链接传递值肯定是通过get
			<a href="base1.html?key1=value1&key2=value2">链接</a>
	
			post:速度慢，安全性高，没有大小限制，
			不支持中文，不通过浏览器地址栏进行传递，
			传递格式和get相同
			上传操作必须使用post
		-->
		<form action="base1.html" method="get">
			<!--
				1:单行文本输入框
				type="text"
				name:表示key值
				可以随意书写
				（随意书写:
				不包含 空格 数字开头 中文 不要使用
				-可以使用_）
				value:用户填写的内容就是value值
				placeholder:文本框提示，html5新特性
			-->
			用户姓名:<input type="text" 
			name="myname" 
			placeholder="请输入用户姓名" />
			<br>
			<!--
				2:单行文本密码框
				type="password"
			-->
			用户密码:<input type="password" 
			name="mypass"
			placeholder="请输入用户密码" />
			<br>
			<!--
				3:提交按钮
				type="submit"
				注意按钮没有name属性
			-->
			<input type="submit" value="提交" />
			<!--
				4:取消按钮
				type="reset"
			-->
			<input type="reset" value="取消" />
		</form>
	</body>
</html>
 
```



#### frame标签 

```html
<!DOCTYPE html>
<html>
	<head>
		<title>4:小框架</title>
		<meta charset="utf-8" />
	</head>
	<body>
		<center>
			<table width="600px" height="800px">
				<tr>
					<td width="150px" height="100%">
						<ul>
							<li><a href="1.html"
								target="etoak">链接1</a></li>
							<li><a href="2.html"
								target="etoak">链接2</a></li>
							<li><a href="3.html"
								target="etoak">链接3</a></li>
						</ul>
					</td>
					<td width="450px" height="100%">
						<!--
							引入一个框架，此框架可以将
							一个页面引入到本页面，使之合并为一个页面
						-->
						<iframe src="main.html"
						name="etoak" width="100%"
						height="100%" 
						frameborder="0"></iframe>
					</td>
				</tr>				
			</table>
		</center>
	</body>
</html>
```



------

# XML

## 1.0简介xml

~XML~
~Extensible Markup Language~

1:历史
2:XML的格式
	1)头信息
	2)注释
	3)元素
	4)属性
	5)特殊字符
	6)CDATA格式
		
3:DTD (Document Type Definition 文档类型定义)
XML的语法规范
	1)内部DTD
		A:定义元素结构
		B:定义元素属性
	2)外部DTD
		A:本地方式
		B:网络方式

## XML 结构简介

- <![CDATA[]]>

```html
<?xml version="1.0" encoding="UTF-8"?>
<!--
	xml全文也只有一个根元素，头信息之前不能书写任何注释
-->
<学生信息>
	<student id="et001" name="李继坤">
		<家乡>武汉</家乡>
		<hobby>音乐</hobby>
		<hobby>小说</hobby>
		<hobby>电视</hobby>
		<!--
			xml文件中直接换行 空格都无法被正确识别
			字符串从左往右排列
		-->
		<info>
			20岁的
 
					来自武汉的
 

							李继坤非常喜欢看小说
							什么小说都看
					&gt;&lt;&quot;&apos;&amp;

		</info>
	</student>
	<student id="et002" name="王东">
		<家乡>济宁</家乡>
		<hobby>篮球</hobby>
		<hobby>小说</hobby>
		<info>
			<!--
				将字符串放置在cdata格式中
				换行 空格 特殊字符等都可以被正确识别
			
				select*fromuserwherename=**andpass=**salary>100
			-->
			<![CDATA[
				21岁的


				
				来自济宁的
 
							王东非常喜欢打篮球
					&gt;&lt;&quot;&apos;&amp;
					> < "" '' &
			]]>
		</info>
	</student>
</学生信息>


```

## 1.1 DTD约束

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!--
	内部DTD:将DTD文件直接书写在xml文件中
	DTD文件用来规范元素的结构和属性，可以使用哪些
	标签，这些标签出现的频率，嵌套的标签等一系列约束
	条件，违反了dtd的约束，xml无法被解析

	<!DOCTYPE 根元素[
		<!ELEMENT 根元素 (一级子元素)>
	]>

	*:表示此元素可以出现任意多次，也可以不出现
	元素名1,元素名2:此处的逗号表示元素出现的顺序
	必须按照前后显示，如果元素没有后面的标识符，则
	有且只能出现一次
	(元素1|元素2|元素N):此处得元素任选其一
	?:此元素有0个或者1个
	+:至少一个上不封顶
	~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	元素内部的值的类型:
	(#PCDATA):元素内部为任意字符串
	ANY:可以任意字符串也可以再是子元素
	(#PCDATA|元素名)*:元素内部可以是任意字符串，也可以是
	特定子元素，也可以两者同时存在
	EMPTY:元素内部没有任何内容或者子元素
-->
<!DOCTYPE 学生信息[
	<!ELEMENT 学生信息 (student*)>
	<!ELEMENT student (name,age,(location|home),hobby*,
	gf?,teacher+,job*,criminal)>
	<!ELEMENT name (#PCDATA)>
	<!ELEMENT age (#PCDATA)>
	<!ELEMENT location (#PCDATA)>
	<!ELEMENT home (#PCDATA)>
	<!ELEMENT hobby ANY>
	<!ELEMENT gf (#PCDATA)>
	<!ELEMENT teacher (#PCDATA)>
	<!ELEMENT job (#PCDATA|secondJob)*> 
	<!ELEMENT criminal EMPTY>
	<!ELEMENT secondJob (#PCDATA)>
]>

<学生信息>
	<student>
		<name>Damon</name>
		<age>30</age>
		<home>US</home>
		<hobby>敲代码</hobby>
		<hobby>旅行</hobby>
		<gf>Elena</gf>
		<teacher>Aleric</teacher>
		<job>
			软件工程师
			<secondJob>特工人员</secondJob>
		</job>
		<criminal></criminal>
	</student>
</学生信息>
 
```



## 1.2 结构DTD约束

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!--
	内部DTD:将DTD文件直接书写在xml文件中
	DTD文件用来规范元素的结构和属性，可以使用哪些
	标签，这些标签出现的频率，嵌套的标签等一系列约束
	条件，违反了dtd的约束，xml无法被解析

	<!DOCTYPE 根元素[
		<!ELEMENT 根元素 (一级子元素)>
	]>

	*:表示此元素可以出现任意多次，也可以不出现
	元素名1,元素名2:此处的逗号表示元素出现的顺序
	必须按照前后显示，如果元素没有后面的标识符，则
	有且只能出现一次
	(元素1|元素2|元素N):此处得元素任选其一
	?:此元素有0个或者1个
	+:至少一个上不封顶
	~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	元素内部的值的类型:
	(#PCDATA):元素内部为任意字符串
	ANY:可以任意字符串也可以再是子元素
	(#PCDATA|元素名)*:元素内部可以是任意字符串，也可以是
	特定子元素，也可以两者同时存在
	EMPTY:元素内部没有任何内容或者子元素
-->
<!DOCTYPE 学生信息[
	<!ELEMENT 学生信息 (student*)>
	<!ELEMENT student (name,age,(location|home),hobby*,
	gf?,teacher+,job*,criminal)>
	<!ELEMENT name (#PCDATA)>
	<!ELEMENT age (#PCDATA)>
	<!ELEMENT location (#PCDATA)>
	<!ELEMENT home (#PCDATA)>
	<!ELEMENT hobby ANY>
	<!ELEMENT gf (#PCDATA)>
	<!ELEMENT teacher (#PCDATA)>
	<!ELEMENT job (#PCDATA|secondJob)*> 
	<!ELEMENT criminal EMPTY>
	<!ELEMENT secondJob (#PCDATA)>
]>

<学生信息>
	<student>
		<name>Damon</name>
		<age>30</age>
		<home>US</home>
		<hobby>敲代码</hobby>
		<hobby>旅行</hobby>
		<gf>Elena</gf>
		<teacher>Aleric</teacher>
		<job>
			软件工程师
			<secondJob>特工人员</secondJob>
		</job>
		<criminal></criminal>
	</student>
</学生信息>
```

## 1.3 属性DTD约束

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE 学生信息[
	<!ELEMENT 学生信息 (student*)>
	<!ELEMENT student (name,age)>
		<!--
			<!ATTLIST 依附元素名 属性名 属性类型 默认值>
			属性类型:
				ID:全文唯一不能以数字开头
				CDATA:表示可以任意字符串
				(数据1|数据2|数据N):属性值只能从括号内任选其一
			~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
			默认值:
				#REQUIRED:表示默认值必须书写
				#IMPLIED:爱写不写
				具体数据:如果不书写默认值则默认使用此数据
				#FIXED:表示此属性值固定为后面的默认值
				存在浏览器差异性
		-->
		<!ATTLIST student id ID #REQUIRED>
		<!ATTLIST student home CDATA #IMPLIED>
		<!ATTLIST student salary (10k|20k|30k) "20k">
		<!ATTLIST student job CDATA #FIXED "软件工程师">
	<!ELEMENT name (#PCDATA)>
	<!ELEMENT age (#PCDATA)>
]>

<学生信息>
	<student id="et001" home="济南" salary="10k"
		job="软件工程师">
		<name>penny</name>
		<age>20</age>
	</student>
</学生信息>

```

# CSS

## 1.0 简介 	

- css的第一个宗旨是将页面的结构和样式解耦

```html
~Cascading Style Sheet~
~CSS~
~层叠样式表~

由网景公司在1996年发布，取代了html，专门用来渲染页面的样式，css文件以.css结尾，严格区分大小写，毫无容错性，之前渲染页面的结构和样式都是由html负责，这会导致页面非常负责，代码重用性低下，浏览器解析缓慢。

	
	html渲染样式

		<ins>
		<em>
		<font color="" face="" size="">
		<b>
		<h3>济南的夏天就要来到了</h3>
		</b>
		</font>
		</em>
		</ins>

	css渲染样式

		(选择器可以使用非常简练的代码从页面中选择元素)
		

		选择器{
			属性名:属性值;
			属性名:属性值;
			****
		}

		李继坤{
			年龄:20;
			性别:男;
			爱好:小说 电视 音乐;

		}

	
```

## 1.1CSS的引入

```html
<!DOCTYPE html>
<html>
	<head>
		<title>1:引入css的三种方式</title>
		<meta charset="utf-8" />
		<!--
			1:内嵌式
			直接将css代码书写在head标签内
			style:专门用来放置css代码进入此标签内
			语法改变为css语法
			type:MIME数据类型，此数据类型囊括了
			数十种后缀名，专门用来提示浏览器按照
			何种语法解析
			因为在h5技术中css和js已经被包含在h5技术
			序列中，则此处可以省略
			这种引入方式初步的将结构和样式解耦
			但是解耦不彻底，代码重用性不高
		-->
		<style type="text/css">
			/* 这是css的注释方式 */
			h3{
				/* 设置字体颜色 */
				color:orange;
				/* 设置字体 */
				font-family:方正静蕾简体;
				/* 设置字体大小 */
				font-size:70px;
			}
			label{
				background-color:pink;
			}
		</style>
		<!--
			2:外链式
				引入一个外部独立的css文件
				到本页面
				rel:表示引入的是一个样式表
				type:MIME可以省略
				href:表示独立css文件的引入路径
				这种引入方式彻底将结构和样式结构
				代码重用性较高
		-->
		<link rel="stylesheet" 
		type="text/css" href="css/style.css" />
	</head>
	<body>
		<h3>济南的夏天就要开始啦！！</h3>
		<!--
			#:当链接到的目的地还没有书写完成
			或者还没设置时，可以使用#作为占位符
		-->
		<a href="#">我是一个链接</a>
		
		<!--
			3:行内式
				直接将css代码书写在标签内
				这种方式将结构和样式再次耦合在一起
				，严重违背了css结构和样式解耦的原则
		-->
		<div style="background-color:lightblue;color:navy">我是一个层</div>

		<!--
			当多种引入方式出现冲突时优先级问题

			行内式> 内嵌式和外链式谁放在后面会覆盖之前
			出现冲突的css渲染样式
		-->
		<label>看看我听谁的？？</label>
	</body>
</html>
```

## 1.2 css基本选择器

```html
<!DOCTYPE html>
<html>
	<head>
		<title>2:基本选择器</title>
		<meta charset="utf-8" />
		<style>
			/*
				selector(选择器)
				在页面中使用简练的代码快速选择元素
			
				1:标记选择器
					tagName{
						直接使用标签名作为选择元素的
						依据，非常容易引起误操作
					}
			*/
			label{
				background-color:gold;
			}
			/*
				2:类别选择器;
				.className{
					直接使用.类名选择元素
				}
			*/
			.test2{
				background-color:gray;
			}
			/*
				3:id选择器;
				#idName{
					直接使用#id名作为选择元素的依据
				}
			*/
			#test3{
				background-color:maroon;
			}
		</style>
	</head>
	<body>
		<label>测试1</label>
		<label class="test2">测试2</label>
		<label id="test3" class="test2">测试3</label>
		<!--
			CSS层叠特性:
			基本选择器出现冲突时优先级问题
			id选择器>类别选择器>标记选择器
			与顺序无关
			这里如果添加上行内式，则以行内式为准
		-->
		<label id="test3" class="test2"
		style="background-color:purple">测试4</label>
		<!--
			在css中一般id选择器的属性值尽量在全文中唯一
			class没有这个限制，css中并没有这个语法限制
			但是一般页面在使用css后肯定会使用js
			js中存在以下方法
			document.getElementById("idName")
			根据id属性值拿取唯一一个元素，
			如果出现id属性名重复的情况，则有可能出现错误隐患
		-->
	</body>
</html>
```

## 1.3 css复合选择器

```html
<!DOCTYPE html>
<html>
	<head>
		<title>3:复合选择器</title>
		<meta charset="utf-8" />
		<style>
			/*
				1:交集选择器;
					tagName.className{
	
					}
					tagName#idName{
	
					}
				由一个标记后面紧跟一个类别或者id
				则必须满足其中两个条件才可以正确选取
			*/
			label.test2{
				font-style:italic;
			}
			/*
				2:并集选择器;
				sel1,sel2,sel3,selN{
	
				}
				多个基本或者复合选择器用逗号隔开
				只要满足其中任意一个就可以成功选取
			*/
			span.test2,label.test2,h2,h3,h5{
				background-color:yellow;
			}

			/*
				CSS继承特性:
				在没有任何冲突的情况下，css中子元素
				会完全继承父元素的所有css渲染样式效果;
			
				3:后代选择器;
					根据左祖先右后代的原则，用空格隔开
					具有家族关系的元素，可以精确选取
					具有特定祖先元素的子元素
					sel1 sel2 sel3 selN{
	
					}
			*/
			div#outter label.inner{
				background-color:silver;
			}
			/*
				4:全选选择器;
				*{
	
				}
			*/
			*{
				font-weight:bolder;
			}


		</style>
	</head>
	<body>
		<label>测试1</label>
		<span class="test2">测试2</span>
		<label class="test2">测试3</label>
		<h2>我是二级标题</h2>
		<h3>我是三级标题</h3>
		<h5>我是五级标题</h5>
		<div id="outter">济南的<label class="inner">夏天</label>就要开始啦~~~</div>
	</body>
</html>
 
```



##   1.4  css中display

```html
<!DOCTYPE html>
<html>
	<head>
		<title>4:页面元素类型</title>
		<meta charset="utf-8" />
		<style>
			label{
				background-color:lightblue;
				width:300px;
				height:200px;
				/* 居中对齐，类似html中的center标签 */
				text-align:center;
			}
			div{
				background-color:pink;
				width:300px;
				height:200px;
				/* 居中对齐，类似html中的center标签 */
				text-align:center;
			}
		</style>
	</head>
	<body>
		<!--
			在css中将元素分成了多种类型
			比较典型的有以下几种
			1):块元素（block）
			eg:div p h1~h6 ul li img*
			以上元素都是块元素，这种元素自带换行，一行只能书写一个，设置对齐方式有效，设置盒子模型有效
			2):内联元素（inline）
			eg:label span a img*
			以上元素都是内联元素，这种元素没有自带换行，一行可以存在多个，从左往右排列，设置对齐方式 盒子模型等参数一律无效
			*img:（inline-block）内联块元素
			本身属于内联元素，但是具备了块元素的所有特征
			3):空元素（empty）
			一般不嵌套内容，由一个元素表示特定的结构或者
			样式
			br meta hr都属于空元素

		-->
		<label style="display:block">我是label我是label我是label我是label我是label我是label我是label我是label我是label我是label我是label我是label我是label</label>
		<hr>
		<div style="display:inline">我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div</div>
		<div style="display:none">我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div</div>
	</body>
</html>
```

- display详解

```text
CSS规定，每一个网页元素都有一个display属性，用于确定该元素的类型，每一个元素都有默认的display属性值，比如div元素，它的默认display属性值为“block”，成为“块级”元素(block-level)；而span元素的默认display属性值为“inline”，称为“行内”元素。

 块级元素：
      动占据一定矩形空间，可以通过设置高度、宽度、内外边距等属性，来调整的这个矩形的样子；

 行内元素：

      自己的独立空间，它是依附于其他块级元素存在的，因此，对行内元素设置高度、宽度、内外边距等属性，都是无效的。

 

例子：
      链接 a 元素，在默认情况下是是“行内元素”，因此对a元素设置高度、宽度等属性，都是无效的。而比如div，p，li，img等默认情况下的display属性值就是“block”。所以对于链接a来说只能这样：display:block;强制将它改成块元素。

```

##  1.5 盒子模型

```html
<!DOCTYPE html>
<html>
	<head>
		<title>5:盒子模型</title>
		<meta charset="utf-8" />
		<style>
			/*
				在css中，所有的元素都具有四个边框
				类似一个一个的盒子摆放在页面上，通过border属性可以将元素的边框显示出来
			*/
			div#container{
				/*
					border:边框类型 边框粗细 边框颜色;
					类型:solid表示单实线;
					double双实线
					dotted点状线
				*/
				border:solid 2px red;
				/* 
					页面的整体居中
				*/
				margin:0 auto;
				width:600px;
				height:800px;
			}
			h2{
				/*border:solid 2px blue;*/
				/*margin-top:100px;
				margin-left:70px;
				margin-bottom:60px;
				margin-right:120px;

				四个参数表示四个方位
				margin:上 右 下 左;
				三个参数表示 上 （右左） 下
				其中括号内的参数相同
				两个参数表示 （上下） （右左）
				一个参数表示 （上右下左）
				*/
				margin:100px 120px 60px 70px;
				padding-top:60px;
				padding-left:20px;
				padding-bottom:100px; 

				border-top:double 10px black;
				border-right:dotted 12px red;
				border-bottom:dotted 10px navy;
				border-left:double 5px aqua;


			}
			ul{
				border:solid 2px orange;
			}
			li{
				border:solid 2px lightblue;
			}
			p{
				border:solid 2px pink;
			}

			*{
				margin:0;
				padding:0;
			}


		</style>
	</head>
	<body>
		<div id="container">
			<h2>我是二级标题</h2>
			<ul>
				<li>列表1</li>
				<li>列表2</li>
				<li>列表3</li>
			</ul>
			<p>我是一个段落</p>
		</div>
	</body>
</html>
```

## 1.6 float元素浮动

```html
<!DOCTYPE html>
<html>
	<head>
		<title>6:浮动</title>
		<meta charset="utf-8" />
		<style>
			ul{
				/* 去掉列表的徽记 */
				list-style-type:none;
			}

			ul li{
				/* 浮动：
					重新排列元素得顺序，
					left:从左往右排列
					right:相反;
				*/
				float:left;
				margin-right:20px;
				margin-top:80px;

			}
			body{
				/*
					设置背景图
				*/
				background-image:url("image/background.gif");
				/* 只允许背景图横向排列 */
				background-repeat:repeat-x;
				background-color:#cccc99;
			} 
		</style>
	</head>
	<body>
		<ul>
			<li><img src="image/1.jpg"></li>
			<li><img src="image/2.jpg"></li>
			<li><img src="image/3.jpg"></li>
			<li><img src="image/4.jpg"></li>
		</ul>
	</body>
</html>








```

##  1.7元素的定位

```html
<!DOCTYPE html>
<html>
	<head>
		<title>7:定位</title>
		<meta charset="utf-8" />
		<style>
			div#container{
				border:solid 2px red;
				/*position:relative;*/
				margin:0 auto;
				width:600px;
				height:800px;
			}
			div#sub1{
				border:solid 2px orange;
			}
			div#sub2{
				border:solid 2px yellow;
				/*
					相对定位
					position:relative;
					根据元素原先所在位置的左上角进行定位，定位之后元素依然保持原先的类型，原来的位置依然被占用
					根据偏移量来进行新的定位
					top和bottom只能书写一个
					left和right只能书写一个
				*/
				position:relative;
				/* 向下偏移三百像素 */
				top:300px;
				/* 向右偏移二百像素 */
				left:200px;
			}
			div#sub3{
				border:solid 2px green;
			}
			div#sub4{
				/*
					绝对定位
					根据距离元素最近的定位过的父元素的
					左上角进行定位，定位之后元素不再占满一行，原先的位置被占用
					如果没有定位过的祖先元素，则
					根据body来定位，但是这样可能会出现问题
				*/
				position:absolute;
				border:solid 2px aqua;
				top:300px;
				left:200px;
			}
			div#sub5{
				border:solid 2px blue;
				position:absolute;
				top:400px;
				left:300px;
			}
			div#sub6{
				border:solid 2px purple;
			}
		</style>
	</head>
	<body>
		<div id="container">
			<div id="sub1">我是层1</div>
			<div id="sub2">我是层2</div>
			<div id="sub3">我是层3</div>
			<div id="sub4">我是层4</div>
			<div id="sub5">
				<img src="image/logos.jpg" />
			</div>
			<div id="sub6">我是层6</div>
		</div>
	</body>
</html>
```

###  定位详解

```
position的四个属性值：

1.relative
2.absolute
3.fixed
4.static
下面分别讲述这四个属性。

<div id="parent">
     <div id="sub1">sub1</id>
     <div id="sub2">sub2</id>
</div>


1. relative

relative属性相对比较简单，我们要搞清它是相对哪个对象来进行偏移的。答案是它本身的位置。在上面的代码中，sub1和sub2是同级关系，如果设定sub1一个relative属性，比如设置如下CSS代码：

#sub1
{
    position: relative;
    padding: 5px;
    top: 5px;
    left: 5px;
}


我们可以这样理解，如果不设置relative属性，sub1的位置按照正常的文档流，它应该处于某个位置。但当设置sub1为的position为relative后，将根据top，right，bottom，left的值按照它理应所在的位置进行偏移，relative的“相对的”意思也正体现于此。

对于此，您只需要记住，sub1如果不设置relative时它应该在哪里，一旦设置后就按照它理应在的位置进行偏移。

随后的问题是，sub2的位置又在哪里呢？答案是它原来在哪里，现在就在哪里，它的位置不会因为sub1增加了position的属性而发生改变。

如果此时把sub2的position也设置为relative，会发生什么现象？此时依然和sub1一样，按照它原来应有的位置进行偏移。

注意relative的偏移是基于对象的margin的左上侧的。

2. absolute

这个属性总是有人给出误导。说当position属性设为absolute后，总是按照浏览器窗口来进行定位的，这其实是错误的。实际上，这是fixed属性的特点。

当sub1的position设置为absolute后，其到底以谁为对象进行偏移呢？这里分为两种情况：

（1）当sub1的父对象(或曾祖父，只要是父级对象)parent也设置了position属性，且position的属性值为absolute或者relative时，也就是说，不是默认值的情况，此时sub1按照这个parent来进行定位。

注意，对象虽然确定好了，但有些细节需要您的注意，那就是我们到底以parent的哪个定位点来进行定位呢？如果parent设定了margin，border，padding等属性，那么这个定位点将忽略padding，将会从padding开始的地方(即只从padding的左上角开始)进行定位，这与我们会想当然的以为会以margin的左上端开始定位的想法是不同的。

接下来的问题是，sub2的位置到哪里去了呢？由于当position设置为absolute后，会导致sub1溢出正常的文档流，就像它不属于 parent一样，它漂浮了起来，在DreamWeaver中把它称为“层”，其实意思是一样的。此时sub2将获得sub1的位置，它的文档流不再基于 sub1，而是直接从parent开始。

（2）如果sub1不存在一个有着position属性的父对象，那么那就会以body为定位对象，按照浏览器的窗口进行定位，这个比较容易理解。

3. fixed

fixed是特殊的absolute，即fixed总是以body为定位对象的，按照浏览器的窗口进行定位。

4. static

position的默认值，一般不设置position属性时，会按照正常的文档流进行排列。
 
```

