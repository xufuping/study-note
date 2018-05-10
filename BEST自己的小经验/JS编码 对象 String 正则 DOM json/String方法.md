> 目录
>
> 1.String 对象方法

### 1.String对象方法

方法 | 描述
---|---
charAt() | 返回在指定位置的字符。
▲ charCodeAt() | 返回在指定的位置的字符的 Unicode 编码。
concat() | 以打字机文本显示字符串。
fromCharCode() | 从字符编码创建一个字符串。
indexOf() | 检索字符串。
lastIndexOf() | 从后向前搜索字符串。
link() | 将字符串显示为链接。
▲ replace() | 替换与正则表达式匹配的子串。
slice() | 提取字符串的片断，并在新的字符串中返回被提取的部分。
▲ split() | 把字符串分割为字符串数组。
substr() | 从起始索引号提取字符串中指定数目的字符。
toLowerCase() | 把字符串转换为小写。
toUpperCase() | 把字符串转换为大写。

    var str1 = "Hello world!";
    var str2 = "XQF.";
    str1.charAt(1); // e
    str1.charCodeAt(1); // 101(e的Unicode编码是101)
    str1.concat(str2); // Hello world!XQF.
    String.fromCharCode(65,66,67); // ABC
    
    // str.indexOf(searchvalue,fromindex), fromindex规定在字符串中开始检索的位置
    str1.indexOf("world"); // 6
    
    // str.lastIndexOf(searchvalue,fromindex)
    str1.lastIndexOf("world"); // 6
    
    str1.link("https://www.baidu.com"); // 字符串可以链接到这个地址
    str1.replace(/world/, "W3School"); // Hello W3School!
    str1.slice(6); // world!
    
    /*
    str.split(separator,howmany), 
    howmany该参数可指定返回的数组的最大长度。如果设置了该参数，返回的子串不会多于这个参数指定的数组。如果没有设置该参数，整个字符串都会被分割，不考虑它的长度。
    如果把空字符串 ("") 用作 separator，那么 stringObject 中的每个字符之间都会被分割。
    String.split() 执行的操作与 Array.join 执行的操作是相反的。join() 方法用于把数组中的所有元素放入一个字符串: arr1.join()。
    */
    str1.split(" "); // hello,world!
    
    str1.substr(3,7); // lo worl
    str1.toLowerCase(); // hello world!
    str1.toUpperCase(); // HELLO WORLD!
