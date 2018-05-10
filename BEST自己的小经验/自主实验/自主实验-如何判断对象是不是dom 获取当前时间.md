#### 如何判断对象是不是dom
```
const isDom = obj => obj instanceof HTMLElement
```

#### 获取当前时间

    var now = new Date();
    console.log([now.getFullYear(), now.getMonth()+1, now.getDate()].join('-'))
    // 2018-5-4
    