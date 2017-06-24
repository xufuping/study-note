# 1.JavaScript简介
## 1.1 JS实现

一个完整的JS实现应该由下列三个不同的部分组成：==核心(ECMAScript)==、==文档对象模型(DOM)==、==浏览器对象模型(BOM)==.
    
### 1.1.1 核心(ECMAScript)

ECMAScript，由ECMA-262定义，提供核心语言功能。

ECMAScript与Web浏览器没有依赖关系，Web浏览器只是ECMAScript可能的宿主环境之一。

### 1.1.2 文档对象模型(DOM)

DOM主要负责提供访问和操作网页内容的方法和接口。

DOM把整个页面映射为一个多层节点结构(表示文档的树形图)，借助DOM提供的API，开发人员可以轻松自如地删除、添加、替换或修改任何节点。

#### 1.1.2.1 DOM级别

##### 1.1.2.1.1 DOM1级别：
DOM1级别由两个模块组成：DOM核心和DOM HTML.

    DOM核心规定的是如何映射基于XML的文档结构，以便简化对文档中任意部分的访问和操作。
    DOM HTML模块则在DOM核心的基础上加以扩展，添加了针对HTML的对象和方法。

##### 1.1.2.1.2 DOM2级别：
    
DOM2在DOM的基础上又扩充了(DHTML一直都支持的)鼠标和用户界面事件、范围、遍历(迭代DOM文档的方法)等细分模块，而且通过对象接口增加了对CSS的支持。
    
DOM2级引入了下列新模块，也给出了众多新类型和新接口的定义：

    DOM视图：定义了跟踪不同文档（例如，应用CSS之前和之后的文档）视图的接口；
    DOM事件：定义了事件和事件处理的接口；
    DOM样式：定义了基于CSS为元素应用样式的接口；
    DOM遍历和范围：定义了遍历和操作文档树的接口。

##### 1.1.2.1.3 DOM3级别：

进一步扩展了DOM，引入了以统一方式加载和保存文档的方法——在DOM加载和保存模块中定义；新增了验证文档的方法——在DOM验证模块中定义。同时扩展了DOM核心，开始支持XML 1.0规范，涉及 XML Infoset、XPath 和XML Base.

（DOM并不只是针对JavaScript的，很多别的语言也都实现了DOM。）

### 1.1.3 浏览器对象模型(BOM)

BOM提供与浏览器交互的方法和接口，可以访问和操作浏览器窗口。

从根本上讲，BOM只处理浏览器窗口和框架；但人们习惯上也把所有针对浏览器的JavaScript扩展算作BOM的一部分，下面就是一些扩展：
    
    弹出新浏览器窗口的功能；
    移动、缩放、关闭浏览器窗口的功能；
    提供浏览器详细信息的navigator对象；
    提供浏览器所加载页面的详细信息的location对象；
    提供用户显示器分辨率详细信息的screen对象；
    对cookies的支持；
    像XMLHttpRequest和IE 的 ActiveXObject 这样的自定义对象。
（HTML5致力于把很多BOM功能写入正式规范）
# 2.在HTML中使用JavaScript
## 2.1 <script>元素
六个属性
    
    async       【异步脚本】可选。表示立即下载脚本，只对外部脚本文件有效。(标记为async的脚本并不保证按照指定它们的先后顺序执行。)
    //使用：直接写入async，在XHTML文档中写入 async="async"
    charset     可选。表示通过src指定的代码的字符集。（被大多数浏览器忽略，少有人用）
    defer       【延迟脚本】可选。表示脚本延迟到文档完全被解析和显示之后再执行(但不影响下载次序)。只对外部脚本文件有效。
    // 使用：直接写入 defer="defer"
    language    已废弃。
    src         可选。导入外部脚本文件。
    type        可选。表示编写代码用的脚本语言的内容类型(MIME类型)，一般值为"text/javascript".
    //  只要不出现defer和async属性，浏览器都会按照<script>元素在页面中出现的先后顺序对它们依次进行解析。

### 2.1.1 标签的位置

放在<head>，必须等到全部JavaScript代码被下载解析执行后才能开始呈现页面的内容。这很有可能导致浏览器在呈现页面时出现明显的延迟，造成不好的用户体验。

所以，为了避免这个问题，现代Web应用程序一般把JavaScript引用放在<body>元素中，放在页面的内容后面，加强用户体验。

2.1.2 XHTML严格模式

XHTML结合了XML部分的强大功能以及HTML大多数的简单特性，XHTML要求更加严谨，相对HTML的几大区别：

    XHTML 要求争取额嵌套；
    XHTML 所有元素必须关闭；
    XHTML 区分大小写；
    XHTML 属性值要用双引号；
    XHTML 用id属性代替name属性；
    XHTML 特殊字符的处理。

## 2.2 嵌入代码与外部文件

外部JS的优势：

    可维护性：专注编辑JS
    可缓存：同代码多样使用
    适应未来：外部JS无序使用XHTML或注释hack，HTML和XHTML包含外部文件的语法是相同的。

## 2.3 文档模式

最初的两种文档模式：混杂模式和标准模式。

所有浏览器会默认开启混杂模式，不同浏览器在这种模式下行为差异非常大，如果不使用某些hack技术，跨浏览器的行为根本就没有一致性可言。

开启标准模式：
    
    触发标准模式:
    <!-- HTML 4.01 严格型 -->
    <!DOCTYPE HTML PUBLIC"-//W3C//DTD HTML4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
    <!-- XHTML 1.0 严格型 -->
    <!DOCTYPE htmlPUBLIC "-//W3C//DTD XHTML 1.0Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
    <!-- HTML 5 -->
    <!DOCTYPE html>
    
    触发准标准模式:
    <!-- HTML 4.01 过渡型 -->
    <!DOCTYPE HTMLPUBLIC "-//W3C//DTD HTML 4.01Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <!-- HTML 4.01 框架集型 -->
    <!DOCTYPE HTMLPUBLIC "-//W3C//DTD HTML 4.01Frameset//EN" "http://www.w3.org/TR/html4/frameset.dtd">
    <!-- XHTML 1.0 过渡型 -->
    <!DOCTYPE htmlPUBLIC "-//W3C//DTD XHTML 1.0Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
    <!-- XHTML 1.0 框架集型 -->
    <!DOCTYPE htmlPUBLIC "-//W3C//DTD XHTML 1.0Frameset//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-frameset.dtd">

## 2.4 <noscript>元素

浏览器不支持脚本或者脚本被禁用，才会触发<noscript>.

    <html>
    <head>
    	<meta charset="utf-8">
    	<title>noscript</title>
    </head>
    <body>
    	<noscript>
    		<p>本页面JavaScript脚本被禁用</p>
    	</noscript>
    </body>
    </html>