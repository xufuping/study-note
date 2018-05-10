#### 1. 在 Linux 安装启动 Nginx
安装：`yum install nginx -y`

启动： `nginx`，这时访问 `http://你的域名`可以访问到 Nginx 的测试页面。可以使用 `nginx -s reload` 命令重启 Nginx。

#### 2. 配置 nginx.conf，并上传相关文件
打开 Nginx 的默认配置文件 `/etc/nginx/nginx.conf `，将 `server` 下的 `root  /usr/share/nginx/html;` 修改为 `root /data/www;`。

然后我们在data下新建www目录：`mkdir -p /data/www`

将我们的`.html`等本地资源上传到 `/data/www` 目录，重启Nginx：`nginx -s reload`。

#### 3. 修改ip地址访问首页
假如你的首页是`home.html`，这时候如果你要访问你的页面必须是`ip/home.html`，那么怎样直接访问`ip`就可以直接进入`home.html`呢？

修改`nginx.conf`里的`server`下的`location`即可：

    location / {
        index home.html;
    }

重启Nginx：`nginx -s reload`，访问你的页面直接输入`ip`地址即可。