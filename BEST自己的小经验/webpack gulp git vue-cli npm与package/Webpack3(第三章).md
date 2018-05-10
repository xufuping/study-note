> 目录
> >
> > 5.3 JS打包
> >
> > > 5.3.1 JS压缩/hash命名
> > > 
> > > 5.3.2 babel转码ES6/ES7/JSX
> >
> > 5.4 图片打包与路径坑
> >
> > > 5.4.1 图片在CSS中
> > >
> > > 5.4.2 图片路径问题(包括分离后的CSS路径问题)
> > >
> > > 5.4.3 关于HTML中的图片
> >
> 6. 打包后的调试

### 5.3 JS打包

#### 5.3.1 JS压缩/hash命名
`uglifyjs-webpack-plugin`压缩插件是`webpack`版本里默认已经集成，不需再次安装。在`webpack.config.js`中引入即可：

    const uglify = require('uglifyjs-webpack-plugin');
    
引入后，在`plugins`配置一下就可以了：

    plugins:[
        // 压缩js
        new uglify()
    ],
    
然后使用`npm run start`打包即可。

**js文件的hash命名**

如果需要给js文件一个`hash`命名，可以在`output`中的`filename`进行配置:

    filename: '[hash].js'
    // filename: '[chunkhash].js'

[关于hash和chunkhash的区别](http://blog.csdn.net/Scarlett_Dream/article/details/78856240)

**清除重复不同hash名的js**

使用`clean-webpack-plugin`，不过也可以通过直接删除`dist`文件夹，然后再次打包的方式来更新，此处请查阅官方文档。

#### 5.3.2 babel转码ES6/ES7/JSX
`Babel`是一个编译`JavaScript`的平台，它可以帮你使用`ES6/ES7/JSX`等js扩展语言。我们需要下载`babel-core`、`babel-loader`，扩展`es6/7/8`，下载`babel-preset-env`，扩展`JSX`(使用`react`的话)下载`babel-preset-react`。

    	
    cnpm install --save-dev babel-core babel-loader babel-preset-react babel-preset-env
    
下载后在项目根目录新建`.babelrc`文件，并把配置写到文件里。

    {
        "presets":["react","env"]
    }
    
在`webpack.config.js`中`module`进行配置：

    {
        test:/\.(jsx|js)$/,
        use:{
            loader:'babel-loader',
        },
        exclude:/node_modules/
    }

**继续前面的例子**

配置完毕，我们可以在`entry.js`添加代码：

    let xushao = "shuai";
    
然后打包试试看，是不是成功了，哈哈。

### 5.4 图片打包与路径坑

#### 5.4.1 图片在CSS中
**例如：**
在`src`下创建`images`文件夹，放入图片`1.png`,编辑`HTML文件`和`css文件`：

    html中添加：
    <div id="tupian"></div>
    
    css中添加：
    #tupian{
       background-image: url(../images/1.png);
       width:466px;
       height:453px;
    }
    
我们需要安装`file-loader`和`url-loader`两个`loader`(其实`url-loader`内置了`file-loader`，但是为了保险起见和解决一些路径问题，所以建议同时单独安装`file-loader`):

    cnpm install --save-dev file-loader url-loader
    
`file-loader`: 解决引用路径的问题,可以解析项目中的url引入（图片和ccss文件等）。

`url-loader`: 将引入的图片编码。为了避免图片较大导致编码消耗性能，可以通过`limit`参数限制，小于`limit`的文件被转化为`DataURL`。

**在`module`中配置url-loader：**

    {
        // test:/\.(png|jpg|gif)/是匹配图片文件后缀名称
        test:/\.(png|jpg|gif)/ ,
        // use：是指定使用的loader和loader的配置参数
        use:[{
                loader:'url-loader',
                options:{
                    // 是把小于8192的文件转换成成Base64的格式
                    limit: 8192,
                    // 打包输出到images文件夹下
                    outputPath:'images/'
                }
            }]
    }

配置完毕后可以开始打包试试了。

#### 5.4.2 图片路径问题(包括分离后的CSS路径问题)
如果你的`css`分离后或者你的图片引入发现路径有问题，可以使用`publicPath`解决，它主要在`webpack`配置文件的`output`选项中处理静态文件路径。

我们可以声明一个对象：

    var website ={
        // 这里的IP和端口，是你本机的ip或者是你devServer配置的IP和端口。
        publicPath:"http://113.250.159.94:1010"
    }
    
然后在output选项中引用这个对象的publicPath属性：

    //出口文件的配置项
    output:{
        //输出的路径，用了Node语法
        path:path.resolve(__dirname,'dist'),
        //输出的文件名称
        filename:'[name].js',
        publicPath:website.publicPath
    },
    
配置后进行打包，相对路径改为了绝对路径，速度稍微也变快了些。

#### 5.4.3 关于HTML中的图片
标签`<img>`引入的图片怎么办呢？你可以使用一个不是很火但是很实用的`loader`——`html-withimg-loader`:

    cnpm install html-withimg-loader --save-dev

再配置一下`module`:

    {
        test: /\.(htm|html)$/i,
        use:[ 'html-withimg-loader'] 
    }

然后就可以开始打包了。

## 6. 打包后的调试
我们可以通过配置`devtool`进行开发调试，但要记得上线前修改这些调试。

    devtool: 'eval-source-map',

常用四种选项：

`source-map`:在一个单独文件中产生一个完整且功能完全的文件。打包速度比较慢。
 
`cheap-module-source-map`:在一个单独的文件中产生一个不带列映射的map，不带列映射提高了打包速度，但是也使得浏览器开发者工具只能对应到具体的行，不能对应到具体的列（符号）,会对调试造成不便。

 `eval-source-map`:使用`eval`打包源文件模块，在同一个文件中生产干净的完整版的`sourcemap`，但是对打包后输出的JS文件的执行具有性能和安全的隐患。在开发阶段这是一个非常好的选项，在生产阶段则一定要不开启这个选项。
 
 `cheap-module-eval-source-map`:这是在打包文件时最快的生产`source map`的方法，生产的 `Source map` 会和打包后的`JavaScript`文件同行显示，没有影射列，和`eval-source-map`选项具有相似的缺点。
 
【建议：大型项目可以使用source-map；中小型项目使用eval-source-map就完全可以应对。这些调试只适用于开发阶段，上线前记得修改这些调试设置】

![分割线](https://thumbnail0.baidupcs.com/thumbnail/9c0f8419c448613afe5e9ce4e03c42dd?fid=1737706044-250528-418203039170764&time=1517058000&rt=sh&sign=FDTAER-DCb740ccc5511e5e8fedcff06b081203-Qkr71fUc1rIISl3r2mSINMehzCM%3D&expires=8h&chkv=0&chkbd=0&chkpc=&dp-logid=624108863495940664&dp-callid=0&size=c710_u400&quality=100&vuk=-&ft=video)