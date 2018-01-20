**==这一节是关于http网络协议/ajax请求相关的附加补充==**

## 1. ajax请求和普通http请求的区别？

两者本质区别：

`AJAX`通过`xmlHttpRequest对象`请求服务器服务器接受请求返数据实现刷新交互。

`普通http请求`通httpRequest象请求服务器接受请求返数据需要页面刷新。

`传统http请求`的发起者是一个页面，发出的页面同时会处于不可用状态，等待数据刷新。浏览器接收到服务器的响应后要刷新整个页面（即使只有页面中的一小部分需要更新）；`Ajax`中的异步请求发出者是页面中的一个HttpRequest对象，页面本身的显示和操作在请求和接收数据的过程中不受影响。浏览器接收到服务器的响应后传递给对应的处理函数，由该函数决定做什么。

**AJAX请求**
![image](http://images2015.cnblogs.com/blog/1192920/201707/1192920-20170708222608987-1559101214.png)

**普通请求**
![image](http://images2015.cnblogs.com/blog/1192920/201707/1192920-20170708222714175-232115862.png)

## 2. http、https、FTP、TCP有什么不同？

## 3. 同源策略和跨域问题？

## 4. cookie sessionStorage localStorage有什么不同？

## 5. HTML5本地存储分为？

webStorage(localStorage,sessionStorage)

indexDB

## 6. get和post的区别？

请求参数：get参数附在URL后面?隔开，POST参数放在包体中

大小限制：GET限制为2048字符，post无限制

安全问题：GET参数暴露在URL中，不如POST安全

浏览器历史记录：GET可以记录，POST无记录

缓存：GET可被缓存，post无

书签：GET可被收藏为书签，post不可

数据类型：GET只能ASCII码，post无限制

## 7. Jquery中$(document).ready()和window.onload的区别？

http://www.php100.com/html/program/jquery/2013/0905/5954.html



【cookie？搜索引擎？html5本地存储？代理、缓存、网关、隧道、Agent代理？抓包什么时候用，有什么用？爬虫以及爬虫方式？】

附：[浏览器内核以及代表浏览器](http://jingyan.baidu.com/article/5553fa82d50eaf65a339346c.html)