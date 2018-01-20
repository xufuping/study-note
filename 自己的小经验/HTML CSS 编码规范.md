# HTML
### 1.语法
1.嵌套元素缩进一次

2.属性定义用双引号

3.自闭合元素尾部不要添加斜线

4.结束标签不要省略

### 2.HTML5 doctype
为每个 HTML 页面的第一行添加标准模式（standard mode）的声明，这样能够确保在每个浏览器中拥有一致的展现。

    <!DOCTYPE html>

### 3.语言属性
强烈建议为 `html` 根元素指定 `lang` 属性，从而为文档设置正确的语言。这将有助于语音合成工具确定其所应该采用的发音，有助于翻译工具确定其翻译时所应遵守的规则等等。[[语言代码表]
](https://www.sitepoint.com/iso-2-letter-language-codes/)
    <html lang="en-us">
        <!-- ... -->
    </html>

### 4.IE兼容模式
IE 支持通过特定的 `<meta>` 标签来确定绘制当前页面所应该采用的 IE 版本。除非有强烈的特殊需求，否则最好是设置为 `edge mode`，从而通知 IE 采用其所支持的最新的模式。

    <meta http-equiv="X-UA-Compatible" content="IE=Edge">
    
### 5.字符编码
通过明确声明字符编码，能够确保浏览器快速并容易的判断页面内容的渲染方式。这样做的好处是，可以避免在 HTML 中使用字符实体标记（character entity），从而全部与文档编码一致（一般采用 UTF-8 编码）。

    <head>
        <meta charset="UTF-8">
    </head>

### 6.引入 CSS 和 JavaScript 文件
根据 HTML5 规范，在引入 CSS 和 JavaScript 文件时一般不需要指定 type 属性，因为 text/css 和 text/javascript 分别是它们的默认值。

    <!-- External CSS -->
    <link rel="stylesheet" href="code.css">
    
    <!-- JavaScript -->
    <script src="code.js"></script>

### 7.代码实用性
尽量遵循 HTML 标准和语义，但是不要以牺牲实用性为代价。任何时候都要尽量使用最少的标签并保持最小的复杂度。编写 HTML 代码时，尽量避免多余的父元素。很多时候，这需要迭代和重构来实现。

### 8.属性顺序

    class -> id,name -> data-* -> src,for,type,href,value -> 
    title,alt -> role,aria-*
    // class 用于标识高度可复用组件，因此应该排在首位。
    // id 用于标识具体组件，应当谨慎使用（例如，页面内的书签），因此排在第二位。
    
    <a class="..." id="..." data-toggle="modal" href="#">
        Example link
    </a>

    <input class="form-control" type="text">

    <img src="..." alt="...">

### 9.布尔（boolean）型属性
布尔型属性可以在声明时不赋值。

    <input type="text" disabled>

    <input type="checkbox" value="1" checked>

    <select>
        <option value="1" selected>1</option>
    </select>
    
### 10.尽量避免JS生成的标签
通过 `JavaScript` 生成的标签让内容变得不易查找、编辑，并且降低性能。能避免时尽量避免。

# CSS
### 1.语法
1.为选择器分组时，将单独的选择器单独放在一行。

2.为了代码的易读性，在每个声明块的左花括号前添加一个空格。声明块的右花括号应当单独成行。每条声明语句的` : `后应该插入一个空格。

3.为了获得更准确的错误报告，每条声明都应该独占一行。所有声明语句都应当以分号结尾。

4.对于以逗号分隔的属性值，每个逗号后面都应该插入一个空格。不要在 `rgb()`、`rgba()`、`hsl()`、`hsla()` 或 `rect()` 值的内部的逗号后面插入空格。这样利于从多个`属性值`（既加逗号也加空格）中区分多个`颜色值`（只加逗号，不加空格）。

5.对于属性值或颜色参数，省略小于 `1` 的小数前面的 `0` （例如，`.5` 代替 `0.5`；`-.5px` 代替 `-0.5px`）。

6.尽量使用简写形式的十六进制值，例如，用 `#fff` 代替 `#ffffff`。十六进制值应该全部小写，例如，`#fff`。在扫描文档时，小写字符易于分辨，因为他们的形式更易于区分。

7.为选择器中的属性添加双引号，例如，`input[type="text"]`。只有在某些情况下是可选的，但是，为了代码的一致性，建议都加上双引号。

8.避免为 0 值指定单位，例如，用 `margin: 0;` 代替 `margin: 0px;`。

    /* Good CSS */
    .selector,
    .selector-secondary,
    .selector[type="text"] {
        padding: 15px;
        margin-bottom: 15px;
        background-color: rgba(0,0,0,.5);
        box-shadow: 0 1px 2px #ccc, inset 0 1px 0 #fff;
    }

### 2.属性顺序
相关的属性声明应当归为一组，并按照下面的顺序排列：

    1.Positioning(定位)
    2.Box model(盒模型)
    3.Typographic
    4.Visual
    5.Misc
    
    例如：
    .declaration-order {
        /* Positioning */
        position: absolute;
        top: 0;
        right: 0;
        bottom: 0;
        left: 0;
        z-index: 100;

        /* Box-model */
        display: block;
        float: right;
        width: 100px;
        height: 100px;

        /* Typography */
        font: normal 13px "Helvetica Neue", sans-serif;
        line-height: 1.5;
        color: #333;
        text-align: center;

        /* Visual */
        background-color: #f5f5f5;
        border: 1px solid #e5e5e5;
        border-radius: 3px;

        /* Misc */
        opacity: 1;
    }
    
### 3.不要使用 @import
与 `<link>` 标签相比，`@import` 指令要慢很多，不光增加了额外的请求次数，还会导致不可预料的问题。替代办法有以下几种：

    1.使用多个 <link> 元素
    2.通过 Sass 或 Less 类似的 CSS 预处理器将多个 CSS 文件编译为一个文件
    3.通过 Rails、Jekyll 或其他系统中提供过 CSS 文件合并功能

### 4.媒体查询（Media query）的位置
将媒体查询放在尽可能相关规则的附近。不要将他们打包放在一个单一样式文件中或者放在文档底部。如果你把他们分开了，将来只会被大家遗忘。下面给出一个典型的实例：

    .element { ... }
    .element-avatar { ... }
    .element-selected { ... }
    
    @media (min-width: 480px) {
      .element { ...}
      .element-avatar { ... }
      .element-selected { ... }
    }

### 5.带前缀的属性
当使用特定厂商的带有前缀的属性时，通过缩进的方式，让每个属性的值在垂直方向对齐，这样便于多行编辑。

    /* Prefixed properties */
    .selector {
      -webkit-box-shadow: 0 1px 2px rgba(0,0,0,.15);
              box-shadow: 0 1px 2px rgba(0,0,0,.15);
    }

### 6.单行声明
对于只包含一条声明的样式，为了易读性和便于快速编辑，建议将语句放在同一行。

    .icon           { background-position: 0 0; }
    .icon-home      { background-position: 0 -20px; }
    .icon-account   { background-position: 0 -40px; }
    
### 7.Less 和 Sass 中的嵌套
避免不必要的嵌套。这是因为虽然你可以使用嵌套，但是并不意味着应该使用嵌套。只有在必须将样式限制在父元素内（也就是后代选择器），并且存在多个需要嵌套的元素时才使用嵌套。

    // Without nesting
    .table > thead > tr > th { … }
    .table > thead > tr > td { … }
    
    // With nesting
    .table > thead > tr {
      > th { … }
      > td { … }
    }

### 8.class 命名
1.class 名称中只能出现小写字符和破折号（dashe）（不是下划线，也不是驼峰命名法）。

2.class 名称应当尽可能短，并且意义明确。.btn 代表 button。

3.基于最近的父 class 或基本（base） class 作为新 class 的前缀。

### 9.组织代码
1.以组件为单位组织代码段。

2.制定一致的注释规范。

3.如果使用了多个 CSS 文件，将其按照组件而非页面的形式分拆，因为页面会被重组，而组件只会被移动。

    /*
     * Component section heading
     */
    
    .element { ... }
    
    
    /*
     * Component section heading
     *
     * Sometimes you need to include optional context for the entire component. Do that up here if it's important enough.
     */
    
    .element { ... }
    
    /* Contextual sub-component or modifer */
    .element-heading { ... }