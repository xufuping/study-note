# 一、一些概念性的要点

1、HTML元素负责文档内容的结构和含义，内容的呈现则由CSS样式控制。

# 二、创建HTML文档

## 1、base——设置相对URL的解析基准  例如：



```
<!DOCTYPE html>
<html>
<head>
  	<meta charset="utf-8">
 	 <title>345345345345</title>
  	<base href="http://xufuping">
</head>
<body>
  I like <code>apples</code> and oranges.
 	 <a href="www.baidu.com">百度</a>
 	 <a href="新的页面.html">新的页面</a>
</body>
</html>
```



效果：点击a标签后触发链接为http://xufuping/www.baidu.com或者是xufuping/新的页面.html，如果不用base元素，不设置一个基准URL，浏览器会将当前文档的URL认定为所有相对URL的解析基准。

## 2、meta作用

2.1指定名/值元数据对——略

2.2声明字符编码  例如：

```
<meta charset = "utf-8"/>
```

2.3模拟HTTP标头字段，例如：

```
<meta http-equiv = "refresh" content = "5">//每隔5秒就再次载入页面
```

属性值：
refresh	以秒为单位指定时间刷新页面重新载入
default-style	指定页面优先使用的样式表
content-type	另一种声明字符编码的方法。<meta http-equiv = "content-type" content="text/html charset=UTF-8"/>

## 3、定义CSS样式

3.1style元素中的media属性值，可以将CSS样式适用于规定设备。例如：

```
<style media = "screen" type = "text/css">……</style>//适用于计算机显示器屏幕
<style media ="print">……</style>//适用于打印预览和打印页面时
```

3.2media属性值也可以设定在不同大小浏览器窗口显示不同的样式，例如：


```
<style media = "screen AND (max-width:500px)" type = "text/css">
……
</style>
<style media = "screen AND (min-width:500px)" type = "text/css">
……
</style>
```


这段代码使用media的width特性区分两组样式。浏览器窗口宽度小于500像素时使用的是第一组样式，川口宽度大于500像素时使用的是第二组。用浏览器打开这个HTML文档，然后拖拉窗口不hi安源用来改变起大小，就能看到这个特性的效果。

3.3 link元素可用来HTML文档和外部资源之间建立联系,例如（载入样式表）：

```
<link rel="stylesheet" type="text/css" href="style.css"/>
```
3.4 link还可以用来为页面定义网站标志，改变浏览器左上角的图标


```
<link rel= "shortcut icon" href = "favicon.ico" type="image/x-icon" />
//这段代码为网站添加了一个浏览器链接栏Logo(图片文件名为：favicon.ico)
```

## 4、使用脚本元素<script>的一些知识点

载入脚本使用的代码：<script src="123.js"></script>

使用async异步执行脚本（打乱次序执行脚本），使用这个属性的一个重要后果是页面中的脚本可能不再按定义他们的次序执行，因此如果脚本使用了其他脚本中定义的函数或值，就不要再使用这个属性了。

noscript元素，可以用来向仅用了JavaScript或浏览器不支持JavaScript的用户显示一些内容，例如：
```
<noscript>
    <h1>Javascript is required!</h1>
    <p>You cannot use this page without Javascript</p>
</noscript>
```
或者在浏览器不支持JS时将其引致另一个URL，这需要在noscript元素中加入一个meta元素。例如：

```
<noscript>
    <meta http-equiv = "refresh" content="0;http://www.apple.com"/>
</noscript>
```
# 三、标记文字

## 1、生成内部超链接

```
<a href="#fruits">到达水果站</a>
<p id="fruits">这里是水果站：apples,bananas,mangoes</p>
```
# 四、一些可考虑的新标签

**header**元素表示一节的首部，里面可以包含各种适合出现在首部的东西。

**footer**表示一节的尾部，通常包含着该节的总结信息，还可以包含做着介绍、版权信息、到相关内容的链接、微标以及免责声明等。

**nav**（添加导航区域）元素表示文档中的一个区域，它包含着到其他页面或同一页面的其它部分的链接。可以用来写导航。

**article**元素代表HTML文档中一段独立成片的内容。

**aside**元素用来表示跟周边内容稍沾一点边的内容，类似于书籍或杂志中的侧栏。

**details**元素在文档中剩一个区域，用户可以展开它来了解关于某主题的更多详情。details元素通常包含一个sunmary元素，后者的作用是为该详情区域生成一个说明标签或标题。例如：

```
<details>
    <summary>点击我可以展开或许到下面p标签隐藏的内容</summary>
    <p>我是被隐藏的内容，哈哈哈哈哈哈，嘻嘻嘻嘻嘻嘻</p>
</details>
```
