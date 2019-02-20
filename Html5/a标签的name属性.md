## a标签的name属性

我们可以使用a标签的name属性来实现页面内的跳转，百度百科就是里面就是大量运用的这种方法。

下面是一个实例：

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title></title>
	</head>
	<body>
		<a name="tips">测试</a>
		<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
		<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
		<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
		<a href="#tips">跳转</a>
	</body>
</html>
```

