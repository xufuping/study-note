# 1.JSON中的安全问题
Web中使用JSON时最常见的两个安全问题：跨站请求伪造和跨站脚本攻击。

客户端和服务端的关系：

    当我们提起客户端代码时，通常指的是javascript、HTML、CSS;
    当我们提到服务端代码时，常常是指一些服务端语言，如ASP.NET、Ruby on Rails、Java.
## 1.1跨站请求伪造（CSRF）
含义：利用站点对用户浏览器信任而发起攻击的方式。

例如：

    [
        {
            "user": "bobbarker"
        },
        {
            "phone": "555-555-5555"
        }
    ]//这种情况称为顶层JSON数组，它是合法的JSON，但是它们也是可以执行的JavaScript脚本。
    
如何阻止CSRF攻击：

    1.首先，应该将数组作为一个值存入JSON对象，这样数组将不再是合法的JavaScript.
    {
        "info":[
            {
                "user": "bobbarker"
            },
            {
                "phone": "555-555-5555"
            }
        ]
    }//将数组存放到对象之中，使其成为非法的javascript，这样就不会被<script>标签加载.
    2.下一步，应仅允许POST请求获取数据的时候，禁止使用GET请求，这样黑客便无法使用他自己的URL中的链接了。
    GET用于请求数据，得到响应；POST用于提交数据，得到响应。
    如果服务器端允许GET请求，就可以直接通过浏览器或<script>标签链接到它，但POST就不可以直接被链接到。
    
    PS：用户敏感数据也是引发窃取行为的关键。不在JSON中使用顶级数组，不要贪图使用GET代替POST的便利，这样你就不会写出那些满是漏洞、让用户感到愤怒的代码。
## 1.2注入攻击
含义：利用系统本身的漏洞来实现的。
### 1.2.1跨站脚本攻击
理解：JSON本身仅仅是文本，在编程中，如果想要对代表对象的文本进行操作，首先要将它转换成对象并装入内存中，这样它才能被编辑操作。

在JS中，可以使用eval()函数来进行这一操作，获取一段字符串，并对其编译与执行。但是，在有些情况下，我们的JSON是从别的服务器上获取的，该服务器通常会是你无权控制的第三方服务器。如果服务器本身或发来的JSON数据被人劫持，那么很可能就会运行恶意代码。eval()的问题就是它会将传入的字符串无差别地编译执行，如果被劫持并替换恶意脚本，那么访问站点就会无辜蒙冤。

JSON.parse()可以用来解决这样的问题，该函数仅会解析JSON，并不会执行脚本。【有一小部分老式浏览器不兼容该函数，可以将这一错误捕获，并弹出一条形如“请升级你的浏览器至最新版本”，来避免用户看到满是错误的页面】

### 1.2.2安全漏洞：决策上的失误
1.采取手段使得JSON消息中不包含HTML，可以在客户端和服务端都加上这一认证。

2.将消息中所有的HTML字符进行转码。

    诸如<div>这样的标签就会被转换成&lt;div&gt;，然后插入页面(&lt;div&gt;将不会是合法的HTML).

总之：允许在JSON中使用HTML以及直接将值插入页面都是一些幼稚的决策。抵御注入攻击的关键是要找出可能的注入点，并加入一些额外的步骤(有时可能会很麻烦)来加以防范。
# 2.JS中的XMLHttpRequest与Web API
javascript中的XMLHttpRequest负责在客户端发起请求，而Web API负责在服务端返回响应。

URL的全称是通用资源标识符，我们在浏览器中使用的URL通常指向HTML资源。
## 2.1 Web API
Web API 是通过HTTP服务进行交互的一族指令和标准。这些交互可以包括创建、读取、更新、删除(CRDU)等操作。

我们创建某段JSON数据后进行交互，JS在幕后进行的这些操作，比如请求天气数据，成为异步操作。异步操作通常指那些发生在幕后的、不会中断主进程的操作。在JS的异步操作中，“主进程”指Web浏览器的显示进程。

AJAX(Asynchronous JavaScript and XML)不仅是一种缩写，更是用来描述JS中的任何一种异步操作。

## 2.2 XMLHttpRequest对象
JS的XMLHttpRequest用它来发起HTTP请求，XML是在发起这类请求时最常用的数据交换格式，XMLHttpRequest并不仅限于使用XML。而那些允许我们通过访问站点进行数据交流的基础是基于超文本传输协议(HTTP)的。

JavaScript中，使用这种协议来发送这类请求的代码就是XMLHttpRequest。JavaScript是一种面向对象的语言，而XMLHttpRequest就是一类对象。当使用new XmlHttpRequest()语法，并将其返回值赋值给一个变量时，它就具有了从某一地址请求资源的功能。

我们主要关注XMLHttpRequest中的以下这些可用的函数：

    open(method,url,async(可选),user(可选),password(可选))
    send()
    onreadystatechange 可以在代码中给它赋值为一个函数
    readyState         返回一个0~4的值，用来表示状态码
        0 表示未发送   表示open()函数还没有执行
        1 表示已发送   指open()函数已执行，但send()函数还没有执行。
        2 表示接收到头部 表示send()函数已执行且头部和状态码status都可以获取了
        3 表示解析中   表示头部已经收到，但响应体正在解析中
        4 表示完成     表示请求完成，包括响应头和响应体的内容都已经接收到了。
    status             返回HTTP状态码
    responseText       当请求成功时，该属性会包含作为文本的响应体(如我们请求的JSON)
在JS中，一个重要原则：属性值可以是一个函数。因为JavaScript中的函数也是一类对象。对象是一类数据，因此它可以被赋值给一个变量（属性）、修改和传递。onreadystatechange的值应该是一个函数。

▲▲▲▲▲▲▲▲▲▲一个重要的交互实例▲▲▲▲▲▲▲▲

    //一个新的XMLHttpRequest对象
    var myXMLHttpRequest = new XMLHttpRequest();
	var url = "http://";//指向保存着JSON资源的URL

	myXMLHttpRequest.onreadystatechange = function() {
		if (myXMLHttpRequest.readyState === 4 && myXMLHttpRequest.status === 200) {
			var myObject = JSON.parse(myXMLHttpRequest.responseText);
			var myJSON = JSON.stringify(myObject);
		}
	}
	myXMLHttpRequest.open("GET", url, true);
	myXMLHttpRequest.send();
	
序列化：将对象转换成文本的过程

    通过JSON.stringify()对JSON进行序列化

反序列化：将文本转换成对象的过程

    使用javascript中的JSON.parse()进行反序列化操作，响应返回的JSON以文本的形式存储在responseText中。
    当它被JSON.parse()解析后，就不再是JSON了，而是JavaScript对象。
    
    var myJSON = JSON.parse(myXMLHttpRequest.responseText);
    //由于JSON一开始不是对象，使用JSON.parse()进行反序列化让JSON变成对象。
    
同源策略

    出于安全考虑，浏览器对资源共享有一定的限制。同源策略就要求此类后台请求仅可以请求来自同一域名的资源。
    由于后台请求仅可在浏览器的开发者工具中看见，因此保护了普通用户的安全。
## 2.3 混乱的关系与共享的原则
### 2.3.1 跨域资源共享(CORS)
通过AJAX向公共API发送请求而不受到同源策略的影响，这是因为在服务器上实现了==跨域资源共享==(CORS)。这些服务器会在响应头额外加上一些带有Access-Control-Allow前缀的属性。

    //定义了证书是否可用。
    Access-Control-Allow-Credentials:true
    //定义了哪些HTTP方法(GET、POST、PUT、DELETE、HEAD、OPTIONS、TRACE、CONNECT)是可用的。
    Access-Control-Allow-Methods:GET, POST
    //允许哪些域名，这里*表示任意域名都是允许的。
    Access-Control-Allow-Origin:*
银行防止黑客入侵

    //使用CORS进行安全防护
    Access-Control-Allow-Methods:POST
    Access-Control-Allow-Origin: http://www.bank.com
    本例中仅允许通过POST方式请求资源.
    同时设定了银行站点的URL，浏览器会禁止除了到"http://www.bank.com"以外的站点去获取资源.
### 2.3.2 JSON-P
作用：使用CORS也会遇到一些问题，而且也不是所有的JSON数据都是从标准的Web API获取的。比如你想要不同域名的多个站点共享一些JSON文件，你需要用到JSON-P.

JSON-P是一种不是很规范的备选方案，通过"padding(内联)"——将JavaScript加入JSON文档，然后创建函数调用。

    //JSON-P
    getTheAnimal(
        {
            "animal": "cat"
        }
    );
    
    //在JS中声明的函数
    function getTheAnimal(data) {
        var myAnimal = data.animal;//"cat"
    }
    
    //声明函数后利用<script>标签不受同源策略影响，将<script>标签动态添加到HTML文档的<head>标签中
    var script = document.createElement("script");
    script.type = "text/javascript";
    script.src = "http://xufuping.com/animal.json";
    document.getElementsByTagName('head')[0].appendChild(script);
    
    //服务端也需要对JSON-P提供一定的支持，它应该允许用户自定义函数的名字，它通常是作为URL中queryString的参数传递的
    script.src = "http://xufuping.com/animal.json?callback=getThing";
    
    //服务端会根据callback参数的值来动态地为在JSON中内联的函数命名。
    getThing(
        {
            "animal": "cat"
        }  
    );

JSON-P还需要服务端的不少支持，因为JSON资源必须包含JavaScript内联。不管是使用CORS还是JSON-P都离不开服务端的支持。因此，客户端跨域的XMLHttpRequest需要服务端的支持来保证JSON资源请求成功。

