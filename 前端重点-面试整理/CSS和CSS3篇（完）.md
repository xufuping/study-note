# CSS和CSS3相关问题
--------
# 本章问题汇总：

> 1.display:none和visibility:hidden的区别？
> 
> 2.CSS中 link 和@import 的区别是？
> 
> 3.position的值relative，fixed，absolute分别是相对于谁进行定位的？
> 
> 4.position:absolute和float属性的异同？
> 
> 5.介绍一下box-sizing属性？
> 
> 6.CSS 选择符有哪些？哪些属性可以继承？优先级算法如何计算？CSS3新增伪类有那些？
> 
> 7.CSS3有哪些新特性？
> 
> 8.什么是BFC、IFC、GFC、FFC（CSS3才有了GFC和FFC）？
> 
> 9.行内元素块级元素内联元素有哪些？有些什么区别？
> 
> 10.解释下浮动和它的工作原理？浮动元素引起的问题和解决办法？清除浮动的技巧？
> 
> 11.什么叫优雅降级和渐进增强？

## 1.display:none和visibility:hidden的区别？

    display:none  隐藏对应的元素，在文档布局中不再给它分配空间，它各边的元素会合拢，
    就当他从来不存在。
    
    visibility:hidden  隐藏对应的元素，但是在文档布局中仍保留原来的空间。

## 2.CSS中 link 和@import 的区别是？

    (1) link属于HTML标签，而@import是CSS提供的; 
    (2) 页面被加载的时，link会同时被加载，而@import引用的CSS会等到页面被加载完再加载;
    (3) import只在IE5以上才能识别，而link是HTML标签，无兼容问题; 
    (4) link方式的样式的权重 高于@import的权重.

## 3.position的值relative，fixed，absolute分别是相对于谁进行定位的？

    absolute 生成绝对定位的元素， 相对于最近一级的 定位不是 static 的父元素来进行定位。

    fixed （老IE不支持）生成绝对定位的元素，相对于浏览器窗口进行定位。 

    relative 生成相对定位的元素，相对于其在普通流中的位置进行定位。 

    static  默认值。没有定位，元素出现在正常的流中
    
## 4.position:absolute和float属性的异同？
    
    共同点：
    对内联元素设置`float`和`absolute`属性，可以让元素脱离文档流，并且可以设置其宽高。
    
    不同点：
    float仍会占据位置，absolute会覆盖文档流中的其他元素。

## 5.介绍一下box-sizing属性？

> `box-sizing`属性主要用来控制元素的盒模型的解析模式。默认值是`content-box`。

- `content-box`：让元素维持W3C的标准盒模型。元素的宽度/高度由border + padding + content的宽度/高度决定，设置width/height属性指的是content部分的宽/高。（标准浏览器下，按照W3C规范对盒模型解析，一旦修改了元素的边框或内距，就会影响元素的盒子尺寸，就不得不重新计算元素的盒子尺寸，从而影响整个页面的布局。） 

- `border-box`：让元素维持IE传统盒模型（IE6以下版本和IE6~7的怪异模式）。设置width/height属性指的是border + padding + content

> IE 8以下版本的浏览器中的盒模型有什么不同？

- `W3C盒子模型`的范围包括margin、border、padding、content，并且 content 部分不包含其他部分。
- IE8以下的为`IE盒子模型`，范围也包括margin、border、padding、content，和标准`W3C盒子模型`不同的是：`IE盒子模型`的 content 部分包含了 border 和 pading。

## 6.CSS 选择符有哪些？哪些属性可以继承？优先级算法如何计算？CSS3新增伪类有那些？

   
    1.id选择器（ # myid）
    2.类选择器（.myclassname）
    3.标签选择器（div, h1, p）
    4.相邻选择器（h1 + p）
    5.子选择器（ul > li）
    6.后代选择器（li a）
    7.通配符选择器（ * ）
    8.属性选择器（a[rel = "external"]）
    9.伪类选择器（a: hover, li:nth-child）
          
  *  可继承的样式： font-size font-family color, text-indent(首行文本缩进);
          
  *  不可继承的样式：border padding margin width height ;
      
>优先级为:
    
    优先级就近原则，同权重情况下样式定义最近者为准；载入样式以最后载入的定位为准。

    !important >  id > class > tag  
      
    （important 比 内联优先级高,但内联比 id 要高）
    
>CSS3新增伪类举例：
    
    p:first-of-type 选择属于其父元素的首个 <p> 元素的每个 <p> 元素。
    p:last-of-type  选择属于其父元素的最后 <p> 元素的每个 <p> 元素。
    p:only-of-type  选择属于其父元素唯一的 <p> 元素的每个 <p> 元素。
    p:only-child    选择属于其父元素的唯一子元素的每个 <p> 元素。
    p:nth-child(2)  选择属于其父元素的第二个子元素的每个 <p> 元素。
    :enabled  选择器匹配每个已启用的元素（大多用在表单元素上）。
    :disabled 选择器匹配每个被禁用的元素（大多用在表单元素上）。
    :checked  单选框或复选框被选中。
 



## 7.CSS3有哪些新特性？

> CSS3实现圆角（border-radius），阴影（box-shadow），对文字加特效（text-shadow、），
> 线性渐变（linear-gradient），径向渐变（radial-gradient）
    
> 旋转（transform）
> transform:rotate(9deg) scale(0.85,0.90) translate(0px,-30px)skew(-9deg,0deg);//旋转,缩放,定位,倾斜
    
> [增加了更多的CSS选择器](http://www.w3school.com.cn/cssref/css_selectors.asp/)，比如：::selection。
    
> 背景(新增背景定位区域background-origin和多重背景)

> rgba(red,green,blue,alpha)(alpha代表透明度，取值0-1之间)

> 媒体查询：@media max-width min-width

> 多栏布局：columns 适用于大段文字

> 图片边框：border-image



## 8.什么是BFC、IFC、GFC、FFC（CSS3才有了GFC和FFC）？

-  [BFC（Block Formatting Contexts），块级格式上下文。](http://www.cnblogs.com/lhb25/p/inside-block-formatting-ontext.html/)

    
    含义：它是页面上的一个隔离的渲染区域，容器里面的子元素不会在布局上影响到外面的元素，而外面的元素也不会影响到里面的元素。
    
    条件：float的值不为none;
    overflow的值不为visible;
    position的值为absolute或者fixed;
    display的值为inline-block,table-cell,table-caption，flex,inline-flex。
    
    作用：1.自适应两栏布局（BFC区域不会与float box重叠）；
    2.触发父元素生成BFC清除内部浮动；(overflow:hidden;)
    3.防止垂直margin重叠（在外部包裹容器，over-flow:hidden;）。
    
- IFC（Inline Formatting Contexts）,内联格式化上下文。

    
    水平居中：当一个块要在环境中水平居中时，设置其为inline-block则会在外层产生IFC，通过text-align:center;则可以使其水平居中。
    
    垂直居中：创建一个IFC，用其中一个元素撑开父元素高度，然后设置其vertical-align:middle;其他行内元素则可以在此父元素下垂直居中。

- GFC（GirdLayout Formatting Contexts），网格布局格式化上下文，设置display:gird。

    
    通过在网格容器中定义网格行和网格列为每一个网格项目定义位置和空间。
    与table表格有什么不同呢？同样是一个二维的表格，但GridLayout会有更加丰富的属性来控制行列，控制对齐以及更为精细的渲染语义和控制。
    
- FFC（Flex Formatting Contexts），自适应格式化上下文。也称为“弹性布局”。在各大浏览器兼容性不太好。


    含义：display为flex或inline-flex可以得到一个伸缩容器，设置为flex容器被渲染为一个块级元素，设置为inline-flex容器则渲染为一个行内元素。
    
    作用：flex定义了伸缩容器内伸缩项目的布局。在移动端更加适用。

## 9.行内元素块级元素内联元素有哪些？有些什么区别？
- block（块）元素的特点：


    1.总是在新行上开始；
    2.高度，行高以及外边距和内边距都可控制；
    3.宽度缺省是它的容器的100%，除非设定一个宽度;
    4.它可以容纳内联元素和其他块元素.
    
    常见block元素：
    div - 常用块元素，也是css layout的主要标签
    form - 交互表单
    h1 - 大标题，h2 - 副标题，h3 - 3级标题，h4 - 4级标题，h5 - 5级标题，h6 - 6级标题
    hr - 水平分隔线
    p - 段落
    table - 表格
    ul - 非排序列表
    address - 地址
    noscript - 可选脚本内容（对于不支持script的浏览器显示此内容）
    ol - 排序表单
    pre - 格式化文本

- inline元素的特点（中文叫法有多种内联元素、内嵌元素、行内元素、直进式元素）：


    1.和其他元素都在一行上；
    2.高，行高及外边距和内边距不可改变；
    3.宽度就是它的文字或图片的宽度，不可改变;
    4.内联元素只能容纳文本或者其他内联元素.
    
    常见inline元素：
    a - 锚点
    br - 换行
    img - 图片
    input - 输入框
    label - 表格标签
    select - 项目选择
    span - 常用内联容器，定义文本内区块
    textarea - 多行文本输入框
    
## 10.解释下浮动和它的工作原理？浮动元素引起的问题和解决办法？清除浮动的技巧？

- 浮动和它的工作原理


    浮动元素脱离文档流，不占据空间。
    浮动元素碰到包含它的边框或者浮动元素的边框停留。
    
    
- 浮动元素引起的问题和解决办法：


    （1）父元素的高度无法被撑开，影响与父元素同级的元素。
    （2）与浮动元素同级的非浮动元素（内联元素）会跟随其后。
    （3）若非第一个元素浮动，则该元素之前的元素也需要浮动，否则会影响页面显示的结构。

使用`CSS`中的`clear:both`;属性来清除元素的浮动可解决2、3问题。对于问题1，添加如下样式，给父元素添加`clearfix`样式：

    .clearfix:after{content: ".";display: block;height: 0;clear: both;visibility: hidden;}
    .clearfix{display: inline-block;} /* for IE/Mac */

- 清除浮动的几种方法（常用前三种）：


    1.额外标签法：
    在最后额外添加一个标签，<div style="clear:both;"></div>
    （缺点：不过这个办法会增加额外的标签使HTML结构看起来不够简洁。）
    
    2.使用after伪类
    .contentBox:after{
        content:".";
        height:0;
        visibility:hidden;
        display:block;
        clear:both;
    }
    
    3.父级元素设置overflow为hidden或者auto;
    
    4.父级元素浮动或者绝对定位脱离文档流;
    
    5.父级元素设置新的高度。

## 11.什么叫优雅降级和渐进增强？

> 优雅降级：Web站点在所有新式浏览器中都能正常工作，如果用户使用的是老式浏览器，则代码会检查以确认它们是否能正常工作。由于IE独特的盒模型布局问题，针对不同版本的IE的hack实践过优雅降级了,为那些无法支持功能的浏览器增加候选方案，使之在旧式浏览器上以某种形式降级体验却不至于完全失效.
   
> 渐进增强：从被所有浏览器支持的基本功能开始，逐步地添加那些只有新式浏览器才支持的功能,向页面增加无害于基础浏览器的额外样式和功能的。当浏览器支持时，它们会自动地呈现出来并发挥作用。