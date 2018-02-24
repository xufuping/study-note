# 1.语法
## 1.1 枚举(for in)

    for (myvar in obj) {
        if (obj.hasOwnProperty(myvar)) {
            ...
        }
    }
    
## 1.2 运算符优先级

运算符 | 作用
---|---
. [] () | 提取属性与调用函数
delete new typeof + - ! | 一元运算符
* / % |乘法、除法、求余
+ - | 加法/连接、减法
>= <= > < |不等式运算符
=== !== |等式运算符
&& |逻辑与
\|\| |逻辑非
?: |三元

# 2.对象
## 2.1 对象简述
JS的简单数据类型包括`数字`、`字符串`、`布尔值`(true和false)、`null`和`undefined`。其它所有值都是对象。JS中，`数组`、`函数`、`正则表达式`和`对象`都是对象。

JS的对象是`无类型的`，它对新属性的名字和属性的值没有限制，对象适合用于`汇集和管理数据`，对象`可以包含其他对象`，所以它们可以容易地表示成树状或图形结构。

## 2.2 对象字面量

    1.一个对象字面量就是包围在一个花括号中的零或多个“名/值”对
    2.不强制要求用引号括住属性名，但必须用引号括住值
    3.逗号分隔多个“名/值”对
    4.对象可嵌套
    
    var XuQingfeng = {
        appearance:"handsome",
        age:"20",
        hobby:{
            motion:"swimming",
            food:"prawns"
        }
    };

## 2.3 检索

    1.可以采用在[]后缀中括住一个字符串表达式检索对象里包含的值，也可以用.表示法。
    stooge["first-name"]  //"xu"
    flight.departure.last-name  //"qingfeng"
    
    2. || 运算符可以用来填充默认值
    var status = flight.status || "there is none"
    
## 2.4 引用
对象通过引用来传递。它们永远不会被复制。

    var x = stooge;
    x.name = "shuai";
    var Qingfeng = stooge.name;
    // 因为x和stooge是指向同一个对象的引用，所以Qingfeng为“shuai”。

## 2.5 原型
每个对象都会链接到一个原型对象，并且它可以从中继承属性。所有通过对象字面量创建的对象都连接到Object.prototype,它是JS中的标配对象。

原型链接只有在检索值的时候才被用到。如果我们尝试去获取对象的某个属性值，但是该对象没有此属性名，那么JS会试着从原型对象中获取属性值。如果那么原型对象也没有该属性，那么再从它的原型中寻找，依此类推，直到该过程最后达到达终点==Object.prototype==。如果想要的属性完全不存在于原型链中，那么结果就是undefined值。这个过程称为==委托==。 

原型关系是一种动态的关系。如果我们添加一个新的属性到原型中，该属性会立即对所有基于该原型创建的对象可见。

## 2.6 反射
使用typeof操作符检测对象属性:

    typeof Qingfeng.status  // 'number'

请注意原型链中的任何属性都会产生值:

    typeof Qingfeng.toString  // 'function'

有两种方法处理掉这些不需要的属性:

    1.让你的程序检查并丢弃值为函数的属性。
        
        if (typeof Object.Qingfeng !== 'function') {
            ……
        }
        
    2.另一个方法是使用hasOwnProperty方法，如果对象拥有独有的属性，它将返回true。hasOwnProperty方法不会检查原型链。
    
        Qingfeng.hasOwnProperty('number')        // true
        Qingfeng.hasOwnProperty('constructor')   // false

## 2.7 枚举
==for-in==用来便利对象中的所有属性名。但是会列出所有的属性-包括函数和原型中的属性，所以有必要过滤这些你可能不需要的值。常用的过滤器是==hasOwnProperty==方法和==typeof==来排除函数。

    var name;
    for (name in Qingfeng) {
        if (typeof Qingfeng.name !== 'function') {
            document.writeLn(name + ':' + Qingfeng.name);
        }
    }
    
## 2.8 删除
delete运算符可以用来删除对象具有的某个属性。它不会触及原型链中的任何对象，但是删除对象的属性可能会让来自原型链中的属性透现出来：

    Qingfeng.name  // 'xfp'
    
    // 删除 Qingfeng 的 name 属性，从而暴露出原型的 name 属性。
    delete Qingfeng.name;
    
    Qingfeng.name  // 'shuai'

## 2.9 减少全局变量污染
最小化使用全局变量的方法之一是为你的应用只创建一个唯一的全局变量：
    
    var Qingfeng = {};
    
该变量此时变成了你的应用的容器：

    Qingfeng.name = {
        firstName : "Xu",
        lastNanme : "Fuping"
    }

把全局性的资源纳入一个名称空间下，你的程序与其他应用程序、组件或类库之间发生冲突的可能性就会显著降低。当然，也可以使用闭包来进行信息隐藏的方式，它是另一种有效减少全局污染的方法。