> 目录
>
> 1.XSS（跨站脚本攻击）
>
> 2.CSRF（跨站请求伪造）

#### 1.XSS（跨站脚本攻击）
`XSS`是常见的Web攻击技术之一.所谓的跨站脚本攻击指得是:恶意攻击者往Web页面里注入恶意`Script`代码，用户浏览这些网页时，就会执行其中的恶意代码，可对用户进行盗取cookie信息、会话劫持等各种攻击.

解决方案:

1. **输入过滤**。永远不要相信用户的输入，对用户输入的数据做一定的过滤。如输入的数据是否符合预期的格式，比如日期格式，Email格式，电话号码格式等等。这样可以初步对XSS漏洞进行防御。上面的措施只在web端做了限制，攻击者通抓包工具如Fiddler还是可以绕过前端输入的限制，修改请求注入攻击脚本。因此，后台服务器需要在接收到用户输入的数据后，对特殊危险字符进行过滤或者转义处理，然后再存储到数据库中。

2. **输出编码**。服务器端输出到浏览器的数据，可以使用系统的安全函数来进行编码或转义来防范XSS攻击。

    1. 过滤和编码，使用正则表达式
    
        ![XSS过滤字符](http://mmbiz.qpic.cn/mmbiz_jpg/sGfPWsuKAfdPNtzwZZ3NwQFsQCI1VXGzYxxIUS7UqicxgrRPhhb9ib0wMJEhXFwaE47oY62RB9Be28aKe1kCF4gQ/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1)
   
    
    var HtmlUtil = {
        /*1.用正则表达式实现html转码*/
        htmlEncodeByRegExp:function (str){  
            var s = "";
            if(str.length == 0) return "";
            s = str.replace(/&/g,"&amp;");
            s = s.replace(/</g,"&lt;");
            s = s.replace(/>/g,"&gt;");
            s = s.replace(/ /g,"&nbsp;");
            s = s.replace(/\'/g,"&#39;");
            s = s.replace(/\"/g,"&quot;");
            return s;  
        },
        /*2.用正则表达式实现html解码*/
        htmlDecodeByRegExp:function (str){  
            var s = "";
            if(str.length == 0) return "";
            s = str.replace(/&amp;/g,"&");
            s = s.replace(/&lt;/g,"<");
            s = s.replace(/&gt;/g,">");
            s = s.replace(/&nbsp;/g," ");
            s = s.replace(/&#39;/g,"\'");
            s = s.replace(/&quot;/g,"\"");
            return s;  
        }
    };

4. **HttpOnly Cookie**。预防XSS攻击窃取用户cookie最有效的防御手段。Web应用程序在设置cookie时，将其属性设为HttpOnly，就可以避免该网页的cookie被客户端恶意JavaScript窃取，保护用户cookie信息。

5. **WAF(Web Application Firewall)**，Web应用防火墙，主要的功能是防范诸如网页木马、XSS以及CSRF等常见的Web漏洞攻击。由第三方公司开发，在企业环境中深受欢迎。

#### 2.CSRF（跨站请求伪造）
`CSRF攻击`的原理:`CSRF攻击`过程的受害者用户登录网站A，输入个人信息，在本地保存服务器生成的`cookie`。然后在A网站点击由攻击者构建一条恶意链接跳转到B网站,然后B网站携带着的用户`cookie`信息去访问B网站.让A网站造成是用户自己访问的假相,从而来进行一些列的操作,常见的就是转账。

解决方案:

1. **验证码**。应用程序和用户进行交互过程中，特别是账户交易这种核心步骤，强制用户输入验证码，才能完成最终请求。在通常情况下，验证码够很好地遏制。CSRF攻击。但增加验证码降低了用户的体验，网站不能给所有的操作都加上验证码。所以只能将验证码作为一种辅助手段，在关键业务点设置验证码。

2. **Referer Check**。`HTTP Referer` 是`header`的一部分，当浏览器向web服务器发送请求时，一般会带上`Referer`信息告诉服务器是从哪个页面链接过来的，服务器籍此可以获得一些信息用于处
理。可以通过检查请求的来源来防御`CSRF攻击`。正常请求的`referer`具有一定规律，如在提交表单的`referer`必定是在该页面发起的请求。所以通过检查`http包头`的`referer`的值是不是这个页面，来判断是不是`CSRF攻击`。但在某些情况下如从`https`跳转到`http`，浏览器处于安全考虑，不会发送`referer`，服务器就无法进行`check`了。若与该网站同域的其他网站有`XSS`漏洞，那么攻击者可以在其他网站注入恶意脚本，受害者进入了此类同域的网址，也会遭受攻击。出于以上原因，无法完全依赖`Referer Check` 作为防御 `CSRF攻击` 的主要手段。但是可以通过 `Referer Check` 来监控`CSRF攻击`的发生。

3. **添加Token**。目前比较完善的解决方案是加入`Token`，即发送请求时在 `HTTP请求` 中以参数的形式加入一个随机产生的`token`，并在服务器建立一个拦截器来验证这个`token`。服务器读取浏览器当前域`cookie`中这个`token`值，会进行校验该请求当中的`token`
和 `cookie` 当中的 `token` 值是否都存在且相等，才认为这是合法的请求。否则认为这次请求是违法的，拒绝该次服务。

    1. 实现
    
        对于 `GET请求`，`token` 将附在请求地址之后，这样 URL 就变成 `http://url?csrftoken=tokenvalue`。 而对于 `POST请求`来说，要在 `form` 的最后加上 `<input type="hidden" name="csrftoken" value="tokenvalue" />`，这样就把 `token` 以参数的形式加入请求了。
        
        如果一个网站有很多请求，使用 js 遍历整个`DOM树`，对于 `DOM` 中所有的 `a `和 `form` 标签（需要使用`token`的地方）中加入 `token`。但是对于页面加载之后动态生成的`html`代码，就不能遍历添加了，需要手动添加。
        
        该方法有一个缺陷就是是难以保证 `token` 本身的安全。比如在一些论坛之类支持用户自己发表内容的网站，黑客可以在上面发布自己个人网站的地址。由于系统也会在这个地址后面加上 `token`，黑客可以在自己的网站上得到这个 `token`，并马上就可以发动 `CSRF` 攻击。为了避免这一点，系统可以在添加 `token` 的时候增加一个判断，如果这个链接是链到自己本站的，就在后面添加 `token`，如果是通向外网则不加。但是，即使这个 `token` 不以参数的形式附加在请求之中，黑客的网站也同样可以通过 `Referer` 来得到这个 `token`以发动 `CSRF攻击`。
        
    2. 在`HTTP`头中自定义属性并验证
    
        把`token`参数放在`HTTP`头中的自定义属性（比如自定义个 `csrf-token` 这样的`HTTP头属性`）中，通过`XHR`这个类就自动为该类请求都加上了`token`，这样避免了加入`token`不方便，也不用担心 `token` 会透过 `Referer` 泄露到其他网站中去。它的局限性在于页面请求如果不是`XHR`这个类的请求，就没有办法发送验证`token`值。