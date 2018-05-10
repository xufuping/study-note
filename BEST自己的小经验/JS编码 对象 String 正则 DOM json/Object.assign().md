> 目录
>
> 1.浅拷贝
>
> 2.合并对象

**语法：**

`Object.assign(目标对象, ...源对象)`

    const object1 = {
      a: 1,
      b: 2,
      c: 3
    };
    
    const object2 = Object.assign({c: 4, d: 5}, object1);
    
    console.log(object2.c, object2.d); // 3 5

#### 1.浅拷贝
可以使用`Object.assign`实现对象的浅拷贝，只要第一个参数是相对应的对象或数组（'{ }'或'[ ]'）则`Object.assign`拷贝的对象就是放在新的堆内存的一个全新对象。（如果没有第一个参数则是直接拷贝，只是栈内存地址不同，但都指向同一个堆内存对象或数组）。

    var obj = { a:1 };
    var copy = Object.assign({}, obj);
    console.log(copy.a); // 1
    copy.a = 2;
    console.log(obj.a); // 1
    console.log(copy.a); // 2
    
▲▲▲ 使用`JSON.parse(JSON.stringify(obj))`实现深拷贝。

    obj1 = { a: 0 , b: { c: 0}};
    let obj2 = JSON.parse(JSON.stringify(obj1));
    obj1.a = 4;
    obj1.b.c = 4;
    console.log(obj2); // { a: 0, b: { c: 0}}


#### 2.合并对象

    var o1 = { a: 1 };
    var o2 = { b: 2 };
    var o3 = { c: 3 };
    
    var obj = Object.assign(o1, o2, o3);
    console.log(obj); // { a: 1, b: 2, c: 3 }
    console.log(o1);  // { a: 1, b: 2, c: 3 }, 注意目标对象自身也会改变。
    
如果对象具有相同属性，属性被后续参数中具有相同属性的其他对象覆盖。

    var o1 = { a: 1, b: 1, c: 1 };
    var o2 = { b: 2, c: 2 };
    var o3 = { c: 3 };
    
    var obj = Object.assign({}, o1, o2, o3);
    console.log(obj); // { a: 1, b: 2, c: 3 }