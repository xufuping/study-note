## 1.基本概念
采用 Flex 布局的元素，称为 Flex 容器（flex container），简称"容器"。它的所有子元素自动成为容器成员，称为 Flex 项目（flex item），简称"项目"。

容器默认存在两根轴：水平的主轴（main axis）和垂直的交叉轴（cross axis）。主轴的开始位置（与边框的交叉点）叫做`main start`，结束位置叫做`main end`；交叉轴的开始位置叫做`cross start`，结束位置叫做`cross end`。
项目默认沿主轴排列。单个项目占据的主轴空间叫做`main size`，占据的交叉轴空间叫做`cross size`。

![容器图](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071004.png)

## 2.容器的属性

属性 | 含义
---|---
flex-direction | 决定主轴的方向
flex-wrap | 项目不在一条轴线如何换行
flex-flow | flex-direction 和 flex-wrap 的简写形式
justify-content | 定义项目在主轴上的对齐方式
align-items | 定义项目在交叉轴上如何对齐
align-content | 定了多根轴线的项目对齐方式

### 2.1 flex-direction : 决定主轴的方向

    .box {
        flex-direction: row | row-reverse | column | column-reverse;
    }


值 | 含义
---|---
row(默认) | 主轴为水平方向，起点在左端。
row-reverse | 主轴为水平方向，起点在右端。
column | 主轴为垂直方向，起点在上沿。
column-reverse | 主轴为垂直方向，起点在下沿。

### 2.2 flex-wrap : 项目不在一条轴线如何换行

    .box {
        flex-wrap: nowrap | wrap | wrap-reverse;
    }
    
值 | 含义
---|---
nowrap(默认) | 不换行
wrap | 换行，第一行在上方，新内容在下方（按顺序添加内容）
wrap-reverse | 换行，第一行在下方，新内容在上方

### 2.3 flex-flow : flex-direction 和 flex-wrap 的简写形式
默认值为`flex-flow: row nowrap`

    .box {
        flex-flow: flex-direction || flex-wrap;
    }

### 2.4 justify-content : 定义项目在主轴上的对齐方式

    .box {
        justify-content: flex-start | flex-end | center | space-between | space-around;
    }

值 | 含义
---|---
flex-start(默认值) | 左对齐
flex-end | 右对齐
center | 居中
space-between | 两端对齐，项目之间的间隔相等
space-around | 每个项目两侧的间隔相等。(因为中间间隔是两个间隔之和，所以项目之间的间隔比项目与边框的间隔大一倍)

### 2.5 align-items : 定义项目在交叉轴（竖轴）上如何对齐

    .box {
        align-items: flex-start | flex-end | center | baseline | stretch;
    }
    
值 | 含义
---|---
flex-start | 交叉轴的起点对齐
flex-end | 交叉轴的终点对齐
center | 交叉轴的中点对齐
baseline | 项目第一行文字的基线对齐
stretch(默认值) | 如果项目未设置高度或设为auto，将占满整个容器的高度

### 2.6 align-content : 定了多根轴线的项目对齐方式
如果项目只有一根轴线，该属性不起作用。

    .box{
        align-content: flex-start | flex-end | center | space-between | space-around | stretch;
    }

值 | 含义
---|---
flex-start | 与交叉轴(竖轴)起点对齐(项目在容器纵向方向顶部)
flex-end | 与交叉轴(竖轴)终点对齐(项目在容器纵向方向底部)
center | 与交叉轴(竖轴)中点对齐(项目在容器纵向方向中间)
space-between | 与交叉轴(竖轴)两端对齐，轴线之间间隔平均分布
space-around | 在纵向方向每根轴线两侧间隔相等。(因为中间间隔是两个间隔之和，所以项目之间的间隔比项目与边框的间隔大一倍)
stretch(默认值) | 轴线占满整个交叉轴

## 3.项目的属性
以下属性设置在项目上：

属性 | 含义
---|---
order | 定义项目排列顺序，数值越小排列越靠前，默认为`0`
flex-grow | 定义项目的放大比例，默认为`0`
flex-shrink | 定义项目的缩小比例，默认为`1`
flex-basis | 定义了分配多余空间之前，项目占据的主轴空间。浏览器根据这个属性，计算主轴是否有多余空间。默认值为`auto`
flex | 是`flex-grow`、`flex-shrink`、`flex-basis`的简写，默认值为`0 1 auto`
align-self | 允许单个项目与其他项目不一样的对齐方式，可覆盖`align-items`，默认值为`auto`

### 3.1 order : 定义项目排列顺序
定义项目排列顺序，数值越小排列越靠前，默认为`0`

    .item {
        order: <integer>;
    }

### 3.2 flex-grow : 定义项目的放大比例
定义项目的放大比例，默认为`0`

    .item {
       flex-grow: <number>; /* default 0 */
    }

### 3.3 flex-shrink : 定义项目的缩小比例
定义项目的缩小比例，默认为`1`

    .item {
        flex-shrink: <number>; /* default 1 */  
    }

### 3.4 flex-basis : 定义了分配多余空间之前，项目占据的主轴空间
定义了分配多余空间之前，项目占据的主轴空间。浏览器根据这个属性，计算主轴是否有多余空间。默认值为`auto`，即项目的本来大小。可以设置跟`width`和`height`属性一样的值(比如150px)

    .item {
        flex-basis: <length> | auto; /* default auto */
    }

### 3.5 flex : `flex-grow`、`flex-shrink`、`flex-basis`的简写
是`flex-grow`、`flex-shrink`、`flex-basis`的简写，默认值为`0 1 auto`

    .item {
        flex: none | [ <`flex-grow`> <`flex-shrink`> || <`flex-basis`>]
    }
    // 这个属性有两个推荐使用的快捷值： auto(1 1 auto) 和 none(0 0 auto)

### 3.6 align-self : 允许单个项目与其他项目不一样的对齐方式
允许单个项目与其他项目不一样的对齐方式，可覆盖`align-items`。默认值为`auto`，表示继承父元素的`align-items`属性，如果没有父元素，则等同与`stretch`。除了`auto`属性其余属性和`align-items`属性一致。

    .item {
        align-self: auto | flex-start | flex-end | center | baseline |stretch;
    }

