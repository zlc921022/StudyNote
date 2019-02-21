# Git 使用


配置用户信息

    git config -- global user.name 'name'
    git config -- global user.email 'xxx@163.com'
 
显示config的配置 加--list

    git config --list --local
    git config --list --global
    git config --list --system
初始化 

    git init 

添加

    git add [file path]
    git add . // 添加全部文件
    
提交新建的

    git commit -m “message”
    
提交所有的文件以及修改的 

    git commit -a -m “massage”

添加远程分支

    git remote add [name] [url]
    
查看远程分支信息
    
    git remote
    git remote -v  //详细信息

删除远程分支

    git remote remove [name]
    
推送到远程分支

    git push [remote-name][branch-name] 
    
检查更改前后的区别

    git diff
    git diff HEAD
    git diff HEAD filename // 比较文件


查看当前的分支信息

    git branch -av
     
修改 最新 commit的message
    
    git commit --amend

