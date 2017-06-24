HTML5和HTML5篇
------
# 本章问题汇总：

> 1.说说你对语义化的理解？
>
> 2.Doctype作用?严格模式与混杂模式如何区分？它们有何意义?
>
> 3.你知道多少种Doctype文档类型？
>
> 4.HTML与XHTML二者有什么区别？
>
> 5.html5有哪些新特性、移除了那些元素？如何处理HTML5新标签的浏览器兼容问题？
>
> 6.iframe的优缺点？
>
> 7.如何实现HTML语言的首尾共用？
>
> 8.什么是 FOUC（无样式内容闪烁）？你如何来避免FOUC？

## 1.说说你对语义化的理解？

    1.去掉或者丢失样式的时候能够让页面呈现出清晰的结构；
    
    2.有利于SEO：和搜索引擎建立良好沟通，有助于爬虫抓取更多的有效信息(爬虫依赖于标签来确定上下文和各个关键字的权重)；
    
    3.方便其他设备解析（如屏幕阅读器、盲人阅读器、移动设备；
    
    4.便于团队开发和维护，可以减少差异化，语义化更具可读性。
    
## 2.Doctype作用? 严格模式与混杂模式如何区分？它们有何意义?    

    1.<!DOCTYPE> 声明位于文档中的最前面，处于<html>标签之前，告知浏览器以何种模式来渲染文档。 
    
    2.严格模式的排版和JS运作模式是:以该浏览器支持的最高标准运行。
    
    3.在混杂模式中，页面以宽松的向后兼容的方式显示,模拟老式浏览器的行为以防止站点无法工作。
    
    4.DOCTYPE不存在或格式不正确会导致文档以混杂模式呈现。
    
    5.HTML5 没有 DTD ，因此也就没有严格模式与混杂模式的区别，HTML5 有相对宽松的语法，实现时，已经尽可能大的实现了向后兼容。（ HTML5 没有严格和混杂之分）

## 3.你知道多少种Doctype文档类型？

    DOCTYPE标签可声明三种 DTD 类型，分别表示严格版本、过渡版本以及基于框架的 HTML 文档。
    
    HTML5规定了一种文档类型：<!DOCTYPE html>
    HTML 4.01 规定了三种文档类型：Strict、Transitional 以及 Frameset。
    XHTML 1.0 规定了三种XML文档类型：Strict、Transitional、Frameset以及XHTML 1.1 该 DTD 等同于 XHTML 1.0 Strict，但允许添加模型。
    Standards(标准)模式(也就是严格呈现模式)用于呈现遵循最新标准的网页，而Quirks(包容)模式(也就是松散呈现模式或者兼容模式)用于呈现为传统浏览器而设计的网页。

## 4.HTML与XHTML二者有什么区别？

    区别：
    XHTML 元素必须被正确地嵌套：标签不能混淆位置。
    XHTML 元素必须被关闭：非空标签必须使用结束标签。
    XHTML 标签名必须用小写字母。
    XHTML 文档必须拥有根元素：所有的 XHTML 元素必须被嵌套于 <html> 根元素中。其余所有的元素均可有子元素。子元素必须是成对的且被嵌套在其父元素之中。
    XHTML 用 id 属性代替 name 属性。
    XHTML 所有属性都必须使用双引号。
    XHTML 不允许使用target="_blank"。
    
## 5.html5有哪些新特性、移除了那些元素？如何处理HTML5新标签的浏览器兼容问题？

- H5新特性：


    1.拖拽释放(Drag and drop):拖放是一种常见的特性，即抓取对象以后拖到另一个位置;
    
    2.语义化更好的内容标签（header,nav,footer,aside,article,section,footer,nav）;
    
    3.音频、视频(audio,video);
    
    4.画布(Canvas);
    
    5.Geolocation（地理定位）用于定位用户的位置;
    
    6.支持内联 SVG（可伸缩矢量图形 Scalable Vector Graphics）;
    
    7.HTML5 提供了两种在客户端存储数据的新方法：
        localStorage - 没有时间限制的数据存储；
        sessionStorage - 针对一个 session 的数据存储；
    (之前，这些都是由cookie完成的。但是cookie不适合大量数据的存储，因为它们由每个对服务器的请求来传递，这使得 cookie 速度很慢而且效率也不高。)
    
    8.应用程序缓存（Application Cache）,通过创建 cache manifest 文件，可以轻松地创建 web 应用的离线版本;
    
    9.Web Worker,是运行在后台的JavaScript，独立于其他脚本，不会影响页面的性能。您可以继续做任何愿意做的事情：点击、选取内容等等，而此时web worker在后台运行;
    
    10.服务器发送事件（server-sent event）允许网页获得来自服务器的更新;
    
    11.新的表单控件，calendar、date、time、email、url、search等;
    
    12.Websocket,WebSocket是HTML5开始提供的一种在单个TCP连接上进行全双工通讯的协议。

- H5移除的元素：


    纯表现的元素：basefont，big，center，font, s，strike，tt，u。
    
    不再使用frame框架：frame，frameset，noframes。只支持iframe框架，或者用服务器方创建的由多个页面组成的符合页面的形式。
    
- 处理HTML5新标签的浏览器兼容问题：
    

    IE8/IE7/IE6支持通过document.createElement方法产生的标签，
    可以利用这一特性让这些浏览器支持HTML5新标签，浏览器支持新标签后，还需要添加标签默认的样式：
    
    当然最好的方式是直接使用成熟的框架、使用最多的是html5shim框架
       <!--[if lt IE 9]> 
            <script> src="http://html5shim.googlecode.com/svn/trunk/html5.js"</script> 
       <![endif]--> 

## 6.iframe的优缺点？

`<iframe>`优点：
    
    iframe的优点：
    
    1.iframe能够原封不动的把嵌入的网页展现出来
    
    2.如果有多个网页引用iframe，那么你只需要修改iframe的内容，就可以实现调用的每一个页面内容的更改，方便快捷
    
    3.网页如果为了统一风格，头部和版本都是一样的，就可以写成一个页面，用iframe来嵌套，可以增加代码的可重用
    
    4.如果遇到加载缓慢的第三方内容如图标和广告，这些问题可以由iframe来解决
        
`<iframe>`的缺点：
   
    1.会产生很多页面，不容易管理
    
    2.框架个数多的话，可能会出现上下左右滚动条，会分散访问者的注意力，用户体验度差
    
    3.代码复杂，无法被一些搜索引擎索引到，这一点很关键，现在的搜索引擎爬虫还不能很好的处理iframe中的内容，所以使用iframe会不利于搜索引擎优化
    
    4.很多的移动设备（PDA 手机）无法完全显示框架，设备兼容性差
    
    5.iframe框架页面会增加服务器的http请求，对于大型网站是不可取的
    
    6.不容易打印（目前只能实现分框架页面的打印，不能实现对frameset的打印）
    
    7.浏览器的后退按钮无效

## 7.如何实现HTML语言的首尾共用？


1.使用iframe标签引入头部尾部。

    首部：
    <iframe MARGINWIDTH=0 MARGINHEIGHT=0 HSPACE=0 VSPACE=0 FRAMEBORDER=0 SCROLLING=no src=”head.htm” height=“auto” width="100%"></iframe>
    尾部：
    <iframe MARGINWIDTH=0 MARGINHEIGHT=0 HSPACE=0 VSPACE=0 FRAMEBORDER=0 SCROLLING=no src=”foot.htm” height="auto" width="100%"></iframe>

2.制作一个共用头部文件head.js和一个共用底部文件foot.js， <script src="head.js"> 和 <script src="foot.js"> 调用共同的网页头部或者网页底部。根据html利用转换工具：http://tool.chinaz.com/Tools/Html_Js.aspx

3.利用ajax动态拉取填充：

    例1：
        $(function() {
            $("#header").load("head.html");
        })
    
    
    $(function() {
		$.ajax({
			type: "GET",
			url: "head.html",
			dataType: "html"
		});
	})
	
	// 谷歌、IE有ajax本地跨域问题，火狐可以实现
	
    例2：
    // 修改为JS文件后，导入JS文件
    $(function() {
		$.ajax({
			type: "GET",
			url: "head.js",
			dataType: "script"
		});
	})

4.web服务器中设定包含

    比如使用ssi技术页面生成shtml文件，只用在头部文件位置加入<#include file="header.htm">，然后修改的时候只要修改header.htm文件就可以了。
    使用shtml的好处是对搜索引擎比较友好，需要处理的文件在服务器端完成的， 不会加重访问者的浏览器负担。

5.后台模板引擎处理：比如jade模版引擎

6.angular js里的<ng-include>的使用

## 8.什么是 FOUC（无样式内容闪烁）？你如何来避免FOUC？

什么是FOUC(Flash Of Unstyled Content 文档样式闪烁)？
> 如果使用import方法对CSS进行导入,会导致某些页面在Windows 下的IE出现一些奇怪的现象:以无样式显示页面内容的瞬间闪烁,这种现象称之为文档样式短暂失效(Flash of Unstyled Content),简称为FOUC。


原因大致为：

    1.使用import方法导入样式表。
    2.将样式表放在页面底部
    3.有几个样式表，放在html结构的不同位置。

其实原理很清楚：当样式表晚于结构性html加载，当加载到此样式表时，页面将停止之前的渲染。此样式表被下载和解析后，将重新渲染页面，也就出现了短暂的花屏现象。`解决方法：使用LINK标签将样式表放在文档HEAD中`。
     
