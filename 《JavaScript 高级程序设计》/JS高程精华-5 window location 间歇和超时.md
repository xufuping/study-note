> 目录
>
> 1. window对象
> >
> > 1.1 屏幕/浏览器/元素宽高
> > 
> > 1.2 导航和打开窗口
> >
> > 1.3 系统对话框
>
> 2. 间歇调用和超时调用
>
> 3. location对象
> >
> >3.1 位置操作

### 1.window对象
浏览器中，window对象既是通过JS访问浏览器窗口的一个接口，又是ECMAScript规定的Global对象。

所有在全局作用域中声明的变量、函数都会变成window对象的属性和方法。

定义全局变量和在window对象上定义属性的一点差别：全局变量不能通过delete操作符删除，但window对象上定义的属性可以。

    var age = 29;
	window.color = "red";

	delete window.age; // 无效
	delete window.color; // 删除

	console.log(window.age); // 29
	console.log(window.color); // undefined
	
#### 1.1 屏幕/浏览器/元素宽高

    console.log(document.body.clientWidth);       //网页可见区域宽(body)
	console.log(document.body.clientHeight);      //网页可见区域高(body)
	console.log(document.body.offsetWidth); //网页可见区域宽(body)，包括border、margin等
	console.log(document.body.offsetHeight); //网页可见区域宽(body)，包括border、margin等
	console.log(document.body.scrollWidth); //网页正文全文宽，包括有滚动条时的未见区域
	console.log(document.body.scrollHeight); //网页正文全文高，包括有滚动条时的未见区域
	console.log(document.body.scrollTop);
	//网页被卷去的Top(滚动条)
	console.log(document.body.scrollLeft);
	//网页被卷去的Left(滚动条)
	console.log(window.screenTop);                //浏览器距离Top
	console.log(window.screenLeft);               //浏览器距离Left
	console.log(window.screen.height);            //屏幕分辨率的高
	console.log(window.screen.width);             //屏幕分辨率的宽
	console.log(window.screen.availHeight);       //屏幕可用工作区的高
	console.log(window.screen.availWidth);        //屏幕可用工作区的宽
	
![屏幕/浏览器/元素宽高](http://images2015.cnblogs.com/blog/153475/201512/153475-20151222173139109-87271821.png)

#### 1.2 导航和打开窗口
使用window.open()方法既可以导航到一个特定的URL，也可以打开一个新的浏览器窗口。这个方法接收四个参数：要加载的URL、窗口目标、一个特性字符串以及一个表示新页面是否取代浏览器历史记录中当前加载页面的布尔值(通常只传递一个参数，最后一个参数只在不打开新窗口的情况下使用)。

    (function(){
		var xixi = document.getElementById("xixi");

		xixi.onclick = function(){
		    // 新的页面打开百度链接
			window.open("https:www.baidu.com","_blank");
		}
	})();
	
#### 1.3 系统对话框
浏览器通过alert()、confirm()、prompt()方法可以调用系统对话框向用户显示消息。通过这几个方法打开的对话框都是同步和模态的(也就是说，显示这些对话框的时候代码会停止执行，而关掉这些对话框后代码又恢复执行)。

### 2.间歇调用和超时调用
==超时调用==：使用window对象的setTimeout()方法，它接收两个参数，第一个参数一般是函数，第二个参数是以毫秒表示的时间。
	
如果要取消尚未执行的超时调用，使用clearTimeout()方法并将相应的超时调用ID作为参数传递给它。

    // 不推荐第一个参数使用字符串，会导致性能损失，推荐的调用方式如下：
    var timeoutId = setTimeout(function() {
		console.log("123");
	}, 1000)
	
	clearTimeout(timeoutId); // 取消超时调用

==间歇调用==：按照指定时间间隔重复执行代码，直到间歇调用被取消或者页面被卸载。设置间歇调用的方法是setInterval(),接收的参数是要执行的函数和每次执行前需要等待的毫秒数。

    // 每隔5秒输出
    setInterval(function() {
		console.log("123");
	}, 1000)
	
注：在使用超时调用时，没必要跟踪超时调用ID，因为每次执行代码之后，如果不再设置另一次超时调用，调用就会自行停止。一般认为，使用超时调用来模拟间歇调用的是一种最佳模式。在开发环境中，很少使用真正的间歇调用，原因是一个间歇调用可能会在前一个间歇调用结束之前启动。所以像下面这样使用超时调用，则完全可以避免这一点：

    vfunction incrementNumber() {
    	num++;

    	// 如果执行次数未达到max的值，则设置另一次超时调用
    	if (num < max) {
    		console.log('我是第' + num + '次输出'); //输出1~9次
    		setTimeout(arguments.callee, 200);
    	} else {
    		console.log("XuQingfeng"); // 第10次输出XuQingfeng
    	}
    }

    setTimeout(incrementNumber, 200);

### 3.location对象
location对象很特别，它既是window对象的属性，也是document对象的属性。换句话说：window.location和document.location引用的是同一个对象。

常用location对象属性：
属性名 | 说明
---|---
hash | 返回URL中的hash（#号后跟零或多个字符），如果URL中不包含散列，则返回空字符串
search | 返回URL中的查询字符串，这个字符串以问号开头
hostname | 返回不带端口号的服务器名称
pathname | 返回URL中的目录或文件名
port | 返回URL中指定的端口号，如果URL中不包含端口号，则这个属性返回空字符串

#### 3.1 位置操作
改变浏览器位置的方法：

    法一：
    window.location = "https://www.baidu.com";
    
    法二：
    location.href = "https://www.baidu.com";   

通过hash、search、hostname、pathname和port修改URL：

    // 假设初始值为"https://www.baidu.com", 每次修改location页面都会以新URL重新加载
    
    // 将URL修改为"https://www.baidu.com/#section"
    loction.hash = "#section";
    
    // 将URL修改为"https://www.baidu.com/?name=xuqingfeng"
    location.search = "?name=xuqingfeng";
    
    // 将URL修改为"https://www.taobao.com"
    location.hostname = "www.taobao.com";
    
    // 将URL修改为"https://www.taobao.com/myId"
    location.pathname = "myId";
    
    // 将URL修改为"https://www.taobao.com:8080/myId"
    location.port = "8080";
    
`reload()`重新加载当前显示页面。位于`reload()`后的代码可能不会执行，最好是将`reload()`放在最后一行

    location.reload(); // 重新加载，可能从缓存中加载
    location.reload(true); // 重新加载，从服务器重新加载
    
    