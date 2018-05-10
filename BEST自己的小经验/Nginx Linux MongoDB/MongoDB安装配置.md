> 目录
> 
> 1.MongoDB 安装
> >
> > 1.1 下载安装 MongoDB
> >
> > 1.2 运行 MongoDB 服务器
> >
> > 1.3 连接 MongoDB
> >
> > 1.4 配置 MongoDB 服务
>
> 2.MongoDB 可视化工具

### 1.MongoDB 安装
#### 1.1 下载安装 MongoDB
首先在官网下载对应系统的安装包，然后根据.msi文件提示安装，这时候有两个选择：`complete`(自动安装在C盘) 和 `custom`(Browse选择安装的盘)。

    示例：选择安装在E:\mongodb

创建一个数据目录 `data文件` 用来放数据。

    示例：在 E:\mongodb 下新建 data
    
#### 1.2 运行 MongoDB 服务器
使用命令窗口在 `MongoDB` 目录的 `bin` 目录中执行 `mongod.exe` 文件.

    示例：E:\mongodb\bin\mongod --dpath E:\mongodb\data\db
    
#### 1.3 连接 MongoDB
重新打开一个命令窗口中运行 `mongo.exe` 命令即可连接上 `MongoDB`，执行如下命令：

    E:\mongodb\bin\mongo.exe

在浏览器输入 `http://localhost:27017`，如果出现一排英文则表示连接成功。

#### 1.4 配置 MongoDB 服务
每次启动 MongoDB 比较麻烦，可以通过命令行 `net start MongoDB` 启动，该配置会大大方便。

1. 先在 `data文件` 下创建一个新文件夹 `log文件夹`（用来存放日志文件）
2. 在 `mongodb文件夹` 下新建配置文件 `mongo.config`

    先创建一个 `mongo.config` 文件，再用记事本打开，点击”另存为“，将底下的文件类型更改为”全部类型“，并更改文件名称为 `mongo.config`。
    
3. 用记事本打开 `mongo.config` ，并输入：


    dbpath=E:\mongodb\data\db
    logpath=E:\mongodb\data\log\mongo.log
    
4. 用==管理员身份==打开cmd。

    1. 在开始菜单中输入 cmd，会出现 cmd 程序
    2. 点击右键，选择以管理员身份运行，打开后可以发现在顶端比普通打开的多了”管理员“三个字
    
5. 配置 `windows` 服务：

    1. cmd 先跳转到 E:\mongodb\bin 目录下
    2. 根据刚创建的 `mongo.config` 配置文件安装服务，名称为 `MongoDB`，输入如下命令：
    
    
        mongod --config E:\mongodb\mongo.config --install --serviceName "MongoDB"
        

6. 到这里基本上就配置成功啦，使用 `net start MongoDB` 启动 `MongoDB`，使用 `net stop MongoDB` 关闭 `MongoDB`。启动服务后可以通过浏览器中输入 `http://127.0.0.1:27017` 检测是否安装服务成功。

### 2.MongoDB 可视化工具
MongoDB 有很多可视化工具，这里我使用的是 `adminmongo`。

1. 新建 `mongoView文件夹`，远程仓库克隆 `adminmongo`：

    https://github.com/mrvautin/adminMongo
    
2. 克隆后，进入 `adminmongo文件夹`，安装相关依赖：

    npm/cnpm install
    
3. 安装依赖后，启动可视化工具：

    npm start
    
4. 浏览器输入地址：`http://127.0.0.1:1234`即可访问。

5. 第一次访问需要和本地 `mongodb` 连接起来，需要输入 `Connection name`（输入 `mongodb`） 和 `Connection string`(输入本地 `mongodb` 的 ip地址 ：`mongodb://127.0.0.1:27017`)，然后点击黑色的 `Connect` 即可。