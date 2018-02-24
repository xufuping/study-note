Angular ?
AngularJS以HTML作为模版语言并扩展HTML元素及属性，使得应用组件开发保持高度清晰、一致。

# 1.第一个AngularJS应用及剖析

    <!DOCTYPE html>
    <html ng-app>
    <head>
    	<meta charset="utf-8">
    	<title>First AngularJS</title>
    	<script type="text/javascript" src="angular.js"></script>
    </head>
    <body>
    	<div>{{"Hello World!"}}</div>
    	<div>{{"Hello AngularJS!"}}</div>
    </body>
    </html>
    1.<html>中的ng-app有什么用？
    一是启动AngularJS框架，二是告诉AngularJS框架从ng-app指令所在标签的开始标签到结束标签之间的所有DOM元素由AngularJS框架进行管理。
    2.{{}}是AngularJS的表达式形式，中间为表达式内容。
    上面的例子中，表达式为字符串字面量，AngularJS会将该字面量输出到页面。

# 2.AngularJS应用构成元素

> 模型(Model)：AngularJS程序中用于展示到页面的数据，本质是一个JavaScript对象。

> 视图(View)：从用户角度来看，视图就是用户所看到的网页内容；从AngularJS应用的角度来说，视图则是AngularJS指令与表达式经过解析后的DOM元素。

> 控制器(Controller)：AngularJS应用中用于处理业务逻辑的JavaScript方法。

> 作用域(Scope)：可以把作用域理解为一个容器，在控制器中可以访问这个容器，然后往容器中放入一些模型数据，在视图中就可以通过表达式将数据展现给用户。

> 指令(Directives)：扩展的HTML属性或标签，能够被AngularJS框架识别，根据不同的指令执行相应的动作。例如，前面提到的ng-app指令，作为html元素的扩展属性，能够被AngularJS框架识别，从而启动AngularJS框架。

> 表达式(Expressions)：用于向页面输出信息，在前面已经接触过，下节将会对表达式做更详细的介绍。

> 模板(Template)：AngularJS以HTML作为模版语言，AngularJS模板实际上就是HTML片段。

# 3.AngularJS表达式
表达式的定义方式：{{expression}}

可以在其中加入四则运算，逻辑运算，可以通过表达式输出作用域内容。(==ng-init用于在标签中初始化作用域==，并可以在其中添加对象、数组等类型。)