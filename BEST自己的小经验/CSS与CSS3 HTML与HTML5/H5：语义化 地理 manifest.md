> 目录
> 1. HTML语义化
>
> 2. 地理位置
> > 2.1 取得当前地理位置
> >
> > 2.2 position对象
>
> 3. manifest
>
> 4. viewport - 响应式web设计

### 1. HTML语义化
1.为了在没有CSS的情况下，页面也能呈现出很好地内容结构、代码结构。

2.利于SEO，语义化能和搜索引擎建立良好的联系，有利于爬虫抓取更多的有效信息。爬虫依赖于标签来确定上下文和各个关键字的权重。

3.便于团队开发和维护，语义化更具可读性，遵循W3C标准的团队都遵循这个标准，可以减少差异化。

4.方便其他设备解析（如屏幕阅读器、盲人阅读器、移动设备）以语义的方式来渲染网页。

### 2. 地理位置
H5中为`window.navigator`对象新增了一个`geolocation`属性，可以使用`Geolocation API`对其进行访问：

#### 2.1 取得当前地理位置
`getCurrentPosition`方法的定义：`navigator.geolocation.getCurrentPosition(onSuccess, onError, options);`
。第一个参数为获取当前位置成功后的回调函数；第二个参数为获取失败的回调函数；第三个参数为一些可选属性的列表。

其中第二个参数使用`error`对象作为参数，具有以下属性：

    code属性（为右侧三个值之一）：用户拒绝了位置服务（属性值为1）；获取不到位置信息（属性值为2）；获取信息超时错误（属性值为3）。
    
    message属性：是一个字符串，包含了错误信息，主要用于开发和调试。

第三个参数可以省略，可选属性如下：

    enableHighAccuracy：是否要求高精度的地理位置信息（一般不使用）。
    
    timeout：超时限制（单位毫秒）。如果在该时间内未获取到地理位置信息，则返回错误。
    
    maximumAge：对地理位置进行缓存的有效时间（单位毫秒）。
    
一个完整地获取当前地理位置函数：

    navigator.geolocation.getCurrentPosition(
        function(position) { // 参数position代表position对象，见2.3
            // 获取地理位置信息成功时的处理
        },
        function(error) {
            // 获取地理位置信息失败时的处理
        },
        {
            // 设置缓存有效时间为2分钟
            maximumAge: 60*1000*2,
            // 5秒内未获取到地理位置信息则返回错误
            timeout: 5000
        }
    );

**持续监视当前地理位置的信息：** `watchCurrentPosition(onSuccess, onError, options);`方法可以定期自动获取当前地理位置信息。

    var watchID = navigator.geolocation.watchPosition(function(position) {
        do_something(position.coords.latitude, position.coords.longitude);
    });
    
`watchPosition()` 函数会返回一个 ID，唯一地标记该位置监视器。您可以将这个 ID 传给 `clearWatch()` 函数来停止监视用户位置。

**停止获取当前页用户的地理位置信息：** `navigator.geolocation.clearWatch(watchID);`

#### 2.2 position对象

属性 | 含义
---|---
latitude | 当前地理位置的纬度
longitude | 当前地理位置的经度
altitude | 当前地理位置的海拔高度（不能获取时为null）
accuracy | 获取到的纬度或经度的精度
altitudeAccurancy | 获取到的海拔高度的精度
heading | 设备的前进方向。以面朝正北方向的顺时针旋转角度来表示（不能获取时为null）
speed | 设备的前进速度（以米/秒为单位，不能获取时为null）
timestamp | 获取地理位置信息时的时间

借用上面的例子改编一下：

    function get_location() {
        if (navigator.geolocation) {
            navigator.geolocation.getCurrentPosition(show_map, show_error, {maximumAge: 1000});
        } else {
            console.log("你的浏览器不支持使用HTML5获取地理位置信息。");
        }
    }

    function show_map(position) {
        // 显示地理信息
        var latitude = position.coords.latitude;
        var longitude = position.coords.longitude;
        console.log('Latitude : ' + latitude);
        console.log('Longitude: ' + longitude);
    }
    
    get_location();

### 3. manifest
`manifest`是一个文本文件，其中以清单的形式列举了需要被缓存或不需要被缓存的资源文件的文件名称。如下示例：

    CACHE MANIFEST
    # 文件开头必须写 CACHE MANIFEST
    # 这个manifest文件的版本号
    #version 5
    CACHE:
    hello.html
    hello.js
    images/logo.png
    NETWORK:
    ...
    *
    FALLBACK:
    online.js local.js
    CACHE:
    newhello.html
    newhello.js
    
1. 第一行必须是`CACHE MANIFEST`；
2. 服务器需要配置，让服务器支持`text/cache-manifest`这个MIME类型；
3. 注释行以“#”文字开头；
4. 指定资源文件，文件路径可以是相对路径也可以是绝对路径，可以把资源文件分为三类，分别是`CACHE`、`NETWORK`、`FALBACK`：


    CACHE 指定需要被缓存在本地的资源文件。
    NETWORK 指定不进行本地缓存的资源文件，这些资源文件只有当客户端与服务器端建立连接的时候才能访问。（*为通配符）
    FALLBACK 每行指定两个资源文件，第一个资源文件为能够在线访问时使用的资源文件，第二个资源文件为不能在线访问时使用的备用资源文件。
    
在web应用程序页面中`html`标签的`manifest`属性中指定`manifest`文件的`URL地址`：

    <!--你可以为每个页面单独指定manifest文件，也可以为整个web应用程序指定一个总的manifest文件-->
    <html manifest="hello.manifest">
