> 目录
>
> npm三大安装包命令
>
> 开发依赖和生产依赖的区别
>
> 克隆修改维护别人的项目
>
> npm 终端命令配置

npm三大安装包命令
---

    // 将包安装在全局，全局包要记得及时更新
    cnpm/npm install <package>
    
    // 安装生产依赖包
    cnpm/npm install <package> --save
    
    // 安装开发依赖包
    cnpm/npm install <package> --save-dev


开发依赖和生产依赖的区别
---
`devDependencies`：开发依赖包，开发时使用的包。开发时候使用的包

`dependencies`: 生产依赖包，生产时使用的包，会和你的项目一起上线。(比如你的代码使用了`jquery`，你需要把你的`jquery`也放到线上，你就应该把`jquery`包放在这里)


克隆修改维护别人的项目
---
这时候你需要克隆别人的项目(包括`package.json`)；然后输入`cnpm/npm install`，直接根据`package.json`安装别人的开发依赖包；如果需要安装生产依赖包，你需要输入`cnpm/npm install --production`，安装生产依赖包


npm 终端命令配置
---
如果想要配置终端快捷使用的自定义命令，可以在`package.json`里的`scripts` 以 `key-value`的形式配置。然后使用`npm run 'key'`即可。例如：

    // 配置webpack服务端快速运行，每次运行就可以直接输入 npm run server，就可以执行相应命令了
    "scripts": {
        "server": "webpack-dev-server"
    }


![分割线](https://thumbnail0.baidupcs.com/thumbnail/9c0f8419c448613afe5e9ce4e03c42dd?fid=1737706044-250528-418203039170764&time=1517058000&rt=sh&sign=FDTAER-DCb740ccc5511e5e8fedcff06b081203-Qkr71fUc1rIISl3r2mSINMehzCM%3D&expires=8h&chkv=0&chkbd=0&chkpc=&dp-logid=624108863495940664&dp-callid=0&size=c710_u400&quality=100&vuk=-&ft=video)