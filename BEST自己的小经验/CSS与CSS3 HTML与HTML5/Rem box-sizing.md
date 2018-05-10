> 目录
>
> rem
>
> box-sizing

### rem
`rem`用来定义字体大小，将根据页面上的根元素（一般是html元素）的字体大小而计算出实际的字体大小。

`em`单位根据元素的父元素的字体大小而计算出实际的字体大小。

使用：

    html { font-size: 10px; }
    small { font-size: 1.1rem; }
    
大多数浏览器中默认字体为16个像素，针对默认字大小，可以将根元素的字体大小指定为62.5%，从而使浏览器计算出10个像素。这样用户放大默认字体时，也可以使所有元素的字体大小自动放大。

    html { font-size: 62.5%; }
    small { font-size: 1.1rem; }

### box-sizing
`box-sizing`的属性值有`content-box`，`border-box`。`content-box`表示元素的宽度与高度不包括内部内边距与边框；`border-box`表示元素的宽度与高度包括内部内边距与边框。样式中默认使用的是`content-box`。

只要将两个具有`border-box`属性的盒子都设置为50%，就能保证两个盒子并列显示了。
