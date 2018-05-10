> 目录
>
> 1. Node类型
>
> 2. Document类型
>
> 3. Element类型
>
> 4. DocumentFragment类型
>
> 5. 其它类型

---

基本示例：

    <div id="box" class="first parentBox">
        <ul id="listBox" class="ulDom">
            list
            <li class="listText">1111</li>
            <li class="listText">2222</li>
            <li class="listText">3333</li>
            <li class="listText">4444</li>
            <li class="listText">5555</li>
        </ul>
    </div>

#### 1. Node 类型
`nodeType` 表明节点类型；`nodeName` 表明节点名称；`nodeValue` 表明节点内容；`childNodes` 返回一个 `NodeList` 类数组对象，有`length` 属性，不是 `Array` 的实例。

`NodeList` 是基于DOM结构动态执行查询的结果，因此DOM结构的变化能够自动反映在 `NodeList` 对象中。拥有`item()` 方法。

    var listBox = document.getElementById('listBox');
    
    console.log(listBox.nodeType); // Element 类型 nodeType值为1
    console.log(listBox.nodeName); // UL
    console.log(listBox.nodeValue); // null, 因为ul节点没有值
    console.log(listBox.childNodes); // NodeList(11) [text, li.listText, text, li.listText, text, li.listText, text, li.listText, text, li.listText, text]
    
    console.log(listBox.childNodes[0]); // 返回Text节点，"list5条数据"
    【上面的代码和下面代码等效】
    console.log(listBox.childNodes.item(0)); // 返回Text节点，"list5条数据"
    
    【将 NodeList 转化为数组】
    console.log(Array.prototype.slice.call(listBox.childNodes, 0)); // [text, li.listText, text, li.listText, text, li.listText, text, li.listText, text, li.listText, text]
    【IE8及更早版本要转化为数组必须新建数组然后使用for循环push到新数组中-手动枚举】
    
`parentNode` 指向父节点，`firstChild` 指向第一个子节点，`lastChild` 指向最后一个子节点，`nextSibling` 指向后一个同胞节点，`previousSiblng` 指向前一个同胞节点。【这里的节点是 `NodeList` 里的节点，包括`Document/Text/Element`等】

-   `hasChildNodes()` 在节点包含一或多个子节点时返回 `true`。
-   `appendChild()` 向 `childNodes` 列表的末尾添加一个节点。
-   `insertBefore()`, 接收两个参数，要插入的节点和作为参照的节点。


    someNode.insertBefore(newNode, null); // 插入成为最后一个子节点
    someNode.insertBefore(newNode, someNode.firstChild); // 插入成为第一个子节点
    someNode.insertBefore(newNode, someNode.lastChild); // 插入到最后一个子节点前面
    
-   `replaceChild()`, 接收两个参数，要插入的节点和要被替换的节点。
-   `removeChild()`, 接收一个参数，要移除的节点。
-   `cloneNode()`, 接收布尔值，`true`表示深复制，复制节点及其整个节点树，`false`表示浅复制，复制节点本身。


    someNode.cloneNode(true);
    
-   `normalize()` 处理文档树中的文本节点，如果找到空文本节点则删除它，如果找到相邻的文本节点则合并为一个文本节点。

#### 2. Document 类型
`nodeType`值为 9 。

**常用document节点：**

document节点 | 返回值
---|---
document.documentElement | 返回整个 html
document.head \|\| document.getElementsByTagName("head")[0] | 返回整个head
document.body | 返回整个body
document.doctype | 返回整个Doctype
---------------- | -------------
document.title | 返回title内的文本值
document.URL | 返回页面url
document.domain | 返回页面域名。如果有内嵌框架，可以将其域名设置为同一个子域名，就可以相互访问对方的JS对象了。

**查找元素：**
方法 | 返回值
---|---
getElementById() | 取得id元素的引用
getElementsByTagName() | 返回标签元素的 `NodeList`。在HTML文档中，返回一个 `HTMLCollection` 对象。

**`HTMLCollection` 对象的方法：**
方法 | 返回值
---|---
item() | 返回`HTMLCollection` 对象对应位置的值
namedItem() | 通过元素的 name 取得`HTMLCollection` 对象中的项
getElementsByName() | 通过元素的 name 取得元素的引用

文档写入：`document.write()` 向页面输出内容，会更新整个`document`文档。

#### 3. Element类型
`nodeType`值为 1 。

属性 | 含义
---|---
tagName | 返回标签名（默认大写表示）
id | 返回id
className | 返回元素的class

**属性操作：**

方法 | 含义
---|---
getAttribute | 取得某个属性的值
setAttribute | 两个参数,`key` 与 `value`
removeAttribute | 彻底删除元素属性

`attributes` 属性包含一个 `NamedNodeMap`，是一个动态集合。有`getNamedItem(name)`，`removeNamedItem(name)`，`setNamedItem(node)`三个方法，和前面三个方法相对应；还有一个`item(num)`方法，返回属性列表中第num个位置处的节点。

    console.log(listBox.attributes);
    // NamedNodeMap {0: id, 1: class, id: id, class: class, length: 2}

**创建元素-`document.createElement()`:**

    document.createElement('li'); // 创建li标签
    
#### 4. DocumentFragment类型
`nodeType`值为 11 。文档片段，可以将其中子节点添加到文档中。`document.createDocumentFragment()` 创建文档片段。

    var fragment = document.createDocumentFragment();
    var li = null;
    for(let i=0;i<3;i++){
        li = document.createElement("li");
        // li.appendChild(document.createTextNode((i+6)+''+(i+6)+''+(i+6)+''+(i+6)));
        // li.className = 'listText';
        li.innerText = (i+6)+''+(i+6)+''+(i+6)+''+(i+6);
        fragment.appendChild(li);
    }
    listBox.appendChild(fragment);
    
#### 5. 其它类型
-   Text类型： nodeType值为3，表示文本节点
-   Comment类型：nodeType值为8，表示注释
-   DocumentType类型：nodeType值为10，表示doctype
-   Attr类型：nodeType值为2，表示元素的特性
-   CDATASection类型：nodeType值为4，只针对 XML 文档，表示CDATA区域