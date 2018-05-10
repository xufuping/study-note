JSON
---
### 1 语法

`JSON`，一种表示结构化数据的格式，`JSON`字符串必须使用双引号，不支持变量、函数或对象实例。

**简单值**：可以在 `JSON` 中表示字符串、数值、布尔值和 `null`，但不支持 `js` 中的 `undefined` .

**对象**：与`js`对象字面量相比，没有声明变量，没有末尾的分号。json对象的属性必须加双引号，同一个对象中绝对不应该有两个同名属性。

    {
        "name": "XuQingfeng",
        "age": 21,
        "school": {
            "name": "cqupt"
        }
    }

**数组**：`JSON`也没有变量和分号。

    [
        {
            "name": "XuShao",
            "age": 21
        },
        {
            "name": "Xushuai",
            "age": 27
        }
    ]

### 2 解析与序列化
#### 2.1 JSON对象

*`eval()`函数可以解析、解释JSON并返回`js`对象或数组，但可能会执行一些恶意代码，不推荐使用`eval()`。*

`JSON`的两个方法： `stringify()` 将 `JS `对象序列化为 `JSON` 字符串， `parse()` 将 `JSON` 字符串解析为原生 `JS` 值。

#### 2.2 序列化选项
序列化对象时，所有函数以及原型成员都会被忽略，值为 `undefined` 的属性也会被跳过。

`JSON.stringify()` 接收三个参数，① 需要序列化的 `js对象`，② 过滤器，是数组或函数，③ 表示是否在 `JSON` 字符串中保留缩进。

一个JSON数据例子：

    var book = {
                    "name" : [
                        "xufuping",
                        "xushao"
                    ],
                    "age" : 20
            };

过滤器是数组：

    // 过滤JSON数据，结果中只有数组列出属性
    var jsonText = JSON.stringify(book, ["name", "age"]);
    
过滤器是函数：

    // 函数接收两个参数，键名和键值。
    var jsonText = JSON.stringify(book, function (key, value) {
        switch (key) {
            case "name" :
                return value.join(",") // 将数组连接为一个字符串
            case "age" : 
                return 20;
            // 最后一定要加 default, 返回传入的值，以便其他值正常出现在结果中
            default :
                return value;
        }
    });

关于缩进

    var jsonText = JSON.stringify(book, null, 4); // 每个级别缩进4个空格
    
toJSON() : `stringify()` 序列化一个对象时，首先检测如果存在 `toJSON()` 方法并能通过它取得有效值，则首先调用该方法。

    var book = {
                    "name" : [
                        "xufuping",
                        "xushao"
                    ],
                    "age" : 20,
                    toJSON: function () {
                        return this.name;
                    }
            };

#### 2.3 解析选项
`JSON.parse()` 接收两个参数，① `JSON` 格式的数据，② 一个还原函数，接收两个参数，一个键一个值。

    var book = {
                	"name" : [
                        "xufuping",
                        "xushao"
                    ],
                    "age" : 20,
                    releaseDate: new Date(2011, 11, 21)
            };
            
    // JS对象转为JSON数据格式
    var jsonText = JSON.stringify(book);

    // JSON转为JS对象
    var bookValue = JSON.parse(jsonText, function (key, value) {
    	if (key == "age") {
    		return 21;
    	} else {
    		return value;
    	}
    });
    console.log(bookValue);