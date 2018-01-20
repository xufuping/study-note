> 目录
>
> 1.面向对象的程序设计
> 
> > 1.1 创建对象
> 
> > 1.2 继承

### 1.面向对象的程序设计

#### 1.1 创建对象

六种模式：工厂模式、构造函数模式、原型模式、动态原型模式、寄生构造函数模式、稳妥构造函数模式。

▲ 创建对象的最佳方式是组合使用构造函数模式和原型模式，使用构造函数定义实例属性，而使用原型模式定义共享的属性和方法。

**组合使用构造函数模式和原型模式：**
构造函数模式用于定义实例属性，原型模式用于定义方法和共享的属性。这样的话，不仅支持向构造函数传递参数，而且每个实例都会有自己的一份实例属性的副本，同时又共享着对方法的引用，最大限度地节省了内存。

    // 构造函数模式用于定义实例属性
	function Person(name, age, job) {
		this.name = name;
		this.age = age;
		this.job = job;
		this.friends = ["XiangShuai", "ZhanShen"];
	}

	// 原型模式用于定义方法和共享的属性
	Person.prototype = {
		
		// 默认情况下，所有原型对象都会自动获得一个constructor属性，这个属性是一个指向prototype属性所在函数的指针
		// 这里的Person.prototype.constructor指向Person
		constructor : Person, 
		sayName : function() {
			console.log(this.name);
		}
	}

	var person1 = new Person("Kalen", 30, "Software Engineer");
	var person2 = new Person("Xushao", 21, "Doctor");

	person1.friends.push("Xushao");
	console.log(person1.friends);  // ["XiangShuai", "ZhanShen", "Xushao"]
	console.log(person2.friends); // ["XiangShuai", "ZhanShen"]
	console.log(person1.friends === person2.friends); // false
	console.log(person1.sayName === person2.sayName); // true

#### 1.2 继承

六种方式：原型链、借用构造函数、组合继承、原型式继承、寄生式继承、寄生组合式继承。

▲ **组合继承**：这种模式使用原型链实现对原型属性和方法的继承，而通过借用构造函数来实现对实例属性的继承。这样既通过在原型上定义方法实现了函数复用，又能够保证每个实例都有它自己的属性。

    function SuperType(name) {
		this.name = name;
		this.colors = ["red", "blue", "green"];
	}

	SuperType.prototype.sayName = function() {
		console.log("name:" + this.name);
	};

	function SubType(name, age) {

		// 继承属性
		SuperType.call(this, name);

		this.age = age;
	}

	// 继承方法
	SubType.prototype = new SuperType();
	SubType.prototype.constructor = SubType;
	SubType.prototype.sayAge = function() {
		console.log("age:" + this.age);
	};

    // 这样定义的话，可以让两个不同的SubType既分别拥有自己的属性，又可以使用相同的方法了。

	var instance1 = new SubType("Nicholas", 29);
	instance1.colors.push("black");
	console.log("instance1.colors:" + instance1.colors); // instance1.colors:red,blue,green,black
	instance1.sayName(); // name:Nicholas
	instance1.sayAge(); // age:29

	var instance2 = new SubType("Greg", 22);
	console.log("instance2.colors:" + instance2.colors); // instance2.colors:red,blue,green
	instance2.sayName(); // name:Greg
	instance2.sayAge(); // age:22
