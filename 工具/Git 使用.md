# Git 使用


配置用户信息

    git config --global user.name 'name'
    git config --global user.email 'xxx@163.com'
 
显示config的配置 加--list

    git config --list --local
    git config --list --global
    git config --list --system 

从远程仓库 Clone 项目到本地

    git clone [url]

初始化 

    git init 

添加

    git add [file path]
    git add . // 添加全部文件
    
提交新建的

    git commit -m “message”
    
提交所有的文件以及修改的 

    git commit -am “massage”

添加远程分支

    git remote add [name] [url]
    
查看远程分支信息
    
    git remote
    git remote -v  //详细信息

删除远程分支

    git remote remove [name]

从远程分支拉取最新代码

    git pull [remote-name][branch-name] 
    
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

查看 git 提交历史
    
    git log

保存 Git账号密码

    git config --global credential.helper store

清楚之前保存的账号密码

1. 从控制面板删除

    打开控制面板 依次点开 控制面板\用户帐户\凭据管理器

    ![gitdelete.png](https://upload-images.jianshu.io/upload_images/61189-feb84426b78a8741.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

    将对应凭据删除即可

2. 删除文件

    上述方法在我的电脑上不起作用,
    最后在检查User文件夹发现了 .gitconfig 文件 
    打开查看发现控制账号密码存储的是这个属性

![image.png](https://upload-images.jianshu.io/upload_images/61189-fa30929f50a297e6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

    并在同级文件夹下发现 .git-credentials 文件
    发现里面存的是账号密码

![image.png](https://upload-images.jianshu.io/upload_images/61189-cb563f0b7a60e6e3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](https://upload-images.jianshu.io/upload_images/61189-566cf60dcae2e139.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

    将 .gitconfig 文件中 [credential] 属性删除
    并将 .git-credentials 文件删除即可

Git 迁移远程仓库

    换新仓库 保留之前的提交记录
    直接添加新的remote 然后提交即可