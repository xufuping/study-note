> 目录
>
> 1. console.trace()
>
> 2. `typeof` `Object.prototype.toString` `instanceof`
>
> 3. 内核

## 1.console.trace()

**console.trace()函数**用来打印函数调用的栈信息

    function doTask(){
        doSubTask(1000,10000);
    }
 
    function doSubTask(countX,countY){
        for(var i=0;i<countX;i++){
            for(var j=0;j<countY;j++){} 
        }
        console.trace();
    }
    doTask();
    
控制台就是这样的：

![image](http://files.jb51.net/file_images/article/201412/20141229102759864.png?2014112910288)

## 2.`typeof` `Object.prototype.toString` `instanceof`
类型|结果
---|---
Undefined|"undefined"
Null|"object"
Boolean|"boolean"
Number|"number"
String|"string"
Symbol（ECMAScript 6 新增）|"symbol"
函数对象|"function"
任何其他对象|"object"

使用 typeof 来判断数据类型，只能区分基本类型，即 `number`，`string`，`undefined`，`boolean`，`object` 五种。要想区别对象、数组、函数单纯使用 `typeof` 是不行的，`JavaScript`中,通过`Object.prototype.toString`方法，判断某个对象值属于哪种内置类型。

    console.log(Object.prototype.toString.call(123)) //[object Number]
    console.log(Object.prototype.toString.call('123')) //[object String]
    console.log(Object.prototype.toString.call(undefined)) //[object Undefined]
    console.log(Object.prototype.toString.call(true)) //[object Boolean]
    console.log(Object.prototype.toString.call({})) //[object Object]
    console.log(Object.prototype.toString.call([])) //[object Array]
    console.log(Object.prototype.toString.call(function(){})) //[object Function]
    
`instanceof` 运算符用来测试一个对象在其原型链中是否存在一个构造函数的 `prototype` 属性。`instanceof` 运算符用来检测 `constructor.prototype` 是否存在于参数 `object` 的原型链上。

    obj instanceof Object;
    
    例子：
    // 定义构造函数C
    function C(){} 
    var o = new C();
    o instanceof C; // true，因为 Object.getPrototypeOf(o) === C.prototype
    
    检测数组和对象：
    （要明确 Array 是 object 的子类）
    var a = [];
    console.log(a instanceof Array); // true
    console.log(a instanceof Object); // true
    
    var a = {};
    console.log(a instanceof Array); // false
    console.log(a instanceof Object); // true

## 3.内核
浏览器的内核是分为两个部分的，一是渲染引擎，另一个是JS引擎。现在JS引擎比较独立，内核更加倾向于说渲染引擎。

1.**Trident内核**：代表作品是IE，因IE捆绑在Windows中，所以占有极高的份额，又称为IE内核或MSHTML，此内核只能用于Windows平台，且不是开源的。

代表作品还有腾讯、Maxthon（遨游）、360浏览器等。但由于市场份额比较大，曾经出现脱离了W3C标准的时候，同时IE版本比较多，存在很多的兼容性问题。

2、**Gecko内核**：代表作品是Firefox，即火狐浏览器。因火狐是最多的用户，故常被称为firefox内核它是开源的，最大优势是跨平台，在Microsoft Windows、Linux、MacOs X等主要操作系统中使用。

3、**Webkit内核**：代表作品是Safari、曾经的Chrome，是开源的项目。

4、**Presto内核**：代表作品是Opera，Presto是由Opera Software开发的浏览器排版引擎，它是世界公认最快的渲染速度的引擎。在13年之后，Opera宣布加入谷歌阵营，弃用了Presto。

5、**Blink内核**：由Google和Opera Software开发的浏览器排版引擎，2013年4月发布。现在Chrome内核是Blink。谷歌还开发了自己的JS引擎，V8，使JS运行速度极大地提高了。