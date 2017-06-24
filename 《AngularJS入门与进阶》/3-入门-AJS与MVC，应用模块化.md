MVC是一种软件架构模式，MVC的核心思想是把数据的管理、业务逻辑控制和数据的的展示分离开，使程序的逻辑性和可维护性更强。

# 1. AngularJS的MVC

    Model:前面提到过的作用域对象（如$rootScope对象）中的属性。
    View:大家所熟悉的DOM元素，从用户的角度来看就是HTML页面，在View中可以通过AngularJS表达式访问模型数据。
    Controller:用户自定义的构造方法，作用域中的模型数据可以通过依赖注入的方式注入控制器中。

## 1.1 AngularJS控制器的定义

> 我们是用模块实例controller()方法来声明一个控制器，该方法接收两个参数，第一个参数为控制器名称，第二个参数为一个匿名方法，即控制器的构造方法，用法如下：

    <script type="text/javascript">
		var app = angular.module("app", []);
		app.controller("LoginController", function($scope, $log) {

			$scope.name = "admin";

			$scope.pword = "123456";
		});
	</script>
	AJS在windows对象中添加了一个全局的angular对象，我们调用angular对象的module()方法返回一个模块实例，然后调用模块实例的controller()方法来声明一个控制器。
	
	在上面的案例中我们定义控制器时指定了$scope和$log两个参数：
	$scope是作用域对象，是控制器与视图之间传递信息的载体;
	$log为AngularJS框架内置的日志服务对象，用于向控制台中输入日志信息。
	当我们为控制器构造方法指定这两个参数后，表示控制器依赖于这两个对象，控制器实例化时会把这两个对象注入控制器中。
	
## 1.2 控制器：ng-controller
AJS遇到ng-controller时会根据ng-controller指定的控制器名称查找控制器构造方法，然后使用对应的构造方法实例化控制器对象，并将控制器依赖的对象注入控制器对象中。

每个控制器对应的作用域对象只能与ng-controller所在标签的开始标签和结束标签之间的DOM元素建立数据绑定。

    <!DOCTYPE html>
    <html ng-app="app">
    <head>
    	<meta charset="utf-8">
    	<title>First AngularJS</title>
    	<script type="text/javascript" src="angular.js"></script>
    </head>
    <body>
    	<div ng-controller="UserController" style="border:#ccc solid 1px;">
    		用户名:	<input type="text" ng-model="name" placeholder="用户名" />
    		密码: <input type="password" ng-model="pword" placeholder="密码" />
    		<button>提交</button>
    		<p>你输入的用户名：{{name}}</p>
    		<p>你输入的密码：{{pword}}</p>
    	</div>
    	<br/>
    	<div ng-controller="InfoContoller" style="border:#ccc solid 1px;">
    		个人爱好：<input type="text" ng-model="love" placeholder="个人爱好" />
    		<p>你输入的个人爱好： {{love}}</p>
    	</div>
    	<script type="text/javascript">
    		function UserController($scope, $log) {
    			$scope.name = "admin";
    			$scope.pword = "123456";
    			$log.info("UserController->name:" + $scope.name);
    			$log.info("UserController->pword" + $scope.pword);
    		}
    		function InfoContoller($scope, $log) {
    			$scope.love = "足球";
    			$log.info("InfoContoller->name:" + $scope.name);
    			$log.info("InfoContoller->pword" + $scope.pword);
    			$log.info("InfoContoller->love" + $scope.love);
    		}
    		var app = angular.module("app", []);
    		app.controller ("UserController", UserController);
    		app.controller ("InfoContoller",InfoContoller);
    	</script>
    </body>
    </html>

AJS应用中的DOM事件处理可以在控制其中完成，AJS框架为我们提供了一系列的事件绑定指令，这些指令是在原生的JS事件名称前加“ng-”前缀，例如ng-click、ng-keyup等。

# 2. 应用模块化

应用模块划分的重要性：

    1.使程序实现逻辑更加清晰；
    2.合作开发更加明确，容易控制；
    3.充分利用可以重用的代码；
    4.抽象出可公用的模块，可维护性强，以避免同一处修改在多个地方出现；
    5.可基于模块化设计优秀的遗留系统，方便组装开发新的相似系统，甚至一个全新的的系统。

##  2.1 AngularJS中的模块
定义：调用angular对象的module()方法返回一个模块实例。

> angular.module()方法能够接收3个参数：

> 1.第一个参数为模块的名称；

> 2.第二个参数是一个数组，用于指定该模块依赖的模块名称。如果不需要依赖其他模块，第二个参数传递一个空数组即可；

> 3.第三个参数为可选参数，该参数接收一个方法，用于对模块进行配置，作用和模块实例的config()方法相同。

> PS：angular.module()方法返回一个模块实例对象，我们可以调用该对象的controller()、directive()、filter()等方法向模块中添加控制器、指令、过滤器等其他组件。

    // 定义一个无依赖模块
    angular.module('appModule', []);
    // 定义一个依赖module1、module2的模块
    angular.module('appModule', ['module1', 'module2']);
    
    在HTML引用模块：
    <html ng-app="appModule">
    
使用模块解决命名冲突问题：调用angular.module()方法创建两个或者多个模块，以不同名称命名，然后在不同页面使用ng-app调用对应名称模块下的内容。