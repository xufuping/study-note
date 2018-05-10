> 目录
>
> 1. webpack作用简介
>
> 2. webpack安装
>
> 3. webpack配置文件：入口与出口
>
> 4. webpack配置文件：热更新服务

## 1.webpack作用简介

1. 打包：把多个JS打包成一个文件，减少服务器压力和下载宽带 。当然，也可以压缩html/css/图片等资源。
2. 转换：把扩展语言(ES6/ES7/less/sass)转换为普通的js/css，让浏览器顺利运行。
3. 优化：优化css前缀、去除未使用的css和库，优化和提升前端页面的性能。


## 2.webpack安装

前提是安装好了`node/npm`，(默认大家配置好了`npm`的淘宝镜像`cnpm`)。安装之前，我们在文件根目录先使用`npm init`生成`package.json`文件。

然后我们使用`cnpm`安装`webpack`，不推荐全局安装，==推荐局部安装==：

    // 全局安装
    cnpm install -g webpack
    
    // 局部安装
    cnpm install webpack --sava-dev
    
    /*
    如果遇到如下错误
    Please install 'webpack-cli' in addition to webpack itself to use the CLI.
    请安装wepack-cli：
    全局：cnpm i -g webpack-cli
    在项目本地安装：cnpm install webpack-cli -D
    */

安装完毕后，使用`webpack -v`检测你的`webpack`版本。

全局安装在文件根目录的终端直接使用`webpack`就进行打包，局部安装在文件根目录的`package.json`里配置一下:

    "scripts": {
        "start": "webpack"
    }

然后在文件根目录的终端使用`npm start`或者`npm run start`就可以打包你的文件了。虽然可以使用下面命令进行打包:

    npm webpack {entry file} {destination for bundled file}
    // {entry file}:入口文件的路径。
    // {destination for bundled file}:填写打包后存放的路径。

但是最好还是使用配置文件来进行项目打包。

## 3.webpack配置文件：入口与出口

关于配置文件，如果是中型或者大型项目最好采用模块化的配置文件【我们将在webpack(附)中提到】，这里我们在根目录创建一个 `webpack.config.js` 作为配置文件，一般配置文件中包含以下项:

    module.exports={
        
        // 入口文件的配置项
        entry:{},
        
        // 出口文件的配置项
        output:{},
        
        // 模块, 比如解读打包js/CSS, 转换压缩图片
        module:{},
        
        //插件
        plugins:[],
        
        // 开发调试工具
        devtool:'',
        
        //配置webpack开发服务
        devServer:{}
    }

### 一个例子(后续也会用到此例)：
我们先局部安装`webpack`，并且建好相应文件夹和文件(记得在`package.json`配置好相应 `npm` 运行命令)，目录如下：

    - webpack
        + dist // 文件输出目录
        + node_modules // node包依赖
        - src // 源码文件目录
            entry.js // webpack的入口文件
        package.json
        webpack.config.js // webpack配置文件

webpack.config.js 内容如下：

    // 头部引入path路径包
    const path = require('path');
    
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
            // __dirname是指项目目录下，是node的一种语法，可以直接定位到本机的项目目录中
            path: path.resolve(__dirname, 'dist'),
            // 打包的文件输出名称
            filename: 'output.js'
        },
        
        // 模块, 比如解读打包js/CSS, 转换压缩图片
        module:{},
        
        //插件
        plugins:[],
        
        // 开发调试工具
        devtool:'',
        
        //配置webpack开发服务
        devServer:{}
    }
    
我们在`entry.js`里随意写点js代码，然后在根目录使用终端执行 `npm run start`（`start`是在`package.json`自定义的启用`webpack`的命令，在前面我们也有提到过）。
 
关于多js文件打包为多个相对应js文件，修改一下`entry`和`output`即可。

    entry:{
        entry: './src/entry.js', 
        entry2: './src/entry2.js'
    },
    
    output:{
        // 打包的路径输出位置
        path: path.resolve(__dirname, 'dist'),
        // 打包的文件输出名称——相对应js文件名称
        filename: '[name].js'
    },
    
## 4.webpack配置文件：热更新服务

关于热更新服务，该功能的目的是使用`webpack`打包后，启动服务看打包后的文件的效果。下载：

    cnpm install webpack-dev-server --save-dev
    
因为这里也是局部安装，所以我们也要在`package.json`里配置 `scripts`:

    // --open是在启动服务后自动打开浏览器，也可以不需要，然后手动输入地址和端口
    "server": "webpack-dev-server --open",

然后我们主要在`webpack.config.js` 里的 `devServer` 里配置:

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

关于热更新：因为`webpack`存在`watch`监控机制，所以我们一旦修改源码并保存，浏览器便会自动为我们更新。 ==但是，webpack并不能根据我们的修改而自动打包！！== 如果需要更新后自动打包，可以使用`watch`：

    // 启用观察
    watch: true, // boolean

    watchOptions: {
        
        // 检测修改的时间，以毫秒为单位
        poll:1000,
    
        // 防止重复保存而发生重复编译错误。这里设置的500是半秒内重复保存，不进行打包操作
        aggregateTimeout: 1000,
    
        // 不监听的目录(监听大量文件系统会导致大量的 CPU 或内存占用)
        ignored:/node_modules/,
    }
    
![分割线](https://thumbnail0.baidupcs.com/thumbnail/9c0f8419c448613afe5e9ce4e03c42dd?fid=1737706044-250528-418203039170764&time=1517058000&rt=sh&sign=FDTAER-DCb740ccc5511e5e8fedcff06b081203-Qkr71fUc1rIISl3r2mSINMehzCM%3D&expires=8h&chkv=0&chkbd=0&chkpc=&dp-logid=624108863495940664&dp-callid=0&size=c710_u400&quality=100&vuk=-&ft=video)