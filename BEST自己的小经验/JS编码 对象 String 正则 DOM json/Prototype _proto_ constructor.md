> 目录结构
>
> 1._ proto_ 和 prototype
>
> 2.constructor

我们先来说`_proto_`（隐式原型）和`prototype`（显式原型）:

● 对象（在`JS`中函数`function`也是对象）都有属性`__proto__`,指向构造该对象的构造函数的原型对象。

● 函数除了有属性`__proto__`，还有属性`prototype`，这个属性是一个指针，指向该函数的原型对象。

    function F(){}
    var B = {name: "bbb"};
    F.prototype = B;
    var f = new F();
    
    // 可以使用Object.getPrototypeOf() 代替 __proto__
    Object.getPrototypeOf(f) === f.__proto__;   // true
    
    console.log(f.__proto__ === F.prototype); // true
    console.log(f.__proto__ === B);   // true

![image](http://img.blog.csdn.net/20131112143012281?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY3VldzE5ODc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

**隐式原型的作用**：构成原型链，同样用于实现基于原型的继承。举个例子，当我们访问`obj`这个对象中的x属性时，如果在obj中找不到，那么就会沿着`__proto__`依次查找。

**显式原型的作用**：用来实现基于原型的继承与属性的共享。

举个栗子-原型对象：

    function Person(name) {
        this.name = name;
    }
    Person.prototype = {
        constructor: Person,
        sayName: function(){
            console.log("my name is " + this.name);
        }
    }
    var p1 = new Person("xqf");
    var p2 = new Person("Alice");
    p1.sayName();   // my name is xqf
    p2.sayName();   // my name is Alice

![prototype原型对象](http://img.blog.csdn.net/20161208183943943?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGlnYW5nMjU4NTExNg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

● 原型对象也有一个属性，叫做`constructor`，这个属性包含了一个指针，指回原构造函数。`constructor` 属性是专门为 `function` 而设计的，它存在于每一个 `function` 的 `prototype` 属性中。这个 `constructor` 保存了指向 `function` 的一个引用。

    function Foo() {}
    /*
    我们创建一个函数时，JavaScript 内部会执行如下几个动作：
    1.为该函数添加一个原形（即 prototype）属性 
    2.为 prototype 对象额外添加一个 constructor 属性，并且该属性保存指向函数 Foo 的一个引用
    */

当我们把函数 `Foo` 作为自定义构造函数来创建对象的时候，对象实例内部会自动保存一个指向其构造函数（这里就是 `Foo`）的 `prototype` 对象的一个属性`proto`。 所以我们在每一个对象实例中就可以访问构造函数的 `prototype` 所有拥有的全部属性和方法，就好像它们是实例自己的一样。当然该实例也有一个 `constructor` 属性了（从 `prototype` 那里获得的），每一个对象实例都可以通过 `constrcutor` 对象访问它的构造函数，如下：

    var f = new Foo();
    console.log(f.constructor === Foo);// true
    console.log(f.constructor === Foo.prototype.constructor);// true


我们可以用它进行对象类型判断：

    if(f.constructor === Foo) {...}  // 不推荐这么用
    
    因为constructor 属性易变，不可信赖，我们一般使用 instanceof 进行判断。
    
    if(f instanceof Foo) {...}

为什么说 `constructor` 属性易变呢？因为函数的 `prototype` 属性容易被更改。

    function Foo() {}
    
    // 下面这段代码更改了Foo的prototype属性
    Foo.prototype = {
        name: 'Ace',
        getName: function() {
            return this.name;
        }
    };

    // 于是下面的代码失效了：
    var f = new Foo();
    console.log(f.constructor === Foo); // false
    
为什么？ `Foo` 不是实例对象 f 的构造函数吗？很简单，因为构造函数 `Foo` 的原型被重写了，原有的 `prototype` 对象被一个对象的字面量{}代替了。而新建的对象{}只是 `Object` 的一个实例，系统（或者说浏览器）在解析的时候并不会在{}上自动添加一个 `constructor` 属性，因为这是 `function` 创建时的专属操作，仅当你声明函数的时候解析器才会做此动作。

但是 `constructor` 并不是不存在，如下：

    // 以下是上面的代码
    function Foo() {}
    Foo.prototype = {
        name: 'Ace',
        getName: function() {
            return this.name;
        }
    };
    var f = new Foo();
    console.log(f.constructor === Foo); // false
    
    验证 constructor 现在是否还存在
    console.log(typeof f.constructor == 'undefined'); // false
    说明 constructor 依旧存在

既然 `constructor` 存在，那它在哪里呢？我们回头分析这个对象字面量 
{}。因为{}是创建对象的一种简写，所以{}相当于是 `new Object()`。既然{}是 `Object` 的实例，自然而然它就获得了一个指向构造函数 `Object()` 的 `prototype` 属性的一个引用 `proto`。又因为 `Object.prototype` 上有一个指向 `Object` 本身的 `constructor` 属性，所以可以看出这个 `constructor` 其实就是 `Object.prototype` 的 `constructor`。

    console.log(f.constructor === Object.prototype.constructor); // true
    console.log(f.constructor === Object); // true
    
那我们如何解决 `constructor` 被改变后的这个问题呢？手动恢复：

    function Foo() {}
    Foo.prototype = {
        constructor: Foo, /* 重置 constructor */
        name: 'Ace',
        getName: function() {
            return this.name;
        }
    };
    
    // 之后一切恢复正常，constructor 重新获得构造函数的引用
    
    var f = new Foo();
    console.log(f.constructor === Foo); // true
    
对于JS内建的构造函数（如 `Array`, `RegExp`, `String,Number`, `Object`, `Function` 等等）的 `constructor`：

    console.log(typeof Array.prototype.constructor === Array); // true
    
构造函数也是函数，这就说明它就是 `Function` 构造函数的实例对象，自然他内部也有一个指向 `Function.prototype` 的内部引用 `proto`。因此我们得出结论：这个 `constructor`（构造函数上的 `constructor` 不是 `prototype` 上的）其实就是 `Function` 构造函数的引用：

    console.log(Array.constructor === Function); // true
    console.log(Function.constructor === Function); // true
    
【附图】

(图片 -> 构造函数 实例 原型.png)

原型对象是构造函数的prototype属性，是所有实例化对象共享属性和方法的原型对象；

实例化对象通过new构造函数得到，都继承了原型对象的属性和方法；

原型对象中有个隐式的constructor，指向了构造函数本身。