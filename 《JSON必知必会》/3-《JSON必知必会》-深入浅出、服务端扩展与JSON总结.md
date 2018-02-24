# 1. JSON与客户端框架
框架是一层位于软件或编程语言之上的为开发者提供支持的结构。

## 1.1 jquery和JSON
jquery是一种开发者专注于操作DOM构建功能的抽象化工具。

1、jQuery.parseJSON('{ "animal": "cat"}') 相比较JSON.parse()可以实现跨浏览器兼容。

2、JSON交互更加简单粗暴


JS创建一个XMLHttpRequest对象并从haha API获取JSON

    //一个新的XMLHttpRequest对象
    var myXMLHttpRequest = new XMLHttpRequest();
    var url = "http://api.haha.org/data";//指向保存着JSON资源的URL
    
    myXMLHttpRequest.onreadystatechange = function() {
    	if (myXMLHttpRequest.readyState === 4 && myXMLHttpRequest.status === 200) {
    		var myObject = JSON.parse(myXMLHttpRequest.responseText);
    		var myJSON = JSON.stringify(myObject);
    	}
    }
    myXMLHttpRequest.open("GET", url, true);
    myXMLHttpRequest.send();

jquery并从haha API获取JSON

    var url = "http://api.haha.org/data";//指向保存着JSON资源的URL
    $.getJSON(url, function(data) {
        // 对获取的数据进行一些操作
    });
## 1.2 AngularJS
jQuery框架是为DOM操作服务的抽象化工具，AngularJS框架是专注于创建单页应用的抽象化工具。单页Web应用致力于为用户提供无缝交互的应用体验。（当用户仍在当前页面时，幕后的代码已经完成对资源的请求，用户从一个资源跳跃到另一个资源的操作将不再需要通过URL或是指向URL的链接来进行。）

AngularJS框架这种抽象工具为开发者节约了从零开始构建单页应用的时间，它所提供的，是一个基于模型-视图-控制器(MVC)架构概念得到框架。

    模型
    JavaScript对象即数据模型
    
    视图
    HTML（提供了与模型进行数据绑定的语法）
    
    控制器
    使用AngularJS语法来定义和操作与模型和视图间的交互的JavaScript文件。
    
在AngularJS数据模型中，把数据库中的数据放到数据模型中的最常见的方式就是使用JSON，也就是通过HTTP协议来请求JSON数据，是一种客户端-服务端的关系，AngularJS通过其核心服务$htpp来使得通过这一协议进行的数据模型检索变得轻松。

> 使用AngularJS从haha API获取数据
    
    angular.module('myApp', [])
	    .controller('myAppController', function($scope, $http) {
			$http.get('http://api.haha.org/data')
				.success(function(data, status, headers, config) {
					$scope.weatherData = data;
				});
		});
		
> 绑定从haha API获取的天气数据的描述

    <body ng-app="myApp">
    	<div id="wrapper" ng-controller="myAppController">
    	    <div>
    	        {{ weatherData.weather[0].description }}
    	    </div>
    	</div>
    	// “ng-app”和“ng-controller”属性设置了一个支持数据绑定的视图，这是angularJS语法，详见angularJS
    </body>
    
    调用的是这段JSON数据：
    {
        "coord":{
            ……
        },
        "sys":{
            ……
        },
        "weather":[
            {
               "id":800,
               "main":"Clear",
               "description":"sky is clear",
               "icon":"02n"
            }
        ],
        "base":"stations",
        "main":{
            ……
        },
        "wind":{
            ……
        },
        "clouds":{
            ……
        },
        "dt":1234564564,
        "id":18656456465,
        "name":"Shuzenji",
        "cod":200
    }
    
# 2. JSON与NoSQL

## 2.1 CouchDB
CouchDB是一种使用JSON文档存储数据的NoSQL数据库，与平常的SQL并不同。

> 优点：

    1. CouchDB使用文档来存储数据，当从数据库中查询一个账户时，得到的直接就是一个结构化的文档，没有必要进行重组，这样既高效又方便。
    但是切记如果数据存储模型种类繁多需要的关系比较多那最好使用关系型模型数据库即SQL等。
    // 使用JSON文档来表示账户
    {
		"firstName": "Bob",
		"lastName": "Barker",
		"age": 91,
		"addresses":[
			{
				"city": "Somewhere",
				"state": "OR",
				"zip": 97500
			},
			{
				"city": "Some place",
				"state": "CA",
				"zip": 96026
			}
		]
	}
    
    2. 用CouchDB，当数据发生变化时就毋须修改表的结构了。
    
这里略微讲下：CouchDB API，有了CouchDB就可以通过HTTP请求资源的方式来获取数据库中的数据。

CouchDB:

> 它是一种面向文档得到NoSQL数据库

> 它存储和管理JSON文档

> 它会在存储与获取数据的同时维护好数据的结构

> 它使用基于HTTP的API来获取作为JSON文档资源的数据

> 它使用JS作为查询语言，且通过视图的map和reduce方法来跨API获取数据

# 3. 服务端的JSON
技术按客户端和服务端的区别分类：

客户端：HTML、CSS、JavaScript
服务端：PHP、ASP.NET、Node.js、Ruby on Rails、Java、Go, 等等。

==这一章是《JSON必知必会-第九章》，主要讲述了序列化、反序列化与请求JSON，通过ASP.NET、PHP论述；同时还讲述了发送JSON HTTP请求的其他方式包括Ruby on  Rails、Node.js、Java等方式。==【本章涉及过多了解性的服务端内容，除了node.js简单叙述一下，其余内容略过】

## 3.1 Node.js 发送HTTP
Node.js是服务端的JavaScript，它基于谷歌开源JavaScript引擎V8，通过Node.js就可以使用JavaScript编写服务端应用。


前面探讨过通过JSON.parse()将JSON反序列化为JavaScript对象，因为Node.js也是JS，所以方法相同。但是，在Node.js中不再使用XMLHttpRequest对象，在Node.js中通过更简单的get()函数请求JSON(以及其他类型的资源)。

    // 本示例像API发送请求，并将得到的JSON反序列化为JS对象。
    var http = require('http');
	http.get({
		host: 'api.haha.org',
		path: '/data/2.5/haha?q=Xushao,uk'
	}, function(response) {
		var body = '';
		response.on('data', function(data) {
			body += data;
		});
		response.on('end', function() {
			var hahaData = JSON.parse(body);
			console.log(hahaData.coord.lon);
		});
	});

# 4. JSON总结
JSON不仅仅是一种数据交换格式，还可以作为配置文件放在某一个地方。（比如将某个INI、XML格式文件修改为JSON文件，三者都有良好的可读性，并且都能迅速定位被修改完善。）【这里就不详细写例子了】

> 结语：

> 无论是作为服务器上的配置文件，还是作为通过URL请求的资源，JSON始终都在履行作为数据交换格式的职责。
    
> 在服务端，对象可以被序列化为JSON格式的文本，并且通过反序列化变回对象。服务端代码可以请求JSON（本笔记第三节有ASP.NET和PHP语言两种方式实现这一功能，没有详细叙述。）

> ▲JSXmlHttpRequest可以通过URL请求JSON资源，为了在JS代码中使用JSON，要先将它反序列化为JSON对象。JS内置的JSON.parse()函数能够快速有效地实现这一功能。

> JSON并不是唯一一种数据交换格式，有些数据以逗号分隔值(CSV)为载体，有些是Excel表格，还有些数据是XML格式，这种格式支持数据的嵌套。要记住，JSON不总是最佳选择。在互联网浏览器这类通过JS来支持面向对象编程的系统中，JSON是较为理想的数据交换格式。在那些面向对象的服务端Web框架之类的系统，JSON依旧是较为理想的数据交换格式。

总之，选择格式前先考虑优缺点，用对的工具，去做对的工作。o(∩_∩)o 

