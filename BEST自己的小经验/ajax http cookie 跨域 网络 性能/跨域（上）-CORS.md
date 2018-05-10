同源策略是对XHR的一个主要约束，它为通信设置了“相同的域、相同的端口、相同的协议”这一限制。所以默认情况下， `XHR对象`对象只能访问与包含它的页面位于同一个域中的资源。

跨域源资源共享
---
`CORS(Cross-Origin Resource Sharing`, 跨域资源共享)，基本思想就是使用自定义的HTTP头部让浏览器与服务器进行沟通，从而决定请求或响应的成功与失败。

*一个简单使用GET或POST发送请求实例*：

发送该请求时，需要给它附加一个额外的Origin头部，其中包含请求页面的源信息——协议、域名和端口，以便服务器根据这个头部信息来决定是否给予响应:

    Origin: http://www.nczonline.net

如果服务器认为这个请求可以接收，就在 `Access-Control-Allow-Origin` 头部中回发相同的源信息（如果是公共资源，可以回发“*”）:

    Access-Control-Allow-Origin: http://www.nczonline.net

如果没有这个头部，或者有这个头部但源信息不匹配，浏览器就会驳回请求。**请求和响应都不包含cookie信息。**

### 1.CORS的实现
#### 1.1 IE
IE8引入了`XDR类型`，与XHR相似，不同之处：

1. `cookie` 不随请求发送，也不随响应返回 
2. 只能设置请求头部信息中的 `Content-Type` 字段
3. 不能访问响应头部信息
4. 只支持GET和POST请求

这些变化使`CSRF(跨站点请求伪造)` 和 `XSS(跨站点脚本)`的问题得到了缓解。

`XDR对象`的`open()`方法只接收两个参数：请求的类型和URL。所有 `XDR请求` 都是异步执行，请求返回后，只要响应有效就会触发`load`事件，如果失败就会触发 `error`事件（除了错误本身之外，没有其他信息返回，所以检测错误一般指定一个 `onerror` 事件处理程序）。

    var xdr = new XDomainRequest();
	xdr.onload = function () {
		console.log(xdr.responseText);
	};
	// 建议一定记得通过 onerror 事件处理错误
	xdr.onerror = function () {
		console.log("There is error!");
	};
	xdr.open("get", "http://www.somewhere-else.com/page/");
	xdr.send(null);
	
请求返回前可以调用 `abort()`方法终止请求，`XDR对象` 也支持 `timeout` 属性以及 `ontimeout` 事件。

为支持 `POST`请求，`XDR对象` 提供了 `contentType`属性，用来表示发送数据的格式：

    ...
    xdr.open(...);
    xdr.contentType = "application/x-www-form-urlencoded";
    xdr.send(...);

#### 1.2 其他浏览器
请求位于另一个域中的资源，使用标准的 `XHR对象` 并在 `open()` 方法中传入绝对URL即可。

    var xhr = new XMLHttpRequest();
    
    // 通过跨域XHR对象可以访问 status 和 statusText 属性，还支持同步请求。
	xhr.onreadystatechange = function () {
		if (xhr.readyState == 4) {
			if ((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304) {
			    console.log(xhr.responseText);
			} else {
			    console.log(xhr.status);
			}
		}
	};
	xhr.open("get", "http://www.somewhere-else.com/page/", true);
	xhr.send(null);
	
跨域XHR对象也有一些限制：
1. 不能使用 `setRequestHeader()` 设置自定义头部
2. 不能发送和接收`cookie`
3. 调用 `getALLResponseHeaders()` 方法总会返回空字符串

**▲ 由于无论同源请求还是跨域请求都使用相同的接口，因此对于本地资源，最好使用相对URL，在访问远程资源时再使用绝对URL。这样做能消除歧义，避免出现限制访问头部或本地`cookie`信息等问题。**

### 2. Preflighted Requests 和 withCredentials
*（以上属性IE10及以前版本不支持。）*

#### 2.1 Preflighted Requests
`Preflighted Requests` (一种透明服务器验证机制)支持开发人员使用自定义的头部、GET或POST之外的方法，以及不同类型的主体类容。

#### 2.2 带凭据的请求
默认情况下，跨源请求不提供凭据(cookie，HTTP认证及客户端SSL证明等)。但把 `withCredentials` 属性设置为true，可以指定某个请求应该发送凭据。如果服务器接受带凭据的请求，会用下面的HTTP头部来响应：

    Access-Control-Allow-Credentials: true

### 3. 跨浏览器的 CORS
所有浏览器都支持简单的CORS请求，所以可以实现一个跨浏览器的方案，如下所示：

    function createCORSRequest (method, url) {
		var xhr = new XMLHttpRequest();

		// 检测 withCredentials 属性 ，Firefox3.5+,Safari4+,Chrome支持这个属性
		if ("withCredentials" in xhr) {
			xhr.open(method, url, true); // 异步请求
		} 
		//	检测 XDomainRequest 对象，IE8及以上支持这个属性
		else if (typeof XDomainRequest != "undefined") {
			xhr = new XDomainRequest();
			xhr.open(method, url);
		} else {
			xhr = null;
		}
		return xhr;
	}

	// 一个跨源请求实例
	var request = createCORSRequest("get", "http://www.somewhere-else.com/page/");
	if (request) {
		request.onload = function () {
			console.log(request.responseText);
		};
		request.send();
	}
