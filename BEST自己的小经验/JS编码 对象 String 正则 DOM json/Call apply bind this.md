> 目录
>
> call
>
> apply
>
> bind
>
> this
> > 1.this指向
> >
> > 2.`this` 和 `return`
> >
> > 3.闭包中的this

**核心:**`call`与`apply`主要是为了动态改变`this`而出现的。

### call
call：调用一个对象的一个方法，用另一个对象替换当前对象。

    function.call(thisObj[, arg1[, arg2[, [,...argN]]]]);
    例如：
    B.call(A, args1,args2); // 即A对象调用B对象的方法。

### apply
`apply`：调用一个对象的一个方法，用另一个对象替换当前对象。

    function.apply(thisObj[, argArray])
    例如：
    B.apply(A, arguments); // 即A对象应用B对象的方法。
    
### bind
把obj绑定到thisObj，这时候thisObj具备了obj的属性和方法。与call和apply不同的是，bind绑定后不会立即执行。

    obj.bind(thisObj, arg1, arg2, ...);

    绑定后执行
    obj.bind(thisObj, arg1, arg2, ...)();
    
### this
#### 1.this指向
一般来说，`this`指向的就是调用它的对象。

    function A() {
        console.log(this); // window
    }
    A();
    
    var o = {
        user:"xqf",
        fn:function(){
            console.log(this); // o对象：{user: "xqf", fn: ƒ}
        }
    }
    o.fn();
    
    var o = {
        a:10,
        b:{
            a:12,
            fn:function(){
                console.log(this); // 指向o.b：{a: 12, fn: ƒ}
            }
        }
    }
    o.b.fn();

特殊的情况：

    var o = {
        a:10,
        b:{
            a:12,
            fn:function(){
                console.log(this); //window
            }
        }
    }
    var j = o.b.fn;
    j(); // 因为j是全局属性，将对象中的函数赋值给了j，所以指向了window
    
#### 2.`this` 和 `return`
如果返回值是一个对象，那么`this`指向的就是那个返回的对象，如果返回值不是一个对象那么`this`还是指向函数的实例。还有一点就是虽然`null`也是对象，但是在这里`this`还是指向那个函数的实例，因为`null`比较特殊。

    function fn() {  
        this.user = 'xqf';  
        return {};  
    }
    var a = new fn();  
    console.log(a.user); // undefined
    
    function fn() {  
        this.user = 'xqf';  
        return function(){};  
    }
    var a = new fn;  
    console.log(a.user); // undefined
    
    function fn() {  
        this.user = 'xqf';  
        return 1;  
    }
    var a = new fn();  
    console.log(a.user); // xqf
    
    function fn() {  
        this.user = 'xqf';  
        return undefined;  
    }
    var a = new fn();  
    console.log(a.user); // xqf

#### 3.闭包中的this
一般来说，在全局函数中，`this`等于`window`，而当函数被作为某个对象的方法调用时，`this`等于那个对象。但是，匿名函数的执行环境具有全局性，因此`this`对象通常指向`window`

    var name = "My window";
    var object = {
    	name:"my Object",
    
    	getNameFunc:function(){
    		return function(){
    			return this.name;
            };
        }
    };
    
    console.log(object.getNameFunc()()); // My window

因为闭包的内部函数不能通过搜索变量直接访问到外部函数的变量，因此，我们可以把外部作用域中的`this`对象保存在一个闭包能够访问的变量里，这样闭包中的内部函数就可以访问外部作用域的相应对象了。

    var name = "My window";
    var object = {
    	name:"my Object",
    
    	getNameFunc:function(){
    	    var that = this;
    	    
    		return function(){
    			return that.name;
            };
        }
    };
    
    console.log(object.getNameFunc()()); // my Object