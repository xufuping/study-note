AJAX
---
`Ajax`的核心是`XMLHttpRequest`对象(简称`XHR`)。

### 1. XMLHttpRequest 对象
创建`XMLHttpRequest`对象：

    var xhr = new XMLHttpRequest();

*如果需要兼容IE早期版本，需要加入对原生`XHR`对象的支持。*

#### 1.1 XHR 的用法
##### `open()` 方法
接收三个参数：① 请求类型("get", "post"等)；② 请求的URL；③ 是否异步发送请求(布尔值)。

    // url相对于执行代码的当前页面;调用open()只是启动一个请求准备发送，并不会真正发送请求。
    // 只能向同一个域中使用相同端口和协议的URL发送请求，如果URL与启动请求的页面有差别，就会引发安全错误。
    xhr.open("get", "example.php", false);

##### `send()` 方法
发送请求到服务器，接收一个参数，即作为请求主体发送的数据。如果没有主体数据，**必须传入`null`**。

收到响应后，响应数据会自动填充XHR对象的属性：

属性 | 作用
---|---
**responseText** | 作为响应主体返回的文本
responseXML | 响应类型是"text/xml"或"application/xml"，这个属性保存包含响应数据的`XML DOM`文档
**status** | 响应的HTTP状态
statusText | HTTP状态说明

状态码 `200` 为成功，`304` 表示请求资源未被修改，可以直接使用浏览器中缓存版本。建议通过检测 `status` 决定下一步的操作。

    xhr.open("get", "example.php", false);
    xhr.send(null);
    
    if ((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304) {
        console.log(xhr.responseText);
    } else {
        console.log(xhr.status);
    }

##### ▲ 异步
执行异步请求，可以让JS继续执行而不必等待响应。可以通过检测`XHR对象`的`readyState`属性，这个属性表示请求响应过程中的当前活动阶段，取值如下：

取值 | 说明
---|---
0 未初始化|尚未调用open() 
1 启动| 已经调用open()，尚未调用send()
2 发送| 已经调用send()，尚未收到响应
3 接收| 已经收到部分响应数据
4 完成| 已经收到全部响应数据

只要 `readyState` 属性值改变就会触发一次 `readystatechange` 事件。所以，我们通常在调用 `open()` 之前指定 `onreadystatechange` 事件处理程序(确保跨浏览器兼容性)。

    var xhr = new XMLHttpRequest();
	xhr.onreadystatechange = function () {
		if (xhr.readyState == 4) {
			if ((xhr.status >= 200 && xhr.status <= 300) || xhr.status == 304) {
				console.log(xhr.responseText);
			} else {
				console.log(xhr.status);
			}
		}
	};
	xhr.open("get", "http://rap.taobao.org/mockjsdata/14169/geek", true);
	xhr.send(null);
	
	说明：因为 onreadystatechange 作用域的问题，以防在某些浏览器函数执行失败或出错，在 onreadystatechange事件处理程序中使用xhr对象，而不是this对象

补充：接收到响应前可以调用 `abort()` 方法取消异步请求。调用这个方法后，XHR对象会停止触发事件，而且不再允许访问任何与响应有关的对象属性。**由于内存原因，不建议重用XHR对象。**

#### 1.2 HTTP头部
HTTP请求和响应都会带有相应头部信息。发送 `XHR请求` 时会发送下列头部信息：

属性 | 说明
---|---
Accept | 浏览器能处理的内容类型
Accept-Charset | 浏览器能显示的字符集
Accept-Encoding | 浏览器能处理的压缩编码
Accept-language | 浏览器当前设置的语言
Connection | 浏览器与服务器的连接类型
Cookie | 当前页面设置的任何Cookie
Host | 发出请求的页面所在的域
Referer | 发出请求的页面的URI
User-Agent | 浏览器的用户代理字符串

`setRequestHeader()` 方法可以设置自定义的请求头部信息，接收两个参数：头部名称和头部字段的值。必须在 `open()方法` 之后 `send()` 方法之前调用。不建议修改浏览器正常发送的字段名称以免影响服务器的响应，但可以使用自定义的头部字段名称。

`getResponseHeader()` 方法并传入头部字段名称，可以取得相应的响应头部信息;`getAllResponseHeaders()` 方法取得一个包含所有头部信息的长字符串。

#### 1.3 GET请求
GET请求常用于向服务器查询某些信息。

可以将查询字符串参数追加到URL末尾，以便将信息发送给服务器。使用GET请求查询字符串，每个参数的名称和值都必须用 `encodeURIComponent()` 进行编码，然后才能放到URL末尾；并且所有名-值对都必须由 `&` 分隔。

    // 添加一个辅助函数向现有的URL末尾添加查询字符串参数
    // addURLParam()函数接收三个参数：URL，名，值
    function addURLParam (url, name, value) {
		url += (url.indexOf("?") == -1 ? "?" : "&"); // 检测URL是否有“？”符号，若无则加，若有则采用“&”符号
		url += encodeURIComponent(name) + "=" + encodeURIComponent(value); // 将名值对进行编码添加到URL末尾 
		return url; // 返回url
	}
	
	var xhr = new XMLHttpRequest();
	var url = "example.php";
	
	// 添加参数
	url = addURLParam(url, "name", "XuShao");
	url = addURLParam(url, "age", 21);

	// 初始化请求
	xhr.open("get", url, false);

	console.log(url); // "example.php?name=XuShao&age=21"

#### 1.4 POST请求
`POST请求`通常用于向服务器发送应该被保存的数据。与 `GET请求`相比，`POST请求`消耗的资源会更多一些，`GET请求` 速度比 `POST请求` 更快。 

### 2. XMLHttpRequest 2级
#### 2.1 FormData
使用FormData对象，可以向服务器发送表单数据和上传文件。

    <form id="info">
		<label for="name">姓名：</label>
		<input type="text" id="name">
		<br>
		<label for="age">年龄：</label>
		<input type="text" id="age">
	</form>
	<br>
	<button id="confirm">确定</button>
	<br>
	<output id="result"></output>

	<script>
	var confirm = document.getElementById("confirm");
	var info = document.getElementById("info");
	var data = new FormData(info);

	confirm.addEventListener("click", function(){
		var xhr = new XMLHttpRequest();
		xhr.open('POST', 'test.php', true);
		xhr.onload = function (e) {
			if (xhr.status == 200) {
				document.getElementById("result").innerHTML = this.response;
			}
		};
		xhr.send(data);
	}, false);

	</script>

#### 2.2 超时设定
IE8 为 XHR 添加了一个 `timeout` 属性，表示请求在等待响应多少毫秒之后就终止。终止后调用 `ontimeout` 事件处理程序。超时终止请求之后再访问 `status` 属性就会导致错误。

#### 2.3 overrideMimeType()
这个方法用于重写XHR响应的 `MIME类型`。此方法在 `send()`方法之前调用。

### 3.进度事件
#### 3.1 load 事件
相应接收完毕后将触发load事件。但你必须要检查 `status` 属性，才能确定数据是否真的已经可用。

    var xhr = new XMLHttpRequest();
    xhr.onload = function () {
        ...
    };
    xhr.open(...);
    xhr.send(null);

#### 3.2 progress 事件
在浏览器接收新数据期间周期性触发。`onprogress` 事件处理程序会接收到一个 `event` 对象，其 `target`属性是 `XHR对象`，必须在 `open()`方法之前调用。它包含着三个额外的属性： `lengthComputable`(一个表示进度信息是否可用的布尔值);`position`(表示已经接收的字节数);`totalSize`(表示根据 `Content-Length` 响应头部确定的预期字节数)。

我们可以用这个事件为用户创建进度指示器：

    var xhr = new XMLHttpRequest();
	
	xhr.onload = function (e) {...};
	xhr.onprogress = function (e) {
		var divstatus = document.getElementById("status");
		if (event.lengthComputable) {
			divstatus.innerHTML = "Received" + event.position + "of" + event.totalSize + "bytes";
		}
	};
	xhr.open('get', 'http://rap.taobao.org/mockjsdata/14169/geek', true);
	xhr.send(null);