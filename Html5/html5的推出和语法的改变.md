## html5的推出和语法的改变

***

### 0x00：什么是XHTML?

XHTML指的是可扩展超文本标记语言

XHTML与HTML4.01几乎是相同的

XHTML是更严格更纯净的HTML版本

XHTML是以XML应用的方式定义的HTML

XHTML得到所有主流浏览器的支持

***

### 0x01：为什么使用XHTML?

为了代码的完整性和良好性。

***

### 0x02：文档声明的定义

DTD：规定了使用通用标记语言的网页语法，分为：

- STRICT(严格类型)
- TRANSITIONAL(过渡类型)
- FRAMESET(框架类型)

***

### 0x03：XHTML属性语法规则

XHTML属性必须使用小写

XHTML属性必须用引号包围

XHTML属性最小化也是禁止的(也就是把完整标签写成缩写，比如body写成bd)

***

### 0x04：html5的推出理由、目标和语法的改变

html5的推出的意图是想要把Web上存在的问题一并解决掉：

- Web浏览器之间兼容性很低
- 文档结构不够明确
- Web应用程序的功能受到了限制

**语法的改变：**

- 内容类型(与之前相比基本不变)
- DOCTYPE声明
- 指定字符编码(不需要写一堆属性了，直接用meta标签设置即可)
- 可以省略标记的元素(如checked，不写表示false，写了并指定值表示true，checked = "checked"可以省略写成checked)
- 具有boolean值的属性(如checked等)
- 省略引号(属性值可以省略掉引号)

***

### 0x05：新增的元素和废除的元素

**新增元素：**

- 新增的结构元素：
  - 如section、article、aside、header、footer、nav等等
- 新增的其他元素：
  - 如video、audio、time、canvas等等
- 新增的input元素的类型：
  - 如email、url、number、range、Date Pickers



**废除元素：**

- 能用CSS替代的元素：basefont、big、center、font、s、tt、u等
- 不再使用frame框架，但使用iframe框架
- 只有部分浏览器支持的元素
- 其他被废除的元素



**新增属性：**

- 表单相关的属性
- 链接相关的属性
- 其他属性

**废除属性：**

- 比较多，请具体见官方文档。

***

### 0x06：全局属性

**全局属性：**可以对任何元素使用的属性

- contentEditable属性：值为true时表示允许用户编辑该元素，所以使用该属性的元素必须能获得焦点。该属性还有一个隐藏继承状态，如果子元素未指定该属性，则是否可编辑由其父元素决定。
- designMode属性：指定页面是否可编辑，有`on`和`off`两种取值，如果值为on，则所有含contentEditable属性的元素值都变为true。该属性只能在javascript中编辑和修改，不可直接指定。
- hidden属性：指定后浏览器不再渲染该元素，实现隐藏效果，可以用javascript取消该效果。
- spellcheck属性：给元素开启输入拼写检查。
- tabindex属性：能可以设置键盘中的TAB键在元素中的移动顺序,即焦点的顺序，默认情况下，只有链接和表单元素可以使用tab键遍历，其他元素只能指定tabindex属性。

```html
<body>
    <!--下面的元素按tab键的遍历顺序为1，3， 2-->
    <a href="#" tabindex="1">hello</a>
    <a href="#" tabindex="3">hello</a>
    <a href="#" tabindex="2">hello</a>
    
    <!--tabindex属性默认为0，将排在所有元素之后-->
     <a href="#" tabindex>hello</a>
    <!--下面值为-1的表示不参与tab键的遍历，但可以使用js脚本修改-->
    <a href="#" tabindex="-1">hello</a>
</body>
```

***









