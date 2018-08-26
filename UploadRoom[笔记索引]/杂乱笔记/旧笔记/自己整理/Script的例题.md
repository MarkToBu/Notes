# script的案例

## [1] 切换照片

### [1] 案例

#### [1] Html和Script代码

```
<!DOCTYPE html>
<html>
	<head>
		<title>7：简易相册</title>
		<meta charset="utf-8">
		<link rel="stylesheet" type="text/css" href="css/mystyle.css">
	</head>
	<body>
		<div id="container">
			<table>
				<caption><img src="image/logo.png"></caption>
				<tr>
					<td><input type="image" src="image/left.gif" onclick="change('left')"></td>
					<td><img src="image/1.jpg" id="mypic"></td>
					<td><input type="image" src="image/right.gif" onclick="change('right')"></td>
				</tr>
			</table>
		</div>
		<script type="text/javascript">
			var count = 1;
			function change(value){
				if(value == 'left'){
					if(count>1){
						count--;
					}else{
						alert("已经是第一幅图");
					}
						
				}
				if(value == 'right'){
					if(count<6){
						count++;
					}else{
						alert("已经是最后一幅图");
					}
				}
				var dom_ig =document.getElementById("mypic");
				dom_ig.src="image/"+count+".jpg";
			}
		</script>
	</body>
</html>
```

#### [2] 样式代码

```
body{
	background-color:black;
}
div#container{
	margin:0 auto;
	width:830px;
	height:700px;
}
```

## [2] 