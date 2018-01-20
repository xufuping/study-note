# 一、概览H5

## 1、H5最重要的三项技术：H5核心规范、CSS、JavaScript.

HTML5核心规范定义用以标记内容的元素，并明确其含义。CSS可用于控制标记过的内容呈现在用户面前的外貌。JavaScript则可以用来操纵HTML文档的内容以及相应用户的操作，此外要想使用HTML5新增元素的一些为编程目的设计的特性也需要用到JavaScript。

# 二、概览HTML标签元素

## 1、HTML实体：HTML文档中有些字符具有特殊含义，需要用到HTML实体来替代特殊字符。常见的有：

    >  &lt       <  &gt        &   &amp；

## 2、<p dir="rtl">文字从右向左</p>     
<p dir="ltr">文字从左向右</p>

## 3、id属性用来给元素分配一个唯一的标识符。

## 4、tabindex通过键盘焦点Tab键在各个元素间切换。EG:

    <form>
    <label>Name:<input type="text" name="name" tabindex="1"/></label>
    </p>
    <label>City:<input type="text" name="city" tabindex="-1"/></label>
    </p>
    <label>Country:<input type="text" name="country" tabindex="2"/></label>
    </p>
    <input type="submit" tabindex="3"/>
    </form>
tabindex值为1的元素会第一个被选中，用户按一下Tab按键后，tabindex值为2的那个元素会被选中，依次类推。tabindex设置为-1的元素不会在用户按下Tab键后被选中。所以实施效果是：按下Tab键，键盘焦点从第一个input元素转到第三个，然后转到submit按钮。

# 三、概览CSS（层叠样式表，Cascading Style Sheets）

## 1、样式如何层叠：

（1）元素内嵌样式（用元素的全局属性style定义的样式）

（2）文档内嵌样式（定义在<style type="text/css"></style>元素中的样式）

（3）外部样式（用link元素导入的样式<link rel="stylesheet" type="text/css" href=""></link>）

（4）用户样式（用户定义的样式，以Chrome为例，它在用户的个人配置信息目录中生成一个名为Default\UserStyleSheet\Custom.css的文件，添加到这个文件中的任何样式都会被用于用户访问的所有网站）；

（5）浏览器样式（浏览器应用的默认样式，例如a｛color:blue;    text-decoration:underline;｝）。

【（6）样式具体程度越高，可以覆盖程度低的样式，EG:a.myclass }样式可以覆盖a{ }样式】

【（7）样式定义次序，后面的具体程度相同的样式也会覆盖之前的具体程度相同样式】

## 2、用重要样式调整层叠次序，EG：

    a{color:black !important;}//不管样式定义在什么地方，浏览器都会优先考虑这个样式。
## 3、样式继承：子元素会继承父元素的样式，EG：

    <head>
    <style type="text/css">
    p{
    color:white;
    background:grey;
    border:medium solid black;
    }
    </style>
    </head>
    <body>
    <p>I like <span>apples</span>and oranges.</p>//这里span标签会继承p标签的样式
    </body>
令人尴尬的是，并非所有CSS属性都可以继承。这方面有条经验可供参考：与元素外观（文字颜色、字体等）相关的样式会被继承；与元素在页面上的布局相关的样式不会被继承。在样式中使用inherit这个特别设立的值可以强行实施继承，明确指示浏览器在该属性上使用父元素样式中的值。EG：

    <head>
    <style type="text/css">
    p{
    color:white;
    background:grey;
    border:medium solid black;
    }
    span{
    border: inherit;
    }//span标签继承了P标签的border元素的值。
    </style>
    </head>
    <body>
    <p>I like <span>apples</span>and oranges.</p>//这里span标签会继承p标签的样式
    </body>
## 4、CSS颜色，一般是十六进制表示颜色，下面是rgb和hsl颜色表示方法：

rgba(r,g,b,a) 用RGB模型表示颜色，外加一个用于表示透明度的α值（0代表全透明，1代表完全不透明）

    比如： color： rgba(112,128,144,0.4)【也可是rgb(r,g,b)】。

hsl(h,s,l)表示HSL（色相[hue]、饱和度[saturation]、明度[lightness]）

    比如：color: hsl(120,100%,22%)  
    如果增加一个表示透明度的α值就是hsla(h,s,l,a)

## ▲5、用less改进CSS：http://lesscss.org

# 四、概览JavaScript

## 1、定义和使用函数

    <script type="text/javascript">
        function myFunc() {
            document.writeln("This is a statement");
        };
        myFunc();//使用上面所定义的函数
    </script>
## 2、定义带参数和返回结果的函数

    <script type="text/javascript">
        function myFunc(name) {//定义一个带参数的函数
            return("hello,"+ name + ".");//返回一个结果值
        };
        document.writeln(myFunc("Nick"));
        myFunc();//使用上面所定义的函数
    </script>
## 3、使用变量和类型

全局变量和局部变量的定义和作用。

### 3.1基本类型

字符串类型（String）、数值类型（number）、布尔类型(boolean)

### 3.2对象

创建对象：

    <script type="text/javascript">
        var myData = new Object();
        myData.name = "Lucy";
        document.writeln("Hello," + myData.name + ". ");
    </script>
#### 3.2.1使用对象字面量

    <script type="text/javascript">
        var myData = {
            name: "Lucy",
            weather: "sunny"
        };//一口气定义一个对象及其属性
        document.writeln("Hello," + myData.name + ". ");
        document.writeln("Today is " + myData.weather + ". ");
    </script>
#### 3.2.2为对象添加方法【printMessages()方法】

    <script type="text/javascript">
        var myData = {
            name: "Lucy",
            weather: "sunny",//▲这里有逗号了哟
            printMessages: function() {
                document.writeln("Hello," + this.name + ". ");
                document.writeln("Today is " + this.weather + ". ");
                //▲在方法内部使用对象属性时要用到this关键字
            }//▲这里不用分号了哟
        };
        myData.printMessages();//调用printMessages()方法
    </script>
#### 3.2.3对象的增删查改

##### 1、读取修改对象属性值

    <script type="text/javascript">
        var myData = {
            name: "Lucy",
            weather: "sunny"
        };
        myData.name = "Joe";//name值修改为Joe了【▲这是圆点表示法】
        myData["weather"] = "raining";//weather值修改为raining了【▲这是类数组索引法】
        document.writeln("Hello" + myData.name + ". ");
        document.writeln("Today is " + myData["weather"] + ". ");
    </script>
枚举对象属性

    <script type="text/javascript">
        var myData = {
            name: "Lucy",
            weather: "sunny"，
            printMessages: function() {
                document.writeln("Hello," + this.name + ". ");
                document.writeln("Today is " + this.weather + ". ");
                //▲在方法内部使用对象属性时要用到this关键字
            }//▲这里不用分号了哟
        };
        for (var prop in myData){
            document.writeln("Name: " + prop + "Value: " + myData[prop]);
        }
    </script>
    //for...in循环代码块中的语句会对myData对象的每一个属性执行一次，在每一次迭代过程中，所要处理的属性名会被赋给prop变量。这里用类数组索引法获取对象属性值。
代码输出结果如下：

    Name: name Value： Lucy
    Name: weather Value： sunny
    Name:printMessages  Value： function() { 
        document.writeln("Hello," + this.name + ".");   
        document.writeln("Today is " + this.weather + ". ");}

##### 2、为对象添加新属性

    <script type="text/javascript">
        var myData = {
            name: "Lucy",
            weather: "sunny"
        };
        myData.dayOfWeek = "Monday"；//为对象添加一个名为dayOfWeek的新属性
    </script>

为对象添加新方法

    <script type="text/javascript">
        var myData = {
            name: "Lucy",
            weather: "sunny"
        };
        myData.sayHello = function() {
            document.writeln("Hello");
        };//需要使用myData.sayHello()执行这个函数
    </script>
##### 3、删除对象属性

用delete关键字删除

    <script type="text/javascript">
        var myData = {
            name: "Lucy",
            weather: "sunny"
        };
        myData.sayHello = function() {
            document.writeln("Hello");
        };
        delete myData.name;//删除对象中的属性值
        delete myData["weather"];//删除对象中的属性值
        delete myData.sayHello;//删除对象中的方法
    </script>
##### 4、判断对象是否具有某个属性

可以用in表达式判断对象是否具有某个属性。

    <script type="text/javascript">
        var myData = {
            name: "Lucy",
            weather: "sunny"
        };
        var hasName = "name" in myData;
        var hasDate = "date" in myData;
        document.writeln("HasName: " + hasName);
        document.writeln("HasDate: " + hasDate);
    </script>
    此例分别用一个已有的和一个没有的属性进行测试。hasName变量的值会是true，而hasDate变量的值会是false。

### 3.3相等和等同测试【易错点】

#### 3.3.1对象的相等和等同测试

    <script type="text/javascript">
        var myData1 = {
            name: "Lucy",
            weather: "sunny"
        };
        var myData2 = {
            name: "Lucy",
            weather: "sunny"
        };
        var myData3 = myData2;
        var test1 = myData1 == myData2;
        var test2 = myData2 == myData3;
        var test3 = myData1 === myData2;
        var test4 = myData2 === myData3;
        document.writeln("Test 1: " + test1 + "Test 2: " + test2):
        document.writeln("Test 3: " + test3 + "Test 4: " + test4):
    </script>
运行结果：

    Test 1: false
    Test 2: true
    Test 3: false
    Test 4: true
    
#### 3.3.2基本类型的相等和等同测试

    <script type="text/javascript">
        var myData1 = 5;
        var myData2 = "5";
        var myData3 = myData2;
        var test1 = myData1 == myData2;
        var test2 = myData2 == myData3;
        var test3 = myData1 === myData2;
        var test4 = myData2 === myData3;
        document.writeln("Test 1: " + test1 + "Test 2: " + test2);
        document.writeln("Test 3: " + test3 + "Test 4: " + test4);
    </script>
运行结果：

    Test 1: true
    Test 2: true
    Test 3: false
    Test 4: true
    
### 3.4显式类型转换【易错点】

字符串连接运算符“+”比加法运算符“+”优先级更高。

    <script type="text/javascript">
        var myData1 = 5 + 5;
        var myData2 = 5 +  "5";
        document.writeln("Result 1: " + myData1);
        document.writeln("Result 2: " + myData2);
    </script>
运行结果：

    Result 1: 10
    Result 2: 55//这里正是因为字符串连接运算符比加法运算符优先级更高。
所以：

    A、将数值转换为字符串有toString()方法toString(2)， toString(8)， toString(16)分别是转换为十进制、二进制、八进制、十六进制。
    B、将字符串转换为数值分别有Number(<str>)、parseInt(<str>)、parseFloat(<str>)三种方法。
## 4、数组

### 4.1创建和填充数组

    <script type="text/javascript">
        var myArray = new Array();
        myArray[0] = 100;
        myArray[1] = "Adam";
        myArray[0] = true;
    </script>
首先，创建数组的时候不需要声明数组中元素的个数。其次，不必声明数组所含数据的类型。本例中分别把一个数值、一个字符串和一个布尔值赋给了不同的数组元素。

### 4.2使用数组字面量

var myArray = [100,"Adam",true];

读取以及修改数组内容:

    <script type="text/javascript">
        var myArray = [100,"Adam",true];
        myArray[0] = "Sunday";//修改数组内容
        document.writeln("Index 0: " + myArray[0]);//读取数组内容
    </script>
▲枚举数组内容（把数组中相对应的内容给一一列举出来）：

    <script type="text/javascript">
        var myArray = [100,"Adam",true];
        for (var i =0; i < myArray.length; i++){
        document.writeln("Index " + i +": " + myArray[i]);
        }
    </script>
### 4.3一些数组中的内置方法

    concat(<otherArray>) 将数组和参数所指数组的内容合并为一个新数组。可以指定多个数组。
    join(<separator>) 将所有数组元素连接为一个字符串。各元素内容用参数指定的字符分隔。
    pop() 把数组当作栈使用，删除并返回数组的最后一个元素
    push(<item>) 把数组当作栈使用，将指定的数据添加到数组中
    reverse() 就地反转数组元素的次序
    shift（） 类似pop，但是操作的是数组中的第一个元素。
    slice(<start>,<end>) 返回一个子数组
    sort() 就地对数组元素排序
    unshift() 类似push,但新元素被插入到数组的开头位置
## 5、处理错误

JavaScript用try...catch语句处理错误。

    <script type="text/javascript">
        try{
            var myArray = [100,"Adam",true];
                for (var i =0; i < myArray.length; i++){
                document.writeln("Index " + i +": " + myArray[i]);
            }
        } catch (e) {
            document.writeln("Error: " + e);
        } finally {
            document.writeln("Statements here are always executed");
        }
    </script>
如果没有发生错误，这段代码会正常执行，catch句子会被忽略。但是如果错误发生，那么try句子语句执行将立即停止，控制权转移到catch语句上，发生的错误由一个Error对象描述。最后，无论是否发生错误，都会执行finally这一行打出一行字。

## 6、分清undefined和null值

如果在读取未赋值的变量或试图读取对象没有的属性时得到的就是undefined，它是在没定义值的情况下得到的值；

null是表示已经赋了一个值，但是该值不是一个有效的object/string/number和boolean值（也就是说定义的是一个无值）。

[这里可以看读书笔记：《编写可维护的JS》——1]