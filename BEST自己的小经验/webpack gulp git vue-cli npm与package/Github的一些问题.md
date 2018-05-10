目录
---
> 1.如何初次上传代码？
> 
> 2.如何在修改项目后再次上传代码？
> 
> 3.提交代码遇到`Everything up-to-date`？
> 
> 4.`git commit -a` 或者 `git commit -m` "日志" 弹出VIM编辑器？
> 
> 5.加入`.gitignore`的文件夹但在远程仓库之前就存在该如何删除?
>
> 6.清屏

### 1.如何初次上传代码？
确定你已经下载`git`配置好相关密钥并在远程仓库已经创建了`your-repository`：

    git init // 初始化
    git add . // 可以跟单一文件，可以跟通配符，可以跟目录，点表示全部
    git commit -m "日志" // 把文件提交到仓库
    git remote add origin "url-git" // 关联远程仓库
    git push -u origin master  // 把本地库的所有内容推送到远程库上

### 2.如何在修改项目后再次上传代码？

    git add .
    git commit -a // 如果这一步出现vim编辑器的情况，请看问题4
    git push origin master

### 3.提交代码遇到`Everything up-to-date`？
1.创建并转移到新的分支

    git checkout newbranch // 创建并切换到新分支newbranch
    或者分两步
    git branch newbranch // 创建新分支newbranch
    git checkout newbranch // 切换到新分支newbranch
    // 可以使用 git branch 查看创建的分支

2.提交

    git add .
    git commit -a // 如果这一步出现vim编辑器的情况，请看问题4
    // 可以使用 git status 查看提交情况
    
3.切换到主分支并合并

    git checkout master // 切换
    git merge newbranch // 合并到主分支
    // 可以使用 git diff 查看产生冲突的文件，然后做对应的修改再提交一次就可以了。
    
4.上传代码，处理分支

    git push -u origin master // 上传代码
    git branch -D newbranch // 删除newbranch这个分支
    git branch -d  newbranch // 保留分支只想删除已合并的部分

### 4.`git commit -a` 或者 `git commit -m` "日志" 弹出VIM编辑器？

如果出现以下情况：

    Please enter the commit message for your changes. Lines starting  
    with '#' will be ignored, and an empty message aborts the commit. 
    On branch master  
    #   new file: change.txt  
    #   new file: change2.txt

你首先需要确定是否使用最新文件（可能会修改甚至删除远程仓库一些没有的文件；一般我们是确定上传最新文件。）好了，现在我们确定要删改这些文件了，我们进行如下操作：

1. 删除每个确认删改文件前面的`#`;
2. 删除后`Esc`退出输入状态，然后`Shift` + `;`，再输入`q!`或`wq!`（不保存改动，`wq!`是保存文件的写入修改）,然后你就可以进行其他合并分支或者上传操作了。

### 5.加入`.gitignore`的文件夹但在远程仓库之前就存在该如何删除?
项目根目录创建`.gitignore`，`.gitignore` 文件夹是设置Git上传时需要忽略的文件以及文件夹：

    .gitignore内容：
    node_modules

我们使用远程仓库删除：

    git rm -r --cached some-directory
    git commit -m 'Remove the now ignored directory "some-directory"'
    git push origin master
    
### 6.清屏
`clear`