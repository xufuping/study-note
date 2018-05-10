> 目录
>
> 1.JS中的三元运算符
>
> 2.JS哪些值为false和true

#### 1.JS中的三元运算符
**(表达式) ? (表达式为true的结果) : (表达式为false的结果)**

- 简单运用


    console.log(5>3 ? '是的':'不对'); // 是的
    
    function a(){return false;}
    a() ? 'A' : 'B' // B
    
    // 使用JQuery时，添加class
    $('.item')[condition ? 'addClass' : 'removeClass']('hover');
    // 等同于
    condition ? $('.item').addClass('hover') : $('.item').removeClass('hover') ;
    
- 优化使用


    // 改变元素的display值，一般我们用如下写法
    oBtn.onclick = function() {
        oUl.style.display == "block" ? oUl.style.display="none" : oUl.style.display="block";
    }
    
    // 优化改写
    oBtn.onclick = function() {
        var style = oUl.style.display;
        oUl.style.display = (style == "block" ? "none":"block"); // 一定不能忘了把运算结果重新赋值给元素
    }

- 嵌套使用

**条件1 ? 真结果1 : ( 条件1.1 ? 真结果1.1 : (条件1.1.1 ? 真结果1.1.1:假结果1.1.1))**

以上表达式可自行扩展使用。

    var a = null;
    var b = null;
    var age = 19;
    age<18 ?  (a='儿童',b='18岁以下'):( age>50 && age<80 ? (a='老年人',b='50岁以上') : (age>=80 ? (a='长寿者',b='80岁以上'):(a='成年人',b='18~50岁')));
    console.log(a, b) // 成年人 18~50岁

- 注意事项
    
    - 符号优先级，? 的运算优先级比 +- 低
    - 三元表达式在使用过程中不能使用`break`，`continue`等语句

#### 2.JS哪些值为false和true
**值为false ： false null undefined ""(空字符串) 0 NaN**

**值为true ： true 对象 字符串 任意非0数值(包括Infinity)**