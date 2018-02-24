# 1.一些JSON基础知识
1.JSON是一种数据交换格式

2.为了获得最大可移植性，应尽可能避免使用空格或特殊字符。

3.使用下列字符，告诉机器如何读取数据：
    
    {  开始读取对象
    }  结束读取对象
    [  开始读取数组
    ]  结束读取数组
    :  在“名称——值对”中分隔名称和值
    ,  分隔对象中的“名称——值对”或者分隔“数组中的值”；也可以认为是“一个新部分得到开始”

4.双引号：
JSON是基于JavaScript的对象字面量的，所以和JavaScript对象字面量很像，但是JS对象字面量不需要给名称——值对中的名称两边加上双引号，而在JSON中，必须给两边都要加上双引号，且不可用单引号代替双引号。例如：
    
    //这不是合法的JSON
    {
        'title':"this is my title.",
        'body' :"this is the body."
    }
    //合法的JSON
    {
        "title": "this is my title.",
        "body" : "this is the body."
    }
5.JSON的MIME类型是 application/json
# 2.JSON的数据类型
1.包含：对象、字符串、数字、布尔值、null、数组

2.JSON本身就是对象，也就是一个被花括号包裹的名称——值对的列表，如果你希望在作为对象的JSON中创建一个名称——值对，那就需要用到嵌套。
    
    实例3-3：嵌套对象
    {
        "person": {
            "name": "linda",
            "heightInInches": 66,
            "head":{
                "hair":{
                    "color": "yellow",
                    "length": "short",
                    "style": "A-line"
                },
                "eyes":"green"
            }
        }
    }
3.使用反斜线对字符串中的双引号进行转义

    \"\"（代表""）
    \\（代表\）
    \/（正斜线）
    \b（退格符）
    \f（换页符）
    \t（制表符）
    \n（换行符）
    \r（回车符）
    \u（后面跟十六进制符 例如：笑脸表情 \u263A）
4.JSON中的null类型

null就是用来表示0、一无所有、不存在等意思，而不是用数字来表示。

不能把null和undefined混淆，undefined不是JSON中的数据类型。在JS中，undefined是在尝试获取一些不存在的对象或变量时返回的结果。

null是一个“没有值”的值，在JSON中，null必须使用小写形式。

5.JSON中的数组例子：
    
    //例一：使用由对象构成的数组来表示一场考试的问题和答案
    {
        "test":[
            {
                "question": "the sky is blue.",
                "answer": true
            },
            {
                "question": "the earth is flat.",
                "answer": false
            },
            {
                "question": "A cat is a dog.",
                "answer": false
            }
        ]
    }
    
    //例二：使用由数组构成的数组来表示三场不同考试的答案
    {
        "test":[
            [
                true,
                false,
                false,
                false
            ],
            [
                true,
                true,
                true,
                true,
                false
            ],
            [
                true,
                false,
                true
            ],
        ]
    }
备注：

1.对象和数组很关键的一个区别就是，对象是名称——值对构成的列表或集合，数组是值构成的列表集合。

2.对象和数组另一个关键的区别是，数组中所有的值应具有相同的数据类型。
# 3.JSON Schema
交互时需要响应，我们称为“一致性验证”，Schema（意为模式）

作用：
    
    1.通过验证数据与Schema的一致性，你轻松地定位和修复了错误。你从每个错误中都能得到许多有用的信息。
    2.创建好了数据并且信心十足。
    3.你从网上将数据发送过去，并收到了成功响应。任务完成。
使用Schema：

    1.声明的名称必须是"$schema",它的值必须为所用版本的链接："http://json-schema.org/draft-04/schema#"
    2.第二个名称-值对应该是JSON Schema文件的标题。例如：
    {
        "$schema": "http://json-schema.org/draft-04/schema#",
        "title": "Cat"
    }
    3.定义对象属性，例如：
    {
        "$schema": "http://json-schema.org/draft-04/schema#",
        "title": "Cat",
        "properties": {
            "name": {
                "type": "string"
            },
            "age": {
                "type": "number",
                "description": "Your cat's age in years."
            },
            "declawed": {
                "type": "boolean"
            }
        }
    }
    4.最后验证JSON是否符合JSON Schema.
    //下列JSON符合定义的JSON Schema
    {
        "name": "Fluffy",
        "age": 2,
        "declawed": false
    }
    这里只是验证了值的数据类型是否正确。
验证规定的一定要包含的JSON对象字段——required.

    {
        "$schema": "http://json-schema.org/draft-04/schema#",
        "title": "Cat",
        "properties": {
            "name": {
                "type": "string"
            },
            "age": {
                "type": "number",
                "description": "Your cat's age in years."
            },
            "declawed": {
                "type": "boolean"
            },
            "description": {
                "type": "string"
            }
        },
        "required": [
            "name",
            "age",
            "declawed"
        ]
    }
    //针对上面的合法JSON
    {
        "name": "Fluffy",
        "age": 2,
        "declawed": false,
        "description": "Fluffy loves to sleep all day."
    }//因为required没有要求description，如果没有description，也是合法的JSON.

    但是下面这个也是合法的JSON:
    
    {}//因为这里对JSON没有值的形式要求。
更详细的对JSON类型的格式要求

    {
        "$schema": "http://json-schema.org/draft-04/schema#",
        "title": "Cat",
        "properties": {
            "name": {
                "type": "string",
                "minLength": 3,
                "maxLength": 20
            },//名字的范围不少于3个字符，不多于20个字符
            "age": {
                "type": "number",
                "description": "Your cat's age in years.",
                "minumum": 0
            },//这里保证小猫的年龄不为负数
            "declawed": {
                "type": "boolean"
            },
            "description": {
                "type": "string"
            }
        },
        "required": [
            "name",
            "age",
            "declawed"
        ]
    }
    
    //这是不能通过验证的JSON
    {
        "name": "xi",//名字最少3个字符
        "age": -1,//年龄最小值为0
        "declawed": false,
        "description": "Fluffy loves to sleep all day."
    }
补充：JSON还支持正则表达式以及枚举类型（一个包含所有科能值的列表）。