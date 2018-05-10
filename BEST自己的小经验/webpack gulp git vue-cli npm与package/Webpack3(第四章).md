> 目录
> 
> 附
>
> > 关于webpack模块化
> >
> > 优雅引用第三方类库
> >
> > 抽离出第三方库
> >
> > 版权或者开发者声明
> >
> > 额外的静态资源
> >
> > 模块热替换

附：
---
#### 关于webpack模块化
在实际工作中我们使用`webpack`比较喜欢使用ES6的`模块化`开发，这里举个例子：

我们可以在`src`下建立一个`webpack`文件夹，在其中建立`entry_webpack.js`，将`webpack.config.js`中的`entry`部分单独写成一个模块。

    entry_webpack.js
    
    //声明entry变量
    const entry ={};  
    //声明路径属性
    entry.path={
        entry:'./src/entry.js'  
    }
    //进行模块化
    module.exports =entry;

在`webpack.config.js`中引入:

    // 引入
    const entry = require("./webpack_config/entry_webpack.js")

    // 设置entry
    entry:entry.path,
    
然后就可以进行打包了，其他模块也是如此。

#### 优雅引用第三方类库
我们在`webpack`打包时第三方库怎么办？接下来用例子给大家展示：

我们以`Jquery`为例:

    // 注意，我们要把它下载在开发环境，而不是生产环境
    cnpm install --save jquery
    
我们可以使用`import`和`ProvidePlugin`两种方法引入第三方库，但是`import`不管你是否在代码中使用第三方库都会打包，会让代码冗余；`ProvidePlugin`则是全局引入，按需打包，推荐使用，这里也只讲`ProvidePlugin`使用方法：

`ProvidePlugin`是`webpack`自带的插件，所以要先再`webpack.config.js`中引入`webpack`:

    const webpack = require('webpack');
    
引入成功后我们配置`plugins`模块:

    plugins:[
        new webpack.ProvidePlugin({
            $:"jquery",
            jQuery: 'jquery'
        })
    ],

就可以进行打包了。

#### 抽离出第三方库
在工作中，我们需要将第三方库抽离出来，以免和我们的js代码混杂在一起不利于后期维护。我们再次以例子来展示抽离第三方类库：

我们先修改`webpack.config.js`的`entry`:

    entry:{
        entry:'./src/entry.js',
        jquery:'jquery'
    },

引入`optimize优化插件`(引入`webpack`即可)并在`plugins`里进行配置:

    new webpack.optimize.CommonsChunkPlugin({
        // name对应入口文件中的名字，我们命名为jQuery
        name:'jquery',
        // 一个把文件打包到哪里的路径
        filename:"assets/js/jquery.min.js",
        // 最小打包的文件模块数，minChunks一般都是固定配置，但是不写会打包失败，这里直接写2就好。
        minChunks:2
    }),

然后我们可以进行打包了，并抽离出`jquery`，但是多个第三方类库抽离怎么办呢？以引入`jq`和`vue`为例:

配置`webpack.config.js`文件中的`entry`：

    entry:{
        entry:'./src/entry.js',
        jquery:'jquery',
        vue:'vue'
    },

修改`plugins`中的`CommonsChunkPlugin`配置：

    new webpack.optimize.CommonsChunkPlugin({
        // name对应入口文件中的名字
        name:['jquery','vue'],
        // 一个把文件打包到哪里的路径
        filename:"assets/js/[name].js",
        // 最小打包的文件模块数，minChunks一般都是固定配置，但是不写会打包失败，这里直接写2就好。
        minChunks:2
    }),

配置好后我们就可以进行打包了。

#### 版权或者开发者声明
在工作时每个人写的代码都要写上备注，为的就是在发生问题时可以找到当时写代码的人。有时候也用于版权声明。我们可以使用`BannerPlugin`这个插件。

    // 引入webck即可
    const webpack = require('webpack');
    
    // plugins配置
    new webpack.BannerPlugin('徐清风笔记版权所有，转载商用请联系本人，未经许可盗用必究')

#### 额外的静态资源
在工作中项目组长会要求你打包时保留一些静态资源(设计图、开发文档等)，直接打包到制定文件夹。使用插件`copy-webpack-plugin`即可：

    // 下载
    cnpm install --save-dev copy-webpack-plugin
    
    // 引入
    const copyWebpackPlugin= require("copy-webpack-plugin");
    
    // 在plugins配置使用
    new copyWebpackPlugin([{
        // from:要打包的静态资源目录地址，这里的__dirname是指项目目录下，是node的一种语法，可以直接定位到本机的项目目录中。
        from:__dirname+'/src/public',
        // to:要打包到的文件夹路径，跟随output配置中的目录。所以不需要再自己加__dirname。
        to:'./public'
    }])

配置好后我们就可以进行打包了。

#### 模块热替换
`模块热替换(HMR - Hot Module Replacement)`功能会在应用程序运行过程中替换、添加或删除模块，而无需重新加载整个页面。在`webpack3`中启用热加载相当的容易，只要加入`HotModuleReplacementPlugin`这个插件就可以了。

    // 引入webck即可
    const webpack = require('webpack');
    
    // plugins配置
    new webpack.HotModuleReplacementPlugin()
    
启动`npm run server`，修改index.html浏览器就可以即时更新某个模块。

![分割线](https://thumbnail0.baidupcs.com/thumbnail/9c0f8419c448613afe5e9ce4e03c42dd?fid=1737706044-250528-418203039170764&time=1517058000&rt=sh&sign=FDTAER-DCb740ccc5511e5e8fedcff06b081203-Qkr71fUc1rIISl3r2mSINMehzCM%3D&expires=8h&chkv=0&chkbd=0&chkpc=&dp-logid=624108863495940664&dp-callid=0&size=c710_u400&quality=100&vuk=-&ft=video)