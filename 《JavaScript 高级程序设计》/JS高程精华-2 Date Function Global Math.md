>目录
> 
> 1.Date()类型的组件方法
> 
> 2.Function类型
> 
> > 2.1 函数声明与函数表达式
> > 
> > 2.2 作为值的函数
> > 
> > 2.3 函数内部属性
> > 
> > 2.4 函数属性和方法
> 
> 3.单体内置对象
> 
> > 3.1 Global对象
> > 
> > 3.2 Math对象

### 1.Date()类型的组件方法

Date()方法使用实例:

    var myTime = new Date();
	
	console.log(myTime.toLocaleString()); // 2017/10/7 下午4:52:53
	
日期/时间组件方法：

方法 | 说明
---|---
getFullYear() | 取得四位数的年份
getMonth() | 返回日期中的月份，0表示一月，11表示12月
getDate() | 返回日期月份中的天数
getDay() | 返回日期中星期几，0表示星期日，6表示星期六
getHours() | 返回日期中的小时数(0-23)
getMinutes() | 返回日期中的分钟数(0-59)
getSeconds() | 返回日期中的秒数(0-59)
getMilliseconds() | 返回日期中的毫秒数
▲ toLocaleDateString() | 获取当前日期
▲ toLocaleTimeString() | 获取当前时间
▲ toLocaleString() | 获取当前日期与时间

### 2.Function类型

#### 2.1 函数声明与函数表达式

`函数声明提升`：解析器向执行环境加在数据的时候，对函数声明和函数表达式并非一视同仁。==解析器会率先读取函数声明，并使其在执行任何代码之前可用(可以访问)；至于函数表达式，则必须等到解析器执行到它所在的代码行，才会真正被解释执行。==

#### 2.2 作为值的函数

函数名本身就是变量，所以函数也可以作为值来使用。

根据对象数组某个属性排序：

    function createComparisonFunction(propertyName) {

		return function(object1, object2){
			var value1 = object1[propertyName];
			var value2 = object2[propertyName];

			if (value1 < value2) {
				return -1;
			} else if (value1 > value2) {
				return 1;
			} else {
				return 0;
			}
		};
	}

	// 使用上面的函数createComparisonFunction
	var data = [
		    {name:"ZhouShao",
			age:25
			},
			{name:"XuShuai", 
			age:20
			},
			{name:"CaiShao", 
			age:22
			}
	];
	
	data.sort(createComparisonFunction("name"));
	console.log(data); // CaiShao...XuShuai...ZhouShao...
	data.sort(createComparisonFunction("age"));
	console.log(data); // XuShuai...CaiShao...ZhouShao...	

#### 2.3 函数内部属性

虽然arguments的主要用途是保存函数参数，但这个对象还有一个名叫callee的属性，该属性是一个指针，指向拥有这个arguments对象的函数。

阶乘函数例子：

    function factorial(number){
		if (number <=1) {
			return 1;
		} else {
		        // 这里可以是return number * factorial(num-1),
		        // 但是为了不让函数的执行和函数名factorial紧紧耦合在一起，可以使用arguments.callee
			return number * arguments.callee(num-1);
		}
	}

	console.log(factorial(5)); // 5*4*3*2*1=120

this引用的是函数 据以执行的环境对象——或者也可以说是this值(当在网页的全局作用域中调用函数时，this对象引用的就是window)。

#### 2.4 函数属性和方法

▲ 每个函数都包含两个属性：length和prototype。length属性表示函数希望接收的命名参数的个数。对于ES中的引用类型而言，prototype是保存它们所有实例方法的真正所在。

▲ 每个函数都包含两个非继承而来的方法：apply()和call()。它们的用途都是在特定的作用域中调用函数，实际上等于设置函数体内this对象的值。

- apply()方法：接收两个参数，第一个是在其中运行函数的作用域，另一个是参数数组(第二个参数可以是Array实例，也可以是arguments对象)。


    function sum(a, b){
        return a + b;
    }
    
    function callSum1(a, b){
        return sum.apply(this, arguments); // 传入arguments对象
    }
    
    function callSum2(a, b){
        return sum.apply(this, [a, b]); // 传入数组
    }
    
    console.log(callSum1(5, 6)); // 11
    console.log(callSum2(5, 6)); // 11

- call()方法：第一个参数是this值，其余参数都直接传递给函数。


    function sum(a, b){
        return a + b;
    }
    
    function callSum(a, b){
        return sum.call(this, a, b); // 传入arguments对象
    }
    
    console.log(callSum(6, 7)); // 13

▲ apply()和call()真正强大的地方是能够扩充函数赖以运行的作用域。使用call()扩充作用域的最大好处就是对象不需要与方法有任何耦合关系。

    var color = 'purple';
	var o = { color : "yellow" };

	function sayColor(){
		console.log(this.color);
	}

	sayColor(); // purple

	sayColor.call(this); // purple
	sayColor.call(window); // purple
	sayColor.call(o); // yellow

ES5还定义了bind()方法，它会创建一个函数的实例，其this值会被绑定到传给bind()函数的值。

    var color = 'purple';
	var o = { color : "yellow" };

	function sayColor(){
		console.log(this.color);
	}

	var say = sayColor.bind(o); // 把o对象传入给saycolor()并创建了say()函数，所以say()函数的this值等于o。
	say(); // yellow

### 3.单体内置对象

#### 3.1 Global对象

事实上，全局变量或全局函数——所有在全局作用域中定义的属性和函数，都是Global对象的属性。

- `encodeURI()`和`encodeURIComponent()`方法：前者将空格转码为20%，后者将所有非字母数字字符转码为20%。(一般使用后者使用更多)。可以分别使用`decodeURI()`和`decodeURIComponent()`进行解码。


    var uri = "http://www.baidu.com/illegal value.html#start";

	// http://www.baidu.com/illegal%20value.html#start
	console.log(encodeURI(uri));

	// http%3A%2F%2Fwww.baidu.com%2Fillegal%20value.html%23start
	console.log(encodeURIComponent(uri));

-eval()方法：只接受一个参数——ES或JS要执行的字符串。

    eval("console.log('123')"); // '123'

#### 3.2 Math对象


方法 | 作用
---|---
Math.max() | 取得最大值
Math.min() | 取得最小值
Math.ceil() | 执行向上舍入
Math.floor() | 执行向下舍入
Math.round() | 执行标准舍入
▲ Math.random() | 返回大于等于0小于1的随机数

补充：如何向Math.max()/Math.min()传入数组：

    var arr = [1, 2, 3, 4, 5];
    var max = Math.max.apply(Math, arr); // 确定一个数组中的最大值
    
    ES6的写法
    Math.max(...arr);

补充：如何返回某个区间的随机数，`Math.random(): 整数值 = Math.floor(Manth.random() * 可能值的总数 + 第一个可能的值)`。
    
    // 选择一个介于 2 到 10 之间整数书值。
    var num = Math.floor(Math.random() * 9 + 2); // 2~10