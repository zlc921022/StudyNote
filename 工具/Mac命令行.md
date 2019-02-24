# Mac 命令行

显示隐藏文件夹

    defaults write com.apple.finder AppleShowAllFiles -boolean true ; killall Finder

隐藏隐藏文件夹

    defaults write com.apple.finder AppleShowAllFiles -boolean false ; killall Finder
    
创建文件夹

    mkdir [name]

删除空文件夹
    
    rmdir 目录

删除非空文件夹
    
    rm -rf 目录
    -r 就是向下递归，不管有多少级目录，一并删除 
    -f 就是直接强行删除，不作任何提示的意思
    