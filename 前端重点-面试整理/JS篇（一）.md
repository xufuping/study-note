# 本章问题汇总：



> 这里还没写目录



## 1.DOM操作——怎样添加、移除、移动、复制、创建和查找节点。 

    （1）创建新节点
    
          createDocumentFragment() //创建一个DOM片段
          createElement() //创建一个具体的元素
          createTextNode() //创建一个文本节点
    
    （2）添加、移除、替换、插入
    
          appendChild() //添加
          removeChild() //移除
          replaceChild() //替换
          insertBefore() //在已有的子节点前插入一个新的子节点
    
    （3）查找
    
          getElementsByTagName() //通过标签名称
          getElementsByName()
          //通过元素的Name属性的值(IE容错能力较强，会得到一个数组，其中包括id等于name值的)
          getElementById() //通过元素Id，唯一性

## 2.null和undefined的区别？

1.`null`和`undefined`的对比：

    1.`null`是一个表示"无"的对象，转为数值时为0；`undefined`是一个表示"无"的原始值，转为数值时为`NaN`。  
  
    2.当声明的变量还未被初始化时，变量的默认值为`undefined`；`null`用来表示尚未存在的对象，常用来表示函数企图返回一个不存在的对象。

2.`undefined`出现的情况：

    （1）变量未被声明或者被声明了但没有赋值时，就等于undefined；
    （2) 调用函数时，应该提供的参数没有提供，该参数等于undefined；
    （3）对象没有赋值的属性，该属性的值为undefined；
    （4）函数没有返回值时，默认返回undefined。

3.关于`null`：

    一.在下列场景中应当使用`null`

        1.用来初始化一个变量，这个变量可能赋值为一个对象；
        2.用来和一个已经初始化的变量比较，这个变量可以是也可以不是一个对象；
        3.当函数的参数期望是对象时，用作参数传入；
        4.当函数的返回值期望是对象时，用作返回值传出。
        例如1：
        var person = null;
        
        例如2：
        function getPerson() {
            if (condition) {
                return new Person("Nicholas");
            } else {
                return null;
            }
        }
        
        例如3：
        var person = getPerson();//已经初始化的变量，也可以不是对象
        if (person !== null) {
            doSomething();
        }
    
    二.在下列场景中不要使用null
    
        1.不要用null来检测是否传入了某个参数；
        2.不要用null来检测一个未初始化的变量。
        例如1：
        var person;//用来和未初始化的变量比较。
        if (person != null) {
            doSomething();
        }
        
        例如2：
        function doSomething(arg1, arg2, arg3, arg4) {
            if (arg4 != null) {
                doSomethingElse();
            }
        }
    
4.关于`undefined`：(有一个令人困惑的地方就是：null == undefined 结果是true)

    //一种非常不好的写法
    var person;
    console.log(person === undefined);//true
    
    尽管这段代码能正常工作，但我建议避免在代码中使用undefined。这个值常常和返回“undefined”的typeof运算符混淆。
    事实上，不管值是undefined的变量或者未声明的变量，typeof的运算结果都是“undefined”。比如：
    
    //foo未被声明
    var person;
    console.log(typeof person);//"undefined"
    console.log(typeof foo);//"undefined"
    
    通过禁止使用特殊值undefined，可以有效地确保只在一种情况下typeof才回返回“undefined”：
    当变量未声明时，如果你使用了一个可能赋值为一个对象的变量时，则将其赋值为null。例如：
    
    ▲▲//好的做法
    var person = null;
    console.log(person === null); //true
    
    将变量初始值赋值为null表明这个变量的意图，它最终很可能赋值为对象。typeof运算符运算null的类型时返回“object”，这样就可以和undefined区分开了。

## 3.new操作符具体干了什么呢?

    1、创建一个空对象，并且 this 变量引用该对象，同时还继承了该函数的原型；
    2、属性和方法被加入到 this 引用的对象中；
    3、新创建的对象由 this 所引用，并且最后隐式的返回 this。

## 4.js的延迟加载的方式有哪些？

如果没有js延迟加载，在浏览器继续解析页面之前，浏览器会立即读取并执行脚本。

【补：window.onload = function() {} 是等待文档加载完成后才加载执行的脚本 】
    
    defer：规定直到页面加载为止才执行脚本。只支持IE。
    <script type="text/javascript" defer="defer"></script>
    
    async：规定一旦脚本可用，则会异步执行（当页面继续进行解析时，脚本将被执行）。仅适用于外部脚本，顺序无序。
    <script type="text/javascript" src="demo_async.js" async="async"></script>
    
    动态创建DOM方式(创建script，插入到DOM中，加载完毕后callBack):
        function loadScript(url, callback) {
    	var script = document.createElement("script");
    	script.type = "text/javascript";
    	if (script.readyState) {
    		// IE
    		script.onreadystatechange = function() {
    			if (script.readyState == "loaded" || script.readyState == "complete") {
    				script.onreadystatechange = null;
    
    				callback();
    			}
    		};
    	} else {
    		// Others: Firefox, Safari, Chrome, Opera
    		script.onload = function() {
    			callback();
    		};
    	}
    	script.src = url; //这里用js地址引入js
    	document.body.appendChild(script);
        }

## 5.如何解决跨域问题? 
概念：只要协议、域名、端口有任何一个不同，都被当作是不同的域。对于端口和协议的不同，只能通过后台来解决。

    跨域资源共享（CORS）
    
    通过jsonp跨域
    
    通过修改document.domain来跨子域
    
    使用window.name来进行跨域
    
    使用HTML5的window.postMessage方法跨域
    
具体参见：[[详解js跨域问题]](https://segmentfault.com/a/1190000000718840)


## 6.documen.write和 innerHTML的区别

    document.write是直接将内容写入页面的内容，会导致页面全部重绘
    
    innerHTML将内容写入某个DOM节点，不会导致页面全部重绘，更精确的控制要刷新页面的那一个部分。（推荐使用）

## 7.call() 和 .apply() 的区别和作用？ ==写到这里，看一下这个链接==

作用：动态改变某个类的某个方法的运行环境。

区别参见：[JavaScript学习总结（四）function函数部分][3]

https://segmentfault.com/a/1190000000660786#articleHeader15



哪些操作会造成内存泄漏？
------------

    内存泄漏指任何对象在您不再拥有或需要它之后仍然存在。
    垃圾回收器定期扫描对象，并计算引用了每个对象的其他对象的数量。如果一个对象的引用数量为 0（没有其他对象引用过该对象），或对该对象的惟一引用是循环的，那么该对象的内存即可回收。
    
    setTimeout 的第一个参数使用字符串而非函数的话，会引发内存泄漏。
    闭包、控制台日志、循环（在两个对象彼此引用且彼此保留时，就会产生一个循环）

详见：[详解js变量、作用域及内存][4]

https://segmentfault.com/a/1190000000687844

JavaScript中的作用域与变量声明提升？
-----------------------

详见：[详解JavaScript函数模式][5]

https://segmentfault.com/a/1190000000758184#articleHeader5


如何判断当前脚本运行在浏览器还是node环境中？
------------------------

    通过判断Global对象是否为window，如果不为window，当前脚本没有运行在浏览器中

你都使用哪些工具来测试代码的性能？
-----------------

    Profiler, JSPerf（http://jsperf.com/nexttick-vs-setzerotimeout-vs-settimeout）, Dromaeo
