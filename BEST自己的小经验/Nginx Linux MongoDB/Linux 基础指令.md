> 目录
>
>

**清屏：** `clear`

#### 1.目录操作

**创建目录：** `mkdir`

    mkdir $HOME/testFolder

**切换目录：** `cd`

    cd $HOME/testFolder // 进入创建的目录
    cd ../ // 返回上级
    
**移动目录：** `mv`

    mv $HOME/testFolder /var/tmp
    
**删除目录：** `rm -rf`

    rm -rf /var/tmp/testFolder
    
**查看目录下的文件：** `ls`

    ls /etc

#### 2.文件操作
**创建文件：** `touch`

    touch ~/testFile
    // 执行 ls 命令, 可以看到刚才新建的 testFile 文件
    ls ~

**复制文件：**`cp`

    cp ~/testFile ~/testNewFile
    
**删除文件：**`rm`

    rm ~/testFile // 删除文件，输入 y 后回车确认
    
**查看文件内容：**`cat`

    // 查看 .bash_history 文件内容
    cat ~/.bash_history
    
#### 3.过滤与重定向
**过滤：**`grep`

    // 过滤出 /etc/passwd 文件中包含 root 的记录
    grep 'root' /etc/passwd
    
    // 递归地过滤出 /var/log/ 目录中包含 linux 的记录
    grep -r 'linux' /var/log/
    
**管道：** 简单来说, `Linux` 中管道的作用是将上一个命令的输出作为下一个命令的输入, 像 `pipe` 一样将各个命令串联起来执行, 管道的操作符是 `|`

    // 比如, 我们可以将 cat 和 grep 两个命令用管道组合在一起
    cat /etc/passwd | grep 'root'
    
    // 过滤出 /etc 目录中名字包含 ssh 的目录(不包括子目录)
    ls /etc | grep 'ssh'
    
**重定向：** 可以使用 `>` 或 `<` 将命令的输出重定向到一个文件中

    echo 'Hello World' > ~/test.txt
    
#### 4.运维常用命令
**ping命令：**

    // 对 cloud.tencent.com 发送 4 个 ping 包, 检查与其是否联通
    ping -c 4 cloud.tencent.com
    
**netstat：** `netstat` 命令用于显示各种网络相关信息，如网络连接, 路由表, 接口状态等等

    // 列出所有处于监听状态的tcp端口
    netstat -lt
    
    // 查看所有的端口信息, 包括 PID 和进程名称
    netstat -tulpn
    
**PS命令：** 过滤得到当前系统中的 `ssh` 进程信息

    ps -aux | grep 'ssh'