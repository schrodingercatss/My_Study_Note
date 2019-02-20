## 使用div和table布局的实例

### div布局

下述是一个`div`布局的实例，使用id为`container`的盒子作为整个内容容器，然后使用百分比来定制布局。

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title></title>
		<style type="text/css">
			body {
				margin: 0px;
				padding: 0px;
			}
			div#container {
				width: 100%;
				height: 950px;
				background-color: darkgray;
			}
			div#heading {
				width: 100%;
				height: 10%;
				background-color: aqua;
			}
			div#content_menu {
				width: 30%;
				height: 80%;
				background-color: aquamarine;
				float: left;
			}
			div#content_body {
				width: 70%;
				height: 80%;
				background-color: blueviolet;
				float: left;
			}
			div#footing {
				width: 100%;
				height: 10%;
				background-color: red;
				clear:both;
			}
		</style>
</head>
<body>
	<div id="container">
		<div id="heading">头部</div>
		<div id="content_menu">内容菜单</div>
		<div id="content_body">内容主体</div>
		<div id="footing">底部</div>
	</div>
	</body>
</html>

```

***

#### table布局

使用每一块作为一个单元格，巧妙使用`colspan`跨列来达到效果，仍然使用百分比来分配页面内容。

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>table布局</title>
		<style type="text/css">
			body {
				margin: 0;
				padding: 0;
			}
		</style>
	</head>
	<body>
		<table width="100%" height="950px" style="background-color: darkgray;">
			<tr>
				<td colspan="2" width="100%" height="10%" style="background-color: aqua;">这是头部</td>
			</tr>
			<tr>
				<td width="30%" height="80%" style="background-color: blue;">左菜单</td>
				<td width="70%" height="80%" style="background-color: purple;">右菜单</td>
			</tr>
			<tr>
				<td width="100%" height="10%" style="background-color: red;" colspan="2">底部</td>
			</tr>
		</table>
	</body>
</html>

```

