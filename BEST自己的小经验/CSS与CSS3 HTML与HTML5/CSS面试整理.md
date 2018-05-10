> 目录
> 
> 1. CSS 选择符有哪些？哪些属性可以继承？优先级算法如何计算？CSS3新增伪类有那些？ 
> 2. css sprite是什么,有什么优缺点？
> 3. `display:none`和`visibility:hidden`的区别？
> 4. CSS中 `link` 和 `@import` 的区别是？
> 5. `display: block;` 和 `display: inline;` 的区别？
> 6. CSS的继承属性
> 7. 解释下浮动和它的工作原理？浮动元素引起的问题和解决办法？清除浮动的技巧？
> 8. 居中
> 9. 介绍一下box-sizing属性？
> 10. CSS3有哪些新特性？
> 
> 附录：
> > 1.css hack原理
> > 
> > 2.IE6的常见bug，缺陷或者与标准不一致的地方，如何解决
> > 
> > 3.flex布局
> > 
> > 4.双飞翼布局和圣杯布局
> > 
> > 5.BFC（块级格式化上下文）

## 1.CSS 选择符有哪些？哪些属性可以继承？优先级算法如何计算？CSS3新增伪类有那些？

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

## 2.css sprite是什么,有什么优缺点？
概念：将多个小图片拼接到一个图片中。通过background-position和元素尺寸调节需要显示的背景图案。

优点：

1. 减少HTTP请求数，极大地提高页面加载速度
2. 增加图片信息重复度，提高压缩比，减少图片大小
3. 更换风格方便，只需在一张或几张图片上修改颜色或样式即可实现

缺点：

1. 图片合并麻烦
2. 维护麻烦，修改一个图片可能需要从新布局整个图片，样式

## 3.`display:none`和`visibility:hidden`的区别？
**`display:none`**：

非继承属性，子孙节点也会消失不可见。读屏器不会读取`display: none;`元素的内容，会让元素完全从渲染树中消失，渲染的时候不占据任何空间。修改常规流中元素的display通常会造成文档重排。
    
**`visibility:hidden`**：

继承属性，设置`visibility: visible;`可以让子孙节点显示。读屏器会读取`visibility: hidden;`元素内容，不会让元素从渲染树消失，渲染的元素继续占据空间，只是内容不可见。修改`visibility`属性只会造成本元素的重绘。

## 4.CSS中 `link` 和 `@import` 的区别是？
1. `link`属于HTML标签，而`@import`是CSS提供的；
2. `link`最大限度支持并行下载，`@import`过多嵌套导致串行下载，出现`FOUC`；
3. `link`可以通过`rel="alternate stylesheet"`指定备选样式；
4. `link`方式的样式的权重高于`@import`的权重，`@import`在css文件中引用其他文件必须在样式规则之前。

## 5.`display: block;` 和 `display: inline;` 的区别？
**`block（块）`元素的特点**：

1. 总是在新行上开始，独占一个水平空间；
2. 没有设置`width`，自动填充满父容器；没有设置`height`，会根据内部元素自动扩展高度；
3. 可以使用`margin/padding`；
4. 内部可以容纳内联元素和其他块元素。
    
常见block元素：

    div form h1-h6 hr p pre table ul ol header footer main aside address noscript

**`inline`元素的特点**：

1. 不会在元素前后进行换行；
2. `margin/padding`在竖直方向上无效，水平方向上有效；
3. `width/height`属性对非替换行内元素无效，宽度由元素内容决定，非替换行内元素的行框高度由`line-height`确定；
4. 浮动或绝对定位时会转换为`block`；
5. 内联元素只能容纳文本或者其他内联元素。
    
常见inline元素：

    a br img input label select span textarea

▲ 行内元素的替换元素：`img`、`input`、`select`、`textarea`,可以为其设置宽高，并且margin属性也是对其起作用,有点类似于类似于`inline-block`的行为。

`inline-block`：通俗来说，就是不独占一行的块级元素（可以设置`宽高/margin/padding`等）。

## 6. CSS的继承属性
- 关于文字排版的属性如：
	
		font  word-break  letter-spacing  text-align  text-rendering  word-spacing
		white-space  text-indent  text-transform  text-shadow

- line-height

- color

- visibility

- cursor

## 7. 解释下浮动和它的工作原理？浮动元素引起的问题和解决办法？清除浮动的技巧？
- **浮动的工作原理**：浮动元素脱离文档流，不占据空间，碰到包含它的边框或者浮动元素的边框停留。
- **浮动元素引起的问题和解决办法**：

	1. 父元素的高度无法被撑开，影响与父元素同级的元素。
    2. 与浮动元素同级的非浮动元素（内联元素）会跟随其后。
    3. 若非第一个元素浮动，则该元素之前的元素也需要浮动，否则会影响页面显示的结构。

	对于2、3问题可以使用`CSS`中的`clear:both`解决。对于问题1，给父元素添加`clearfix`样式：

    	.clearfix:after{
			content: ".";
			display: block;
			height: 0;
			clear: both;
			visibility: hidden;
		}
    	.clearfix{display: inline-block;} /* for IE/Mac */

- **清除浮动的几种方法（常用前三种）**：
	
	1.额外标签法：在最后额外添加一个标签，`<div style="clear:both;"></div>`（缺点：这个办法会增加额外的标签使HTML结构看起来不够简洁。）；
    
    2.使用after伪类：

	    .contentBox:after{
	        content:".";
	        height:0;
	        visibility:hidden;
	        display:block;
	        clear:both;
	    }
    
    3.父级元素设置`overflow`为`hidden`或者`auto`；
    
    4.父级元素浮动或者绝对定位脱离文档流；
    
    5.父级元素设置新的高度。

## 8.居中
**水平居中**：

1.对于`inline`元素：为父元素设置`text-align: center;`即可（子元素所有内容都会居中，所以最好在子元素中使用`text-align:left;`归位）。

2.对于`block`元素：为元素设置宽度，再设置`margin: 0 auto;`（IE6还是需要在父元素设置`text-align: center;`）

3.对于`float`元素：为元素设置宽度，再设置`position:relative;`，再设置`left:50%;`，最后`margin-left`设置为该元素宽度的一半乘以-1。

	.content {
		height: 30px;background-color: rgb(89, 157, 197); /* 无关紧要的代码 */
		float: left;
		width: 300px;
		position: relative;
		left: 50%;
		margin-left: -150px;
	}
4.对于`position:absolute`元素：
	
- 法一：为元素设置宽度，再设置`left:50%;`，最后`margin-left`设置为该元素宽度的一半乘以-1。
- 法二：为元素设置宽度，再设置左右偏移为0（`left: 0;`和`right: 0;`），最后设置左右外边距为`margin: 0 auto;`。

**垂直居中**：

1.对于单行文本垂直居中：设置高度，再设置`line-height`值等于设置的高度值。

2.父容器高度不知，两种方法：

- 法一：父容器设置`position:relative;`，子容器设置`position: absolute; top: 50%; transform: translateY(-50%);`。

		.main {
			position: relative;
			width:100%;height: 500px;background-color: rgb(199, 196, 43); /* 无关紧要的代码 */
		}
		.content {
			background-color: rgb(89, 157, 197); /* 无关紧要的代码 */
			position: absolute; top: 50%; transform: translateY(-50%);
		}

- 法二：（父容器下只有这个子元素时使用）子容器设置`position: relative; top: 50%; transform: translateY(-50%);`。

		.main {	
			width:100%;height: 500px;background-color: rgb(199, 196, 43); /* 无关紧要的代码 */
		}
		.content {
			background-color: rgb(89, 157, 197); /* 无关紧要的代码 */
			position: relative;top: 50%; transform: translateY(-50%);
		}

注：`transform: translate`中的`translate`是根据自身百分比宽高在X/Y轴上移动。所以如果在子元素使用`position: absolute;left:50%; transform:translate(-50%,0);`则可以实现水平居中。

▲ 3.`flex`简单粗暴：

	.main{
		width: 100%; height: 400px; background-color: aqua; /* 无关紧要的代码 */
	    display:flex;/*Flex布局*/
	    display: -webkit-flex; /* Safari */
	    align-items:center;/*指定垂直居中*/
	    // justify-content:center; /*指定水平居中*/
	}
	.content {
		width: 200px;height: 200px;background-color: rgb(89, 157, 197); /* 无关紧要的代码 */
	}

## 9.介绍一下box-sizing属性？

`box-sizing`属性主要用来控制元素的盒模型的解析模式。默认值是`content-box`。

- `content-box`：让元素维持W3C的标准盒模型。元素的宽度/高度由`border + padding + content`的宽度/高度决定，设置`width/height`属性指的是`content`部分的宽/高。

- `border-box`：让元素维持IE传统盒模型（IE8版本以下）。设置`width/height`属性指的是`border + padding + content`。

注：W3C盒模型指的是`content+padding+border+margin`。

## 10.CSS3有哪些新特性？

1. CSS3实现圆角（border-radius），阴影（box-shadow），对文字加特效（text-shadow、），
2. 线性渐变（linear-gradient），径向渐变（radial-gradient）
3. `transform`：

    	// 依次是旋转,缩放,定位,倾斜
        transform:rotate(9deg); // 旋转
        transform:scale(0.85,0.90); // 缩放
        transform:translate(0px,-30px); // 定位
        transform:skew(-9deg,0deg); // 倾斜
4. [增加了更多的CSS选择器](http://www.w3school.com.cn/cssref/css_selectors.asp/)，比如：`::selection`等。
5. 背景(新增背景定位区域background-origin和多重背景)
6. rgba(red,green,blue,alpha)(alpha代表透明度，取值0-1之间)
7. 媒体查询：@media max-width min-width
8. 多栏显示：columns 通常适用于大段文字
9. 图片边框：border-image

一些其他问题
---
### 1.css hack原理
原理：利用不同浏览器对CSS的支持和解析结果不一样编写针对特定浏览器样式。

### 2.IE6的常见bug，缺陷或者与标准不一致的地方，如何解决
- E6不支持`min-height`，可以使用`css hack`：

		.target {
		    min-height: 100px;
		    height: auto !important;
		    height: 100px;   /* 最小高度为100px，IE6下内容高度超过会自动扩展高度 */
		}

- `ol`内`li`的序号全为1，不递增。解决方法：为`li`设置样式`display: list-item;`
- IE5-8不支持`opacity`，解决办法：

		.opacity {
		    opacity: 0.4
		    filter: alpha(opacity=60); /* for IE5-7 */
		    -ms-filter: "progid:DXImageTransform.Microsoft.Alpha(Opacity=60)"; /* for IE 8*/
		}
- 通过为块级元素设置宽度和左右`margin:auto;`时，IE6不能实现水平居中，解决方法：为父元素设置`text-align: center;`
- ...

### 3.flex布局
有道云笔记

### 4.双飞翼布局和圣杯布局
有道云笔记

### 5.BFC（块级格式化上下文）
-  [BFC（Block Formatting Contexts），块级格式上下文。](http://www.cnblogs.com/lhb25/p/inside-block-formatting-ontext.html/)

	    含义：它是页面上的一个隔离的渲染区域，容器里面的子元素不会在布局上影响到外面的元素，而外面的元素也不会影响到里面的元素。
	    
	    条件：float的值不为none;
	    overflow的值不为visible;
	    position的值为absolute或者fixed;
	    display的值为inline-block,table-cell,table-caption，flex,inline-flex。
	    
	    作用：1.自适应两栏布局（BFC区域不会与float box重叠）；
	    2.触发父元素生成BFC清除内部浮动；(overflow:hidden;)
	    3.防止垂直margin重叠（在外部包裹容器，over-flow:hidden;）。