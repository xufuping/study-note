# 一、使用Ajax（获取数据）
    本笔记概要：
    1、发起一个Ajax请求  创建一个XMLHttpRequest对象，然后调用open和send方法
    2、使用一次性事件追踪请求的进度  使用第二级的事件，比如onload、onloadstart、onloadend
    3、探测和处理错误  相应错误事件，或者使用try...catch语句
    4、设置Ajax请求的表头  使用setRequestHeader方法
    5、读取服务器相应的标头  使用getResponseHeader和getAllResponseHeaders方法
    6、发起跨源Ajax请求  设置服务器响应里的Access-Control-Allow-Origin标头
    7、中止一个请求  使用abort方法
## 1、Ajax起步
一个示例说明：

```
//随着用户点击各个水果按钮，浏览器会异步执行并取回所请求的文档，而主文档不会被重新加载。
//这就是典型的Ajax事件。
<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8">
	<title>Ajax Example</title>
</head>
<body>
	<div>
		<button>Apples</button>
		<button>Cherries</button>
		<button>Bananas</button>
	</div>
	<div id="target">
		你可以点击一个按钮试试
	</div>
	<script type="text/javascript">
		var buttons = document.getElementsByTagName('button');
		for (var i = 0; i < buttons.length; i++) {
			buttons[i].onclick = handleButtonPress;
		}

		function handleButtonPress(e) {
			var httpRequest = new XMLHttpRequest();
			//创建一个新的XMLHttpRequest对象，与DOM中见过的大多数对象不同，你并非通过浏览器定义的某个全局变量来访问这类对象，而是关键词new。
			httpRequest.onreadystatechange = handleResponse;
			//下一步是给readystatechange设置一个事件处理器，这个事件会在请求过程中被多次触发。
			httpRequest.open("GET",e.target.innerHTML + ".html");
			//这里告诉XMLHttpRequest你要做什么了，使用open方法来指定HTTP方法，在这里是GET和需要请求的URL.
			//▲这里是一个简单形式，你可以给浏览器提供向服务器发送请求时使用的认证信息，如下：
			//▲httpRequest.open("GET",e.target.innerHTML +".html",true,"adam","secret")
			//▲最后两个参数是应当发送给服务器的用户名和密码。true指定了该请求是否异步执行，它应该始终被设置为true。
			httpRequest.send();
			//这里没有向服务器发送任何数据，所以send方法无参数可用。
		}

		function handleResponse(e) {
			if (e.target.readyState == XMLHttpRequest.DONE && e.target.status == 200) {
					document.getElementsById("target").innerHTML = e.target.responseText;
			}
		}
	</script>
</body>
</html>
```
### 1.处理响应

```
function handleResponse(e) {
	if (e.target.readyState == XMLHttpRequest.DONE && e.target.status == 200) {
	    document.getElementsById("target").innerHTML = e.target.responseText;
    	}
    }
```
一旦脚本调用了send方法，浏览器就会在后台发送请求到服务器，因为请求是在后台处理的，所以Ajax依靠时间来通知你这个请求的进展情况。在这个例子中，我们用handleResponse函数处理这些事件。

当readystatechange事件被触发后，浏览器会把一个Event对象传递给指定的处理函数，target属性则会被设为与此事件关联的XMLHttpReques.

DONE状态并不意味着请求成功，它只代表请求已完成。可以通过status属性获得HTTP状态码，它会返回一个数值（这里200这个数值代表成功）。只有结合readyState和status属性的值才能够确定某个请求的结果（这个例子中就是只有当readyState的值为DONE并且status的值为200时我才会设置div元素的内容）。

用XMLHttpRequest.responseText属性获得服务器发送的数据，就象这样：

```
document.getElementsById("target").innerHTML = e.target.responseText;
```
responseText会返回一个字符串，代表从服务器上取回的数据。这里用这个属性来设置div元素innerHTML属性的值，以显示被请求文档的内容。（这样就可以构成一个简单的Ajax实例：用户点击一个按钮，浏览器在后台向服务器请求一个文档，当它到达时你处理一个事件，并显示被请求文档的内容。）
### 2.主流中的异类：应对Opera
Opera浏览器的XMLHttpRequest标准实现方式不如其他浏览器那么好或者完整，就如本章实例在其他主流浏览器上能完美工作，但需要做些修改来应对Opera的几个问题。如下：

```
<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8">
	<title>Ajax Example</title>
</head>
<body>
	<div>
		<button>Apples</button>
		<button>Cherries</button>
		<button>Bananas</button>
	</div>
	<div id="target">
		你可以点击一个按钮试试
	</div>
	<script type="text/javascript">
		var buttons = document.getElementsByTagName('button');
		for (var i = 0; i < buttons.length; i++) {
			buttons[i].onclick = handleButtonPress;
		}

		var httpRequest;

		function handleButtonPress(e) {
			var httpRequest = new XMLHttpRequest();
			httpRequest.onreadystatechange = handleResponse;
			httpRequest.open("GET",e.target.innerHTML + ".html");
			httpRequest.send();
		}

		function handleResponse() {
			if (httpRequest.readyState == 4 && httpRequest.status == 200) {
					document.getElementsById("target").innerHTML = httpRequest.responseText;
			}
		}
	</script>
</body>
</html>
```
第一个问题是，在出发readystatechange事件时不会省城一个Event对象。这就意味着必须把XMLHttpRequest对象指派给一个全局变量，这样才能在以后引用它。这个例子中定义个名为httpRequest的var，随后在handleButtonPress函数创建对象以及handleResponse函数处理已完成时请求调用了它。这个问题就是如果用户在请求处理过程中按下按钮，全局变量就会被指派给一个新的XMLHttpRequest对象，你就无法再与原来那个请求交互了。

第二个问题是Opera没有在XMLHttpRequest对象上定义就绪状态常量。这就意味着你必须用数值来比对readyState属性的值。必须依靠检查4这个数值，而不是XMLHttpRequest.
## 2.使用Ajax事件
    XMLHttpRequest对象定义的事件：
    abort                   在请求被中止时触发
    error                   在请求失败时触发
    load                    在请求成功完成时触发
    loadend                 在请求已完成时触发，无论成功还是发生错误
    loadstart               在请求开始时触发
    progress                触发以提示请求的进度
    readystatechange        在请求生命周期的不同阶段触发
    timeout                 如果请求超时则触发
    （除了readystatechange之外，表中展示的其他事件都定义于XMLHttpRequest规范的第二级。）
    （浏览器对这些事件支持程度不一，Firefox刘阿兰其有着最完整的支持，Opera完全不支持，而Chrome支持其中的一部分，但是所使用的方式并不符合规范。）
    （▲考虑到第二级事件的实现还不到位，readystatechange是目前唯一能可靠追踪请求进度的事件。）
##     3.处理错误
    使用Ajax必须留心两类错误，它们之间的区别源于视角不同。
    第一类问题是从XMLHttpRequest对象的角度看到的问题：某些因素组织了请求发送到服务器，例如DNS无法解析主机名，链接请求被拒绝或者URL无效。
    第二类问题是从应用程序的角度看到的问题，而非XMLHttpRequest对象：它们发生于请求成功发送至服务器，服务器接受请求、进行处理并生成响应，但该响应并不指向你期望的内容时。
一个处理Ajax错误的代码示例：

```
<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8">
	<title>Ajax Example</title>
</head>
<body>
	<div>
		<button>Apples</button>
		<button>Cherries</button>
		<button>Bananas</button>
		<button>Cucumber</button>
		<button id="badhost">Bad Host</button>
		<button id="badurl">Bad URL</button>
	</div>
	<div id="target">你可以点击一个按钮试试</div>
	<div id="errormsg"></div>
	<div id="statusmsg"></div>
	<script type="text/javascript">
		var buttons = document.getElementsByTagName('button');
		for (var i = 0; i < buttons.length; i++) {
			buttons[i].onclick = handleButtonPress;
		}

		var httpRequest;

		function handleButtonPress(e) {
			clearMessages();
			var httpRequest = new XMLHttpRequest();
			httpRequest.onreadystatechange = handleResponse;
			httpRequest.onerror = handleError;
			try{
				switch (e.target.id){
					case "badhost":
						httpRequest.open("GET","http://a.nodomain/doc.html" );
						break;
					case "badurl":
						httpRequest.open("GET","http://");
						break;
					default:
						httpRequest.open("GET",e.target.innerHTML + ".html");
						break;
				}
					httpRequest.send();
			} catch (error){
				displayErrorMsg("try/catch",error.message);
			}
		}

		function handleError(e) {
			displayErrorMsg("Error event",httpRequest.status + httpRequest.statusText);
		}

		function handleResponse() {
			if (httpRequest.readyState == 4) {
					var target = document.getElementById('target');
					if (httpRequest.status == 200) {
						target.innerHTML =httpRequest.responseText;
					}else{
						document.getElementById("statusmsg").innerHTML = "Status:" + httpRequest.status + " " +httpRequest.statusText;
					}
			}
		}...

		function displayErrorMsg(src,msg) {
			document.getElementById("errormsg").innerHTML = src + ":" +msg;
		}

		function clearMessages() {
			document.getElementById("errormsg").innerHTML = "";
			document.getElementById("statusmsg").innerHTML = "";
		}
	</script>
</body>
</html>
```
### 3.1处理设置错误
你需要处理的第一类问题是向XMLHttpRequest对象传递了错误的数据，比如格式不正确的URL，它们极其容易发生在生成基于用户输入的URL时，为了模拟这类问题，示例文档添加了一个Bad URL(错误的URL)的button，按下这个button会以下列形式调用open方法：

    httpRequest.open("GET","http://");
通常，提示用户在某个input元素里输入一个值，其中的内容会被用于生成Ajax请求所需的URL。当用户触发了请求却没有输入值时，传递给open方法的就会是一个残缺的URL，而上面这句话只有协议部分。

这是一种会阻止请求执行的错误，而XMLHttpRequest对象会在发生这类事件时抛出一个错误。这就意味着你需要用一条try...catch语句来围住设置请求的代码，就像这样：
```
try{
        ...
        httpRequest.open("GET","http://");
        ...
        httpRequest.send();
    } catch (error){
        displayErrorMsg("try/catch",error.message);
    }
```
catch子句让你有机会从错误中恢复。可以选择提示用户输入一个值，也可以回退到默认的URL，或者是简单地丢弃这个请求。在这个例子中，仅仅是调用了displayErrorMsg函数来显示错误消息。这个函数是在示例脚本中定义的，它会在ID为errormsg的div元素里显示Error.message这个属性。
### 3.2处理请求错误
第二类错误发生在请求已生成，但其他方面出错时。
