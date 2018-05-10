使用 `vue-cli` 脚手架构建项目
---

    node>=6.0.0 ;
    npm>=3.0.0 ;
    全局安装 vue-cli：npm install -g vue-cli
    安装 webpack : cnpm install webpack -g

在项目所在文件夹初始化项目，执行命令：`vue init webpack demo`
    
    name: 项目名，可默认
    description: 项目描述，可默认
    author: 项目作者，可默认
    build: 项目输出构建，可默认
    router: vue路由，默认安装
    ESLint: 语法检测器，最好选择NO
    unit tests: 项目测试，最好选择NO
    e2e tests: 项目测试，最好选择NO
    after the project has been created run npm install:  
    是否创建项目后执行 npm install，选择否就安装结束后手动执行，安装package.json里的各个依赖包。
    // 最后一步如果是在国内建议选择自行安装，在安装好的文件夹内项目根目录处使用淘宝镜像cnpm，通过cnpm install下载会快一些
    
项目安装后项目结构：

![image](https://files.jb51.net/file_images/article/201707/2017071816240939.jpg)

    ├── build              // 项目构建(webpack)相关代码  
        │ ├── build.js       // 生产环境构建代码
        │ ├── check-versions.js // 检查node&npm等版本
        │ ├── dev-client.js       // 热加载相关(实现页面自动刷新)
        │ ├── dev-server.js        // 构建本地服务器(npm run dev 即运行它)
        │ ├── utils.js          // 构建配置公用工具
        │ ├── vue-loader.conf.js // vue加载器
        │ ├── webpack.base.conf.js // webpack基础环境配置
        │ ├── webpack.dev.conf.js //  webpack开发环境配置
        │ └── webpack.prod.conf.js // webpack生产环境配置
    
    ├── config       // 项目开发环境配置相关代码
        │ ├── dev.env.js  // 开发环境变量配置
        │ ├── index.js  // 项目主要配置变量（包括监听端口，打包路径等）
        │ └── prod.env.js // 生产环境变量

    ├──node_modules// 项目依赖的模块
    
    ├── src     // 源码目录
        │ ├──  assets// 资源目录(样式类文件，如css,less,sass，以及一些外部js文件)
            │ └── logo.png
        │ ├── components// vue公共组件
            │ └── Hello.vue
        │ ├──router// 前端路由
            │ └── index.js// 路由配置文件
        │ ├── App.vue// 页面入口文件（根组件）
        │ └── main.js// 程序入口文件（入口js文件）
    
    └── static   // 静态文件，比如一些图片，json数据等
        │ ├── .gitkeep
        
    ├── .babelrc// ES6语法编译配置
    ├── .editorconfig// 该文件定义项目的编码规范，编译器的行为会与.editorconfig文件中定义的一致，在多人合作开发项目时十分有用
    ├── .gitignore// git上传需要忽略的文件格式
    ├── .postcssrc.js// 转换css的工具
    ├── index.html// 入口页面
    ├── package.json// 项目基本信息
    ├── README.md// 项目说明
    
本地服务器预览项目，执行命令: `npm run dev`

生成项目输出文件，执行命令: `npm run build`，可以看到在你的项目里生成了一个dist文件夹，里面就是你的项目输出文件。



