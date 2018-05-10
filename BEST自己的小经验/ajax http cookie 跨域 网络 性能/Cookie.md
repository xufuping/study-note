大小：4KB

请求：随HTTP事务一起发送（所以会浪费一部分带宽）

加密：后台使用MD5或者DES加密

cookie属性 | 含义
---|---
name | 一个cookie的名称
value | 一个cookie的值
domain | 可以访问此cookie的域名
path | 可以访问此cookie的页面路径
Size | 此cookie大小
http | cookie的httponly属性。若此属性为true，则只有在http请求头中会带有此cookie的信息，而不能通过document.cookie来访问此cookie
secure | 设置是否只能通过https来传递此条cookie

三步创建：设置->获取->删除

    var cookie = {
        set:function(key,val,time){//设置cookie方法
            var date=new Date(); //获取当前时间
            var expiresDays=time;  //将date设置为n天以后的时间
            date.setTime(date.getTime()+expiresDays*24*3600*1000); //格式化为cookie识别的时间
            document.cookie=key + "=" + val +";expires="+date.toGMTString();  //设置cookie
        },
        get:function(key){ //获取cookie方法
            /*获取cookie参数*/
            var getCookie = document.cookie.replace(/[ ]/g,"");  //获取cookie，并且将获得的cookie格式化，去掉空格字符
            var arrCookie = getCookie.split(";")  //将获得的cookie以"分号"为标识 将cookie保存到arrCookie的数组中
            var tips;  //声明变量tips
            for(var i=0;i<arrCookie.length;i++){   //使用for循环查找cookie中的tips变量
                var arr=arrCookie[i].split("=");   //将单条cookie用"等号"为标识，将单条cookie保存为arr数组
                if(key==arr[0]){  //匹配变量名称，其中arr[0]是指的cookie名称，如果该条变量为tips则执行判断语句中的赋值操作
                    tips=arr[1];   //将cookie的值赋给变量tips
                    break;   //终止for循环遍历
                }
            }
            return tips;
        },
        delete:function(key){ //删除cookie方法
            var date = new Date(); //获取当前时间
            date.setTime(date.getTime()-10000); //将date设置为过去的时间
            document.cookie = key + "=v; expires =" +date.toGMTString();//设置cookie
        }
    }

使用：

    cookie.set("uesrname","xqf",24); //设置为24天过期
    cookie.get("uesrname");//获取cookie

附：https://www.cnblogs.com/zqifa/p/js-cookie-1.html