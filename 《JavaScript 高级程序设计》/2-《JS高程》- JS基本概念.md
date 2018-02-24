# 1.语法，关键字和保留字，变量

## 1.1语法

1.ECMAScript区分大小写；

2.标识符(变量、函数、属性的名字或者函数参数)：第一个字符必须是字母、下划线或一个美元符号($)，其他字符可以是字母、下划线、美元符号或数字(建议采用驼峰命名)；

3.严格模式：为JS定义了一种不同额度解析与执行模型，对ES3的一些不确定行为进行处理，对某些不安全操作也会抛出错误。启用严格模式，在脚本顶部添加代码："use strict"。

4.语句：分号结尾，使用代码快写法在流控制语句等语句中使用{}

## 1.2关键字和保留字

不要使用关键字和保留字作为标识符和属性名，以便与将来的ECMAScript版本兼容。常见的有：

    关键字:
    break,do,typeof,case,else,new,var
    continue,for,switch,while,function
    this,with,default,if,throw,delete,try
    保留字:
    abstract,int,short,boolean,export
    static,byte,long,super,char,final
    class,float,throws,const,goto,private
    debugger,protected,double,import,public
    

## 1.3变量

ES变量是松散类型的，可以保存任何类型的数据。

    var message = "hi";

# 2.数据类型

## 2.1 typeof操作符
作用：检测给定变量的数据类型：

    值未定义——"undefined"
    值是布尔值——"boolean"
    值是字符串——"string"
    值是数值——"number"
    值是对象或null——"object"(null被认为是一个空的对象引用)
    值是函数——"function"
    示例：
    var message = "some string";
    console.log(typeof message); // "string"
▲typeof是一个操作符，不是一个函数。

## 2.2 Undefined类型
未被初始化和未被声明的对象，都会返回undefined。

    var message;
    //var age;
    console.log(message); // "undefined"
    console.log(age); // 产生错误

## 2.3 Null类型
空对象

    ▲推荐写法：
    如果定义的变量准备将来用于保存对象，可以把变量初始值设为null。
    这样没有被声明的变量返回undefined，没有被初始化的变量返回null。
    //var message;
    var age = null;
    console.log(message); // undefined
	console.log(age); // null
	console.log(typeof age); // object

## 2.4 Boolean类型
true和false区分大小写。下面是Boolean转换：

    数据类型            转化为true的值             转换为false的值
    Boolean             true                        false
    String              非空字符串                  ""（空字符串）
    Number              非零数字（包括无穷大）      0和NaN
    Object              任何对象                    bull
    Undefined           n/a【不适用】               undefined
对任何数据类型的值调用Boolean()函数，总会返回一个Boolean值。Boolean转换对理解流控制语句很重要，下面是各种数据类型及其对应的转换规则：

    var message = "hello!";
    if (message){
        alert("这是一个非空字符串");
    } else {
        alert("这是一个空字符串");
    } // 这是一个非空字符串，因为message里有字符串。

## 2.5 Number类型
提示：不要测试特定的浮点数值，它们计算会产生舍入误差，这是基于IEEE754数值的浮点计算的通病；

### 2.5.1 NaN(非数值，Not a Number)

    isNaN()函数接受一个参数，确定这个参数是否“不是数值”。
    console.log(isNaN(NaN)); // true
    console.log(isNaN(10)); // false(10是一个数值)
    console.log(isNaN("10")); // false(数字字符串可以被转换成数值10)
    console.log(isNaN("blue")); // true(这个字符串不能转换成数值)
    console.log(isNaN(true)); // false(true可以被转换成数值1)
    
### 2.5.2 数值转换
#### 2.5.2.1 Number()用于任何数据类型数值转换
    1. 如果是Boolean值，true和false将分别被转换为1和0；
    2. 如果是数字值，传入和返回；
    3. 如果是null值，返回0
    4. 如果是undefined，返回NaN；
    5. 如果是字符串：
        (1). 如果字符串只有数字，权当十进制并转换为十进制，例如“011”转换为“11”，忽略前导0； 
        (2). 如果字符串只有浮点数，转换为浮点数，忽略前导0；
        (3). 如果有十六进制数转换为相同大小的十进制数值，例如“0xf”转换为“15”；
        (4). 如果字符串为空，将其转换为0，例如“ " " ”转换为“0”；
    6. 如果是对象，则调用valueOf()方法，然后按照前面的规则转换返回的值。
       如果转换结果为NaN，则调用对象的toString()方法，然后再次按照前面的规则转换返回的字符串值。
    
    示例：
    var num1 = Number("hi");
	var num2 = Number(" ");
    var num3 = Number("011");
    var num4 = Number("true");
    var num5 = Number("undefined");
    var num6 = Number("null");
    var num7 = Number("0xD");
    console.log(num1,num2,num3,num4,num5,num6,num7);
    //NaN,0,11,true,NaN,0,13
    
#### 2.5.2.2 parseInt()用于字符串数值转换为整数
parseInt()可以识别各种整数格式：

    var num1 = parseInt("1234blue");
    var num2 = parseInt(" ");
    var num3 = parseInt("0xA",16);
    var num4 = parseInt("70",8); // ES5和严格模式不具有解析八进制能力，必须写明基数才行
    var num5 = parseInt("11",2);
    var num6 = parseInt("22.5");// 小数点后面的数字被省略掉
    var num7 = parseInt("15"); // 默认十进制转换
    console.log(num1,num2,num3,num4,num5,num6,num7);
    // 1234,NaN,10,56,3,22,15

#### 2.5.2.3 parseFloat()用于字符串数值转换为小数
parseFloat()只解析十进制数值：

    var num1 = parseFloat("1234blue");
    var num2 = parseFloat("0xA");
    var num3 = parseFloat("22.5");
    var num4 = parseFloat("22.34.5");
    var num5 = parseFloat("098.5");
    var num6 = parseFloat("3.125e7");
    console.log(num1,num2,num3,num4,num5,num6);
    //1234 0 22.5 22.34 98.5 31250000

## 2.6 String类型
字符串，建议采用双引号，也可以用单引号。

### 2.6.1 字符字面量
    \n 换行
    \t 制表
    \b 空格
    \r 回车
    \f 进纸
    \\ 斜杠
    \' 单引号
    \" 双引号
    
### 2.6.2 转换为字符串
toString()方法和String()方法:

    var num = 10;
    console.log(num.toString()); // "10"
    console.log(num.toString(2)); // "1010"
    console.log(num.toString(8)); // "12"
    console.log(num.toString(16)); // "a"
    
    var value = true;
    console.log(value.toString()); // "true"
    
    var value = null;
    console.log(value.toString()); 
    // Uncaught TypeError: Cannot read property 'toString' of null
    null和undefined没有toString()方法，但是String()函数不仅有toString()方法的效用，还可以返回这两个值的字面量。
    var value1 = null;
    var value2 = undefined;
    console.log(String(value1)); // "null"
    console.log(String(value2)); // "undefined"

## 2.7 Object类型
new操作符创建一个对象

    var o = new Object();

Object有下列属性和方法：
    
    1.hasOwnProperty(propertyName)：检查给定的属性是否在当前对象中。
        var o = new Object();
	    o.name = "xushuai";
	    console.log(o.hasOwnProperty("name")); // true
	2.isPrototypeof(object):检查传入的对象是否是另一个对象的原型；
	3.propertyIsEnumerable(propertyName)：检查给定的属性是否可以用for-in语句；
	4.toString():返回对象的字符串表示；
	5.valueOf()：返回对象的字符串、数值或布尔值表示，很多时候与toString()方法返回值相同。

# 3.操作符
【一元操作符（++，--，+，-,+=,-=）、位操作符（~，&，|，^，<<,>>,>>>）、乘性操作符（*，/，%）、加性操作符(+,-)、关系操作符（<,>,=<,>=）略过】
## 3.1 布尔操作符

### 3.1.1 逻辑非(!)
    1.如果操作数是一个对象，返回false；
    2.如果操作数是一个空字符串，返回true；如果操作数是一个非空字符串，返回false；
    3.如果操作数是数值0，返回true；如果操作数是任意非0数值（包括Infinity），返回false;
    4.如果操作数是null,NaN,undefined,都返回true。
    示例：
    console.log(!false);  // true
    console.log(!"");     // true
    console.log(!"blue"); // false
    console.log(!0);      // true
    console.log(!12345);  // false
    console.log(!NaN);    // true
    
### 3.1.2 逻辑与(&&)
    1. 如果第一个操作数是对象或者两个操作数都是对象，都返回第二个操作数；
    2. 如果有一个操作数是null，返回null;
       如果有一个操作数是NaN，返回NaN;
       如果有一个操作数是undefined，返回undefined;
       如果有一个操作数是false，返回false.

### 3.1.3 逻辑或(||)
    1. 如果第一个操作数是对象或者两个操作数都是对象，返回第一个操作数；
    2. 如果第一个操作数的求值结果为false，则返回第二个操作数；
    3. 如果有两个操作数是null，返回null;
       如果有两个操作数是NaN，返回NaN;
       如果有两个操作数是undefined，返回undefined;
       如果有一个操作数是true，返回true.

## 3.2 相等操作符、赋值操作符

### 3.2.1 相等与不相等
== 和 != ：

    1. 如果有一个操作数是布尔值，比较相等性前自动转换为数值，false转换为0，true转换为1；
    2. 如果一个操作数是字符串，另一个操作数是数值，在比较相等行之前自动将字符串转换为数值；
    3. 如果一个操作数是对象，另一个操作数不是，自动调用对象的valueOf()方法，用得到的基本类型值按照前面的规则进行比较。
    示例：
    null == undefined //true //就是true，记住就好了，哈哈
    NaN == NaN //false  //非数不等于非数
    NaN != NaN //true
    false == 0;true == 1 //true //false转换后是0，true转换后是1
    "5" == 5 //true //自动转换字符串为数值

### 3.2.2 全等与不全等
=== 和 !==,▲全等仅限在两个操作数未经转换就相等的情况下返回true，不全等在两个操作数未经转换就不相等的情况下返回true。例如：

    "55" == 55; //true,因为转换后相等
    "55" === 55; //false,因为不同的数据类型不相等
    "55" != 55; //false,因为转换后相等
    "55" !== 55; //true,因为不同的数据类型不相等
    null == undefined; //true
    null === undefined; //false,因为他们是不同类型的值
    PS:由于相等和不相等操作符存在类型转换问题，而为了保持代码中数据类型的完善性，我们推荐使用全等和不全等操作符。

### 3.2.3 赋值操作符
    var num = 10;
    var name = "徐少"；

## 3.3 条件操作符、逗号操作符
    条件操作符：
    variable = boolean_expression ? true_value : false_value;
    示例：
    var max = (num1 > num2) ? num1 : num2;
    //如果num1>num2，返回true，num1赋值给max;否则返回false，num2赋值给max。
    
    逗号操作符：
    用逗号操作符声明多个变量，在一条语句中执行多个操作；逗号操作符总是返回表达式中的最后一项。
    var num1 = 1,num2 = 2,num3 = 3; //一条语句执行多个操作
    var num = (5, 1, 0); //num的值为0 

# 4.流控制语句
## 4.1 if语句

    if (i > 25) {
    	console.log("大于25");
    } else if (i < 0) {
    	console.log("小于0");
    } else {
    	console.log("0-25之间");
    }

## 4.2 do-while语句，while语句
do-while语句

    do {
        statement
    } while (expression);
    示例：
    var i = 0;
    do {
        i += 2;
    } while (i < 10);
    console.log(i); // 10

while语句

    while (expression) {
        statement
    }
    示例：
    var i = 0;
    while (i < 10) {
        i += 2;
    }
    console.log(i); // 10

## 4.3 for语句
    for (initialization; expression; post-loop-expression) {
        statement
    }
    示例：
    var count = 10;
    var i = 0;
    for (; i < count;) {
        console.log(i);
        i++;
    }

## 4.4 for-in语句
for-in语句是一种精准的迭代语句，可以用来枚举对象的属性。

    for (property in expression){
        statement
    }
    示例：
    for (var propName in window) {
        document.write();
    }
    //循环显示BOM中window对象的所有属性
    如果要迭代的对象的变量值为null或undefined，for-in语句不会执行循环。使用for-in前先检测对象的值不是null或undefined。

## 4.5 break和continue语句
break跳出整个大循环，continue跳出当次循环后继续执行。

    break示例：
    var num = 0;
    for (var i=1; i < 10; i++) {
    	if (i % 5 == 0) {
    		break;
    	}
    	num++;
    }
    console.log(num); // 4
    
    continue示例：
    var num = 0;
    for (var i=1; i < 10; i++) {
    	if (i % 5 == 0) {
    	    continue;
    	}
    	num++;
    }
    console.log(num); // 8

## 4.6 switch语句
switch比较值时使用的是全等操作符，因此不会发生类型转换。（例如，"10"不等于10）

    switch (expression) {
    	case value: statement
    		break;
    	case value: statement
    		break;
    	case value: statement
    		break;
    	default: statement
    }
    case的含义是：如果表达式等于这个值(value)，则执行后面的语句(statement);
    break关键字会导致代码执行流跳出switch;
    省略break，执行完当前case后，就会继续执行下一个case;
    default: 表达式不匹配前面任何一种情形的时候，执行这里的代码。
    示例：
    switch (num) {
    	case 1: 
    		console.log("我是小明");
    		break;
    	case 2: 
    		console.log("我是小红");
    		break;
    	case 3: 
    		console.log("我是小东");
    		break;
    	default: 
    		console.log("你的数字不是1、2、3哟");
    }

## 4.7 label语句和with语句
label语句可以咋子代码中添加标签，以便将来使用：label:statement

    start: for(var i=0; i < count; i++) {
        console.log(i);
    }
    //这个start标签可以将来由break和continue语句引用，加标签的语句一般都与for语句等循环语句配合使用。
    
with语句：为了简化多次编写同一个对象的工作。【不建议使用with语句】

# 5.函数
    function functionName(arg0) {
        statements
    }
ES中任何函数定义时不必指出是否返回值，但后面随时可跟return语句返回值。函数执行完return后停止并立即退出，因此位于return之后的任何代码永远不会执行。

    function sum(num1, num2) {
        return num1 + num2;
    }
    var result = sum(5, 10);
    建议：要么让函数始终都返回一个值，要么永远都不要返回值，不然时而返回时而不返会给调试代码带来不便。
    
## 5.1 参数
(ES参数与其他语言有些不同，不介意传进来多少个或者什么数据类型，比如只传递两个参数，但可以调用任意个参数。)因为：ES中的参数在内部是用一个数组来表示的。实际上，在函数体内可以通过arguments对象来访问这个参数数组，从而获取传递给函数的每一个参数。

    通过访问arguments对象的length属性可以获知传递了多少个参数：
    //开发人员利用这点让函数能够接受任意个参数并分别实现适当功能，这是一个不太完美的重载
    function howMany() {
        console.log(arguments.length);
    }
    howMany(11, "xushao", abc); // 3
    howMany(); // 0
    
    arguments 对象可以与命名参数一起使用：
    function doAdd(num1, num2) {
        if(arguments.length == 1) {
            console.log(num1 + 10);
        } else if (arguments.length == 2) {
            console.log(arguments[0] + num2);
        }
    }//如果参数长度为1，执行第一个if；如果为2，执行else if。这里执行else if，这里的arguments[0] = num1.
    
    arguments的值永远与对应命名参数的值保持同步：
    function doAdd(num1, num2) {
        arguments[1] = 10;
        sum = arguments[0] + num2;
        console.log(sum);
    }//这里修改了arguments[1]也就是修改了num2。不过这里不是说读取这两个值都会访问相同的空间，他们的内存空间是独立的，但它们的值会同步。但这种影响是单向的，修改命名参数不会改变arguments的值。
    //同时，如果只传入一个参数，那么为arguments[1]设置的值不会反应到命名参数中。因为arguments对象的长度是由传入的参数个数决定的，不是由定义函数时的命名参数的个数决定的。
PS:ES中所有参数传递的都是值，不可能通过引用传递参数。

## 5.2 没有重载
▲ES不能像传统意义上那样实现重载。但是可以通过检查传入函数中参数的类型和数量并作出不同的反应，可以模仿方法的重载。


