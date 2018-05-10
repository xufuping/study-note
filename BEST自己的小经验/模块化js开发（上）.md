**前言**

什么是模块化开发？模块化开发使代码耦合度降低，最大化的设计重用，解决命名冲突、文件依赖的问题。简单说，就是以最少的模块、零部件，更快速的满足更多的个性化需求。通俗地讲，我们使用模块化就可以更方便地使用别人的代码，想要什么功能就加载什么模块。

JS的模块化开发规范有：

1. 服务器端规范：CommonJs -- nodejs使用的规范

2. 浏览器端规范：

    2.1 AMD -- RequireJS
    
    2.2 CMD -- SeaJS

对比`AMD`与`CMD`：

- AMD推崇提前执行，CMD推崇延后执行
- AMD推崇依赖前置，CMD推崇依赖就近
- AMD的API默认是一个当多个用，CMD的API严格区分，推崇职责单一。

ES6模块设计思想推崇静态化，使得编译时就能确定模块的依赖关系和输入输出的变量，`CommonJS`和`AMD`模块都只能在运行时确定这些东西。

> 目录
>
> 1.语法
>
> 2.export default
>
> 3.import()

#### 1.语法
`export`输出变量、函数、类，可以使用`as`重命名，处于模块顶层（在块级作用域内就会报错）。

    function f() {}
    export {f as a};

`import`加载`export`暴露的接口，加载的变量名必须和暴露出的相同，但可以使用`as`重命名。它是只读的，不可重写暴露出的模块（对象可重写属性，但不建议这么做，因为很难查错）。使用`from`获取文件路径，可以省略`.js`。

    import { a as b } from './func.js'
    
由于`import`是静态执行，所以不能使用表达式和变量，这些只有在运行时才能得到结果的语法结构。

    // 报错
    import { 'f' + 'oo' } from 'my_module';
    // 报错
    let module = 'some_module';
    import { foo } from module;
    // 报错
    if (x === 1) {
      import { foo } from 'module1';
    } else {
      import { foo } from 'module2';
    }
    
`import`语句会执行所加载的模块：

    import `lodash` // 执行lodash，不过没有输入任何值
    
#### 2.export default
前面的加载需要知道要加载的变量名或函数名，为了尽快上手不用去阅读文档就能加载属性和方法，可以使用`export default`，为模块指定默认输出。

`export default`后面不能跟变量声明语句，一个模块只能有一个默认输出（所以`export default`只能使用一次）。`import`可以直接重命名`export default`暴露的接口（无论`export default`暴露的是匿名函数还是命名函数），不需要使用`{}`。

    // now
    export default function crc32() { // 输出
      // ...
    }
    
    import newCrc from 'crc32'; // 输入
    
    // before
    export function crc32() { // 输出
      // ...
    };
    
    import {crc32} from 'crc32'; // 输入

例子：输出输入类

    // MyClass.js
    export default class { ... }
    
    // main.js
    import MyClass from 'MyClass';
    let o = new MyClass();
    
例子：输出一个默认接口和其他接口

    // 'lodash'
    export default function (obj) {
      // ···
    }
    
    export function each(obj, iterator, context) {
      // ···
    }
    
    export { each };
    
    // '输入'
    import _, { each } from 'lodash';

注：

`export default`命令其实只是输出了一个叫做`default`的变量。

`export {...} from './...'`是一种复合写法，代表输入某个变量然后又输出，实际上并没有被导入当前模块，只是相当于对外转发了接口。

#### 3.import()
`import`是静态加载（引擎在编译时就处理`import`），`require`是动态加载（运行时加载模块）。可以引入`import()`完成动态加载，`import()`返回一个 `Promise` 对象。`import()`类似于 `Node` 的`require`方法，区别主要是前者是异步加载，后者是同步加载。

    // import()返回一个promise对象
    const main = document.querySelector('main');

    import(`./section-modules/${someVariable}.js`)
      .then(module => {
        module.loadPageInto(main);
      })
      .catch(err => {
        main.textContent = err.message;
      });
     
**适用场景：**

（1）按需加载：`import()`可以在需要的时候，再加载某个模块。

    button.addEventListener('click', event => {
      import('./dialogBox.js')
      .then(dialogBox => {
        dialogBox.open();
      })
      .catch(error => {
        /* Error handling */
      })
    });
    
（2）条件加载：`import()`可以放在if代码块，根据不同的情况，加载不同的模块。

    if (condition) {
      import('moduleA').then(...);
    } else {
      import('moduleB').then(...);
    }

**获取输出接口：**

`import()`加载模块成功以后，这个模块会作为一个对象，当作`then`方法的参数。因此，可以使用对象解构赋值的语法，获取输出接口。

    // export1和export2都是myModule.js的输出接口，可以解构获得。
    import('./myModule.js')
    .then(({export1, export2}) => {
      // ...·
    });

如果模块有`default`输出接口，可以用参数直接获得。

    import('./myModule.js')
    .then(myModule => {
      console.log(myModule.default);
    });
    
如果想同时加载多个模块，可以采用下面的写法。

    Promise.all([
      import('./module1.js'),
      import('./module2.js'),
      import('./module3.js'),
    ])
    .then(([module1, module2, module3]) => {
       ···
    });