> 目录
>
> 5. 模块与插件配置
>
> > 5.1 发布HTML
> >
> > 5.2  CSS打包
> >
> > > 5.2.1  分离CSS
> > >
> > > 5.2.2 压缩(丑化)css代码
> > >
> > > 5.2.3 自动处理CSS3属性前缀
> > >
> > > 5.2.4 消除未使用的CSS
> > >
> > > 5.2.5 less打包

## 5. 模块与插件配置
什么是`Loaders`? `loader` 用于对模块的源代码进行转换。是`webpack`最重要的功能，它有如下配置项：

    test: 用于匹配处理文件的扩展名的正则表达式，必须配置
    
    use: loader名称(就是你要使用的模块的名称)，必须配置
    
    include/exclude: 手动添加的'必须处理的文件(文件夹)/屏蔽不需要处理的文件(文件夹)'，可选配置
    
    query: 为loaders提供额外的设置选项（可选）

### 5.1 发布HTML
我们在src下创建一个`index.html`文件：

    - webpack
        + dist // 文件输出目录
        + node_modules // node包依赖
        - src // 源码文件目录
            entry.js // webpack的入口文件
            index.html 
        package.json
        webpack.config.js // webpack配置文件

`index.html`:

    <!doctype html>
    <html lang="en">
    <head>
    	<meta charset="UTF-8">
    	<!-- 手机页面中加入这句话，可以让页面适应设备的宽度。initial-scale - 初始的缩放比例 -->
    	<meta name="viewport" content="width=device-width, initial-scale=1.0">
    	<!-- Edge 模式通知 Windows Internet Explorer 以最高级别的可用模式显示内容 -->
    	<meta http-equiv="X-UA-Compatible" content="IE=Edge">
    	<title>XQF webpack test</title>
    </head>
    <body>
    	<div id="gogo"></div>
    	<div id="title"></div>
    	<p>我是小段落123</p>
    </body>
    </html>
    
▲ 这份`index.html`文件没有js引入，`webpack`会为我们自动引入`js`。它还能够通过入口js文件帮我们自动引入`css`文件。

我们先安装`html-webpack-plugin`，它简化了HTML文件的创建，以便为你的webpack包提供服务：

    cnpm install --save-dev html-webpack-plugin

然后在`webpack.config.js`中引入插件：

    // 头部require
    const htmlPlugin = require('html-webpack-plugin');
    
    // 在plugins中插入
    new htmlPlugin({
        // minify：是对html文件进行压缩，removeAttrubuteQuotes是却掉属性的双引号。
        minify:{
            removeAttributeQuotes:true
        },
        // hash：为了开发中js有缓存效果，所以加入hash，这样可以有效避免缓存JS。
        hash:true,
        // template：是要打包的html模版路径和文件名称。
        template:'./src/index.html'
    })

### 5.2 CSS打包
在`src`创建文件夹`css`，在`css`里创建`index.css`:

    - src
        - css
            index.css

`index.css`:

    body {
        background-color: #D5B740;
        color: black;
    }
    #gogo {
    	width: 466px;
    	height: 453px;
    	transform: rotate(45deg);
    	box-shadow: 1px 1px 0 rgba(0,0,0,.25);
    }
    #meiyong {
    	width: 100%;
    }
    #meiyong2 {
    	width: 50%;
    }

css打包需要`style-loader`、`css-loader`两个`loader`。 ==一般这两个`loader`结合使用，顺序是`style-loader`在前== ：

    // 使用npm下载两个`loader`:
    cnpm install --save-dev style-loader css-loader

`style-loader`: 让js解析css。

`css-loader`: `css-loader` 解释(`interpret`) `@import` 和 `url()` ，会 `import/require()` 后再解析`(resolve)`它们。

在入口文件`entry.js`里引入`css`：

    import css from './css/index.css';

在`webpack.config.js`的配置:

    module: {
        rules: [
            {
                test: /\.css$/,
                use: [ 'style-loader', 'css-loader' ]
                // use可以换作loader
                // loader: [ 'style-loader', 'css-loader' ]
            }
        ]
    },

执行`npm start`打包一下，我们发现打包成功了。我们可以执行`npm run server`在服务端查看一下。

#### 5.2.1 分离CSS
    
但是我们发现`css`是打包在`js`里的，因为`webpack`官方认为CSS就应该打包到`JavasScript`当中以减少`http`的请求数，但现实需求有时候需要分离`css`。那么我们需要一个插件`extract-text-webpack-plugin`：

    cnpm install --save-dev extract-text-webpack-plugin
    
安装完成后在`webpack.config.js`中引用

    // 顶部require引入
    const extractTextPlugin = require("extract-text-webpack-plugin");
    
    // 在module中进行配置
    {
        test: /\.css$/,
        use: extractTextPlugin.extract({
	        fallback: "style-loader",
			use: 'css-loader'
		})				
    },
    
    // 在plugins中进行配置
    plugins:[
        new extractTextPlugin("./css/index.css")
    ],
    
#### 5.2.2 压缩(丑化)css代码

设置 `minimize:true` ,就可以压缩`css`了。

    module: {
        rules: [
            {
                test: /\.css$/,
                use: extractTextPlugin.extract({
                    fallback: "style-loader",
                    use: [
                        { loader: 'css-loader', options: { minimize: true } }
                    ]
                })
            }
        ]
    },

#### 5.2.3 自动处理CSS3属性前缀

下载 `postcss-loader` 和`autoprefixer`（自动添加前缀的插件）。

    cnpm install --save-dev postcss-loader autoprefixer

在`src`下建立`postcss.config.js`:

    - webpack
        + dist // 文件输出目录
        + node_modules // node包依赖
        - src // 源码文件目录
            - css
                index.css
            entry.js // webpack的入口文件
            index.html 
        package.json
        postcss.config.js // postcss配置文件
        webpack.config.js // webpack配置文件

`postcss.config.js`:

    module.exports = {
        plugins: [
            require('autoprefixer')
        ]
    }
    
没有配置外部css导出，在`module`这样配置`postcss`:

    {
          test: /\.css$/,
          use: [
                {
                  loader: "style-loader"
                }, {
                  loader: "css-loader",
                  options: {
                     modules: true
                  }
                }, {
                  loader: "postcss-loader"
                }
          ]
    }

配置了`extractTextPlugin`，在`module`这样配置`postcss`:

    {
        test: /\.css$/,
        use: extractTextPlugin.extract({
            fallback: 'style-loader',
            use: [
                { loader: 'css-loader', options: { importLoaders: 1 } },
                'postcss-loader'
            ]
        })
        
    }
    
执行打包处理，自动加上css前缀。

#### 5.2.4 消除未使用的CSS
对于框架(比如`bootstrap`)或者自己修改后没有使用的css，可以使用`PurifyCSS-webpack`：

    // -D代表的是–save-dev ,只是一个简写
    //  需要安装purifyCSS-webpack、purify-css这两个包
    cnpm i -D purifycss-webpack purify-css
    
因为我们需要同步检查html模板，所以我们需要引入node的glob对象使用。同时，引入purifycss-webpack。

    const glob = require('glob');
    const PurifyCSSPlugin = require("purifycss-webpack");

我们在`plugins`中配置:

    // PurifyCSSPlugin 要在 extractTextPlugin 之前，不然 extractTextPlugin 里的压缩和自动前缀可能被影响
    new PurifyCSSPlugin({
        // 配置了一个paths，主要是需找html模板，purifycss根据这个配置会遍历你的文件，查找哪些css被使用了
        paths: glob.sync(path.join(__dirname, 'src/*.html')),
    }),

#### 5.2.5 less打包
确定你的生产环境中配了`less`服务：

    cnpm install --save-dev less
    
然后安装`less-loader`:

    cnpm install --save-dev less-loader
    
在`src/css`下创建 `myless.less`：

    @base :#000;
    #gogo{
        width:300px;
        height:300px;
        background-color:@base;
    }
    
配置了`extractTextPlugin`，在`module`里这样配置`less-loader`:

    {
        test: /\.less$/,
        use: extractTextPlugin.extract({
            use: [{
                loader: "css-loader",
                options: { minimize: true }
            }, {
                loader: "less-loader"
            },{
                loader: 'postcss-loader'
            }],
            fallback: "style-loader"
        })
    }
    
在`entry.js`中配置：

    import less from './css/black.less';
    
【sass亦同 : 需要在项目目录下用npm安装两个包。node-sass和sass-loader】
    
![分割线](https://thumbnail0.baidupcs.com/thumbnail/9c0f8419c448613afe5e9ce4e03c42dd?fid=1737706044-250528-418203039170764&time=1517058000&rt=sh&sign=FDTAER-DCb740ccc5511e5e8fedcff06b081203-Qkr71fUc1rIISl3r2mSINMehzCM%3D&expires=8h&chkv=0&chkbd=0&chkpc=&dp-logid=624108863495940664&dp-callid=0&size=c710_u400&quality=100&vuk=-&ft=video)

当前的`webpack.config.js`

    const path = require('path');
    const htmlPlugin= require('html-webpack-plugin');
    const extractTextPlugin = require("extract-text-webpack-plugin");
    const glob = require('glob');
    const PurifyCSSPlugin = require("purifycss-webpack");
    
    module.exports={
        
        // 入口文件的配置项
        entry:{
            // 这里的entry可以随意命名，使用相对路径引入相应js
            entry: './src/entry.js'
            // 如果需要引入多个js
            // entry: ['./src/entry.js', './src/myjs.js']
        },
        
        // 出口文件的配置项
        output:{
            // 打包的路径输出位置，resolve(解析),这里指定一个本机绝对路径
            path: path.resolve(__dirname, 'dist'),
            // 打包的文件输出名称
            filename: 'output.js'
        },
        
        // 模块, 比如解读打包js/CSS, 转换压缩图片
        module: {
            rules: [
                {
                    test: /\.css$/,
                    use: extractTextPlugin.extract({
                        fallback: "style-loader",
                        use: [
                            { loader: 'css-loader', options: { minimize: true } },
                            'postcss-loader'
                        ]
                    })
                },{
                    test: /\.less$/,
                    use: extractTextPlugin.extract({
                        use: [{
                            loader: "css-loader",
                            options: { minimize: true }
                        }, {
                            loader: "less-loader"
                        },{
                            loader: 'postcss-loader'
                        }],
                        fallback: "style-loader"
                    })
                }
            ]
        },
        
        //插件
        plugins:[
            new htmlPlugin({
                // minify：是对html文件进行压缩，removeAttrubuteQuotes是却掉属性的双引号。
                minify:{
                    removeAttributeQuotes:true
                },
                // hash：为了开发中js有缓存效果，所以加入hash，这样可以有效避免缓存JS。
                hash:true,
                // template：是要打包的html模版路径和文件名称。
                template:'./src/index.html'
            }),
    
            // PurifyCSSPlugin 要在 extractTextPlugin 之前，不然 extractTextPlugin 里的压缩和自动前缀可能被影响
            new PurifyCSSPlugin({
                // 配置了一个paths，主要是需找html模板，purifycss根据这个配置会遍历你的文件，查找哪些css被使用了
                paths: glob.sync(path.join(__dirname, 'src/*.html')),
            }),
    
            new extractTextPlugin("/css/index.css"),
        ],
        
        // 开发调试工具
        devtool:'',
        
        //配置webpack开发服务
        devServer:{
            // 设置关联的基本运行路径，这里因为例子是dist，所以我们写的dist
            contentBase:path.resolve(__dirname,'dist'),
            // 服务器的IP地址，可以使用IP也可以使用localhost。可以使用ipconfig获取本机ipv4地址在这里使用。
            host:'localhost',
            // 服务端压缩是否开启，一般为开启
            compress:true,
            //配置服务端口号，这里使用1010
            port:1010
        }
    }
    
当前的`entry.js`:

    import css from './css/index.css';
    import less from './css/myless.less';
    
    console.log("123");
    var xuqingfeng = "asd2333";

![分割线](https://thumbnail0.baidupcs.com/thumbnail/9c0f8419c448613afe5e9ce4e03c42dd?fid=1737706044-250528-418203039170764&time=1517058000&rt=sh&sign=FDTAER-DCb740ccc5511e5e8fedcff06b081203-Qkr71fUc1rIISl3r2mSINMehzCM%3D&expires=8h&chkv=0&chkbd=0&chkpc=&dp-logid=624108863495940664&dp-callid=0&size=c710_u400&quality=100&vuk=-&ft=video)