> 目录
>
> 1.选择符
>
> 2.元素遍历-不包括文本节点和注释
>
> 3.HTML5
> > 3.1 classList
> >
> > 3.2 焦点管理
> >
> > 3.3 readyState
> > 
> > 3.4 自定义数据 data-
> > 
> > 3.5 插入（innerHTML outerHTML等）
>
> 4.children contains() 插入文本 滚动
> > 4.1 children
> >
> > 4.2 contains()
> >
> > 4.3 插入文本
>
> 5.确定元素大小和位置 遍历DOM结构

---

#### 1.选择符
-   `document.getElementById()`
-   `document.getElementsByTagName()`
-   `document.getElementsByClassName()`
-   `querySelector()`
-   `querySelectorAll()`

后面俩选择符兼容性：IE8+, FireFox3.5+, Safari3.1+, Chrome 和 Opera 10+

**`querySelector()`：** 使用 Document 类型调用该方法，会在文档元素的范围内查找匹配元素；使用 Element 类型调用该方法，会在该元素后代元素的范围内查找匹配元素。如果被查找元素有重复结果，直接返回第一个元素。

    document.querySelector('body') // body
    document.querySelector('#myDiv') // id 为 myDiv 的元素
    document.querySelector('.selected') // class 为 selected 的元素
    document.body.querySelector("img.pic") // 获取类为"pic"的第一个图像元素
    
**`querySelectorAll()`:** 接受参数上同，返回一个 NodeList 实例。

**`document.getElementsByClassName()`:** 接收一或多个类名的字符串，返回 NodeList。（兼容到IE9+）

#### 2.元素遍历-不包括文本节点和注释

属性 | 含义
---|---
childElementCount | 返回子元素个数（不包括文本节点和注释）
firstElementChild | 第一个子元素（不包括文本节点和注释）
lastElementChild |  最后一个子元素（不包括文本节点和注释）
previousElementSibling | 前一个同辈元素（不包括文本节点和注释）
nextElementSibling | 后一个同辈元素（不包括文本节点和注释）

#### 3.HTML5

##### 3.1 classList
如果一个元素有多个类名，要想增删可以使用这个属性，兼容Firefox3.6+ 和 Chrome。

-   add(val) 添加一个类
-   contains() 是否存在该类，存在true。
-   remove(val) 移除一个类
-   toggle(val) 如果存在val，删除；如果不存在val，添加。


    <div class="bd user disabled"></div>
    
    div.classList.remove("user"); // 删除"user"
    div.classList.add("current"); // 添加"current"
    div.classList.toggle("user"); // 切换"user"
    if (div.classList.contains("bd")) {
        // 执行某些操作
    }

##### 3.2 焦点管理
（兼容大多数浏览器）

**`document.activeElement`** 属性返回DOM中当前获得了焦点的元素。

**`document.hasFocus()`** 方法用于确定文档是否获得了焦点。

    var inp = document.getElementById('inp');
    inp.focus();
    console.log(document.activeElement); // <input type="text" id="inp">
    console.log(document.hasFocus()); // true
    
##### 3.3 readyState
`Document` 的 `readyState` 有两个值（兼容大多数浏览器）：

-   loading，正在加载文档
-   complete，加载完成文档

    
    if (document.readyState == "complete"){ 执行操作 }
    
##### 3.4 自定义数据 data-
（兼容Ff6+ 和Chrome）

可以使用元素的 dataset 属性访问自定义的属性：

    <div id="mydiv" data-appid="2333" data-myname="Xuqingfeng"></div>
    
    var div = document.getElementById('mydiv');
    var appId = div.dataset.appid;
    var myname = div.dataset.myname;
    console.log(appId + ":" + myname); // 2333:Xuqingfeng
    
##### 3.5 插入（innerHTML outerHTML等）
1. **innerHTML()** : 读模式下，返回调用元素所有子节点对应的HTML标记；写模式下，会根据传入值创建新的DOM树，然后用这个DOM树完全替换调用元素原先的所有子节点。（兼容IE8+）

2. **outerHTML()** ：读模式下，返回调用元素以及调用元素所有子节点的HTML；写模式下，，会根据传入值创建新的DOM树，然后用这个DOM树完全替换调用元素。（兼容IE4+等大多浏览器，Firefox7+）

3. **insertAdjacentHTML()** ：（兼容IE4等大多浏览器，Firefox8+）

    1. beforebegin 当前元素之前插入一个同辈元素
    2. afterbegin 作为第一个子元素插入
    3. beforeend 作为最后一个子元素插入
    4. afterend 当前元素之后插入一个同辈元素


    element.insertAdjacentHTML("beforebegin", "\<p\>Hello world!\<\/p\>");
    
4. 内存与性能

    使用以上三个方法可能导致浏览器内存占用问题，因为替换或者删除某个元素但是并没有将其绑定事件删除，如果这种情况频繁出现，页面占用内存就会明显增加。所以使用这些方法时最好手工删除被删除元素绑定的事件处理程序和JS对象属性。

#### 4.children contains() 插入文本 滚动
##### 4.1 children
`children属性` 是 `HTMLCollection实例`，大多数浏览器支持。

##### 4.2 contains()
判断某个节点是不是另一个节点的后代，大多数浏览器支持，FF9+支持：

    console.log(parentelement.contains(childElement)); // true or false

##### 4.3 插入文本
`innerText`、`textContent`、`outerText(不推荐使用)`三者返回值相同，可读可写。

#### 5.确定元素大小和位置 遍历DOM结构
使用`getBoundingClientRect()`方法返回 `DOMRect 对象`。包含元素的大小和位置。【《高程》P325兼容各大浏览器方案】

使用 `NodeIterator` 和 `TreeWalker` 遍历DOM结构。