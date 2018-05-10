> 目录结构
>
> 1.JS变量声明提升？
>
> 2.null和undefined的区别？

## 1.JavaScript中的作用域与变量声明提升？
函数声明的行为并不等同于命名函数表达式，其区别在于提升行为，看下面：

    //全局函数
	function foo(){
	    console.log("global foo!-1");
	}
	function bar(){
	    console.log("global bar-2");
	}
		 
    function hoist(){
		 console.log(type  foo);//function
		 console.log(type  bar);//undefined
		     
		 foo(); //local foo!-2
		 bar(); //TypeError: 'undefine is not a function  

		 //变量foo以及实现者被提升
		 function foo(){
		     console.log("local foo!-2");
		 }
		     
		 //仅变量bar被提升，函数实现   并未被提升
		 var bar = function(){
		        console.log("local bar!-2");
		 };
    }
    hoist();	
对于所有变量，无论在函数体的何处进行声明，都会在内部被提升到函数顶部。而对于函数通用适用，其原因在于函数只是分配给变量的对象。

`提升`，顾名思义，就是把下面的东西提到上面。在`JS`中，就是把定义在后面的东西（变量或函数）提升到前面中定义。 从上面的例子可以看出，在函数`hoist`内部中的`foo`和`bar`移动到了顶部，从而覆盖了全局`foo`和`bar`函数。局部函数`bar`和`foo`的区别在于，`foo`被提升到了顶部且能正常运行，而`bar()`的定义并没有得到提升，仅有它的声明被提升，所以，当执行`bar()`的时候显示结果为`undefined`而不是作为函数来使用。

详见：[详解JavaScript函数模式](https://segmentfault.com/a/1190000000758184#articleHeader5)

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