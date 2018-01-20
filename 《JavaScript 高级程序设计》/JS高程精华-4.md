> 目录
>
> 1.函数表达式
> 
> > 1.1 递归
> >
> > ▲▲▲1.2 闭包
> >
> > > 1.2.1 闭包与变量
> > >
> > > 1.2.2 关于this对象
> > >
> > > 1.2.3 内存泄漏	
> >
> > ▲ 1.3 模仿块级作用域
> >
> > 1.4 私有变量
> > >
> > > 1.4.1 静态私有变量

### 1.函数表达式
定义函数的方式有两种：一种是函数声明，另一种是函数表达式。

关于函数声明，它的一个重要特征就是函数声明提升，在执行代码之前会先读取函数声明，这意味着可以把函数声明放在调用它的语句后面。

    sayHi();
    function sayHi() {
      console.log("Hi!");  
    };

关于函数表达式，创建一个函数将它赋值给某个变量，这种情况下创建的函数叫做==匿名函数==。

    var functionName = function (arg0, arg1) {
      // 函数主体  
    };

#### 1.1 递归
递归是一个函数通过名字调用自身情况下构成的。

`arguments.callee`是一个指向正在执行的函数的指针，因此可以用它来实现对函数的递归调用，同时它可以实现递归调用时的松耦合。

    function factorial(num) {
        if (num <= 1) {
            return 1;
        } else {
            // 另一种写法：return num * factorial(num-1); 
            return num * arguments.callee(num-1);
        }
    }
    
    var anotherFactorial = factorial;
    factorial = null;
    // 如果使用另一种写法，下面则报错，因为factorial已经不再是函数
    console.log(anotherFactorial(4)); // 24
    
#### ▲▲▲ 1.2 闭包
闭包是指有权访问另一个函数作用域中的变量的函数。（所以创建闭包的常见方式，就是在一个函数内部创建另一个函数。）

一般的函数调用执行：当某个函数被调用时，会创建一个执行环境以及相应的作用域链。然后，使用arguments和其他命名参数的值来初始化函数的活动对象。后台的每个执行环境都有一个变量对象（全局环境的变量对象始终存在，而局部环境的变量对象则只在函数执行的过程中存在）。所以，==无论什么时候在函数中访问一个变量，都会从作用域链中搜索具有相应名字的变量。一般来说，当函数执行完毕后，局部活动对象就会被销毁，内存中仅保存全局作用域(全局执行环境的变量对象)==。

    function compare(value1, value2) {
        if (value1 < value2) {
            return -1;
        } else if (value1 > value2) {
            return 1;
        } else {
            return 0;
        }
    }
    
    var result = compare(5, 10);
    
    以上代码先定义了compare()函数，然后在全局作用域中调用了它。
    在调用时，会创建一个包含arguments、value1和value2的活动对象。
    就compare()函数的执行环境而言，其作用域链中包含两个变量对象：本地活动对象和全局变量对象。
    
▲ 闭包的执行调用：在函数内部定义的函数会将包含函数(即外部函数)的活动对象添加到它的作用域链中。外部函数执行完毕后，它的活动对象也不会被销毁，因为内部内部函数的作用域链仍然在引用这个活动对象(换句话说，当外部函数返回之后，其执行环境的作用域链会被销毁，但它的活动对象仍然会留在内存中，直到内部函数被销毁后，外部函数的活动对象才会被销毁——比如手工解除对内部函数的引用)。【所以，由于闭包会携带包含它的函数的作用域，因此会比其他函数占用更多的内存，过度使用闭包可能会导致内存占用过多，所以要慎重使用闭包】。

##### 1.2.1 闭包与变量
闭包只能取得包含函数中任何变量的最后一个值，闭包所保存的是整个变量对象，而不是某个特殊的变量。

    例如：
    function createFunction () {
		var result = new Array();

		for (var i = 0; i < 10; i++) {
		    // 定义了一个匿名函数，并将立即执行该匿名函数的结果赋给数组，result数组中的每个函数都有自己num变量的一个副本。
			result[i] = function (num) {
			    // 在result数组中创建另一个匿名函数强制让闭包的行为符合预期。
				return function () {
					return num;
				};
			}(i); // 在每次调用匿名函数时我们传入了变量i
			// 函数参数按值传递，会将变量i的当前值复制给参数num
		}
		
		return result;
	}
	
##### 1.2.2 关于this对象
匿名函数的执行环境有全局性，其`this对象`通常指向`window`。不过，==每个函数在被调用时都会自动取得两个特殊变量：this和arguments。== 在闭包中调用`this对象`和`arguments`时，把外部作用域中的`this对象`和`arguments`保存在一个闭包能够访问到的变量里，就可以让闭包访问该对象了。

    var name = "XuShao";

	var obj = {
		name : "XiangShuai",

		// 一般函数引用
		getName : function(){
			return this.name;
		},

		// 闭包调用时，不转换this对象
		getNameFunc1 : function () {
			return function () {
				return this.name;
			};
		},

		// 闭包调用时，转换this对象
		getNameFunc2 : function () {
			 // 定义匿名函数之前，把this对象值赋给that对象，这样定义闭包后，闭包也可以访问这个变量了
			var that = this;
			return function () {
				return that.name;
			};
		}
	};
	console.log(obj.getName()); // "XiangShuai"
	console.log(obj.getNameFunc1()()); // "XuShao"
	console.log(obj.getNameFunc2()()); // "XiangShuai"

##### 1.2.3 内存泄漏
如果闭包的作用域链保存了一个HTML元素，那么该元素无法被销毁。

    <div id="someElement">点击我</div>
    
    function assignHandler() {
		var element = document.getElementById("someElement");

		// element.onclick = function () {
		// 	console.log(element.id);
		// };

            // 我们使用垃圾回收机制接触引用让值脱离执行环境，以便垃圾收集器下次回收，确保正常回收其占用的内存。
            // 把element.id的一个副本保存在一个变量中，在闭包中引用该变量消除了循环引用。
		var id = element.id; 
		element.onclick = function () {
			console.log(id);
		}
		// 闭包会引用包含函数的整个活动对象，其中会至少保存一个引用，所以这里有必要把element设置为null,解除对DOM对象的引用。
		element = null;
	}

	assignHandler();
	
#### ▲ 1.3 模仿块级作用域
JS没有`块级作用域`的概念，但是可以用`匿名函数`来模仿`块级作用域`(也称`私人作用域`)。

例：立即执行的匿名函数

    (function () {
	    // 这里是块级作用域
	})(); // 将函数声明包含在一对圆括号中，表示它实际上是一个函数表达式。
	
只是临时需要一些变量，就可以使用`私有作用域`，在`匿名函数`中定义的任何变量，都会在执行结束时被销毁：

    var len = 5;

	function outputNumber(count) {
		(function () {
			for (var i = 0; i < count; i++) {
				console.log("我是" + i);
			}
		})();

		console.log(i);
	}

	outputNumber(len);
	// 我是0
	// 我是1
	// 我是2
	// 我是3
	// 我是4
	// i is not defined

推荐使用该技术，一方面可以避免过多的全局变量和函数命名冲突，不必担心搞乱全局作用域；另一方面这种做法可以减少闭包占用的内存问题，因为没有指向匿名函数的引用，只要函数执行完毕，就可以立即销毁其作用域链了。

    (function () {
        var now = new Date();
        if (now.getMonth() == 0 && now.getDate() == 1) {
            console.log("Happy new year!");
        }
    })();

#### 1.4 私有变量
任何在函数中定义的变量，都可以认为是私有变量，因为不能在函数的外部访问这些变量。私有变量包括函数的参数、局部变量和在函数内部定义的其他函数。

有权访问私有变量和私有函数的公有方法称为==特权方法==。有两种在对象上创建特权方法的方式。一种是在构造函数中定义特权方法，但构造函数模式的缺点是针对每个实例都会创建同样一组新方法，所以使用==静态私有变量==实现特权方法就可以避免这个问题。

##### 1.4.1 静态私有变量
在私有作用域(块级作用域)中定义私有变量或函数可以创建特权方法：

    (function () {
    
        // 变量name是一个静态的、由所有实例共享的属性。
		var name = "";
		
		// 初始化未经声明的变量，总是会创建一个全局变量。
		// 所以Person就成了一个全局变量，能够在私有作用域之外被访问到。
		Person = function (value) {
			name = value;
		};

		Person.prototype.getName = function () {
			return name;
		};

		Person.prototype.setName = function (value) {
			name = value;
		};
	})();

	var person1 = new Person("Nicholas");
	console.log(person1.getName()); // "Nicholas"
	
	person1.setName("Greg");
	console.log(person1.getName()); // "Greg"

    // 调用setName()或新建一个Person实例都会赋予name属性一个新值
	var person2 = new Person("XuShao");
	console.log(person1.getName()); // "XuShao"
	console.log(person2.getName()); // "XuShao"
	
	person1.setName("Greg");
	console.log(person1.getName()); // "Greg"
	console.log(person2.getName()); // "Greg"

附：多查找作用域链中的一个层次，就会在一定程度上影响查找速度。这正是使用闭包和私有变量的一个不足之处。