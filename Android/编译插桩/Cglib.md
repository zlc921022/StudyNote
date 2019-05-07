# Cglib

## 1. 介绍

## 2. 使用

    Idea 引入 jar包 后,直接就可以使用
   
### 2.1 使用遇到的坑

![93FA193C70FB6A228916786C8BA03699.png](https://upload-images.jianshu.io/upload_images/61189-de545aa4b538bf8d.png)

    如上图,我使用的是 cglib-3.2.12 jar包,提示 ClassNotFound 

    其实这里有个点需要注意下, 官网提供了 2 个版本的 jar 包,供开发者下载,具体可看下图

        cglib-nodep-3.2.12 内包含 asm jar包,
        cglib-3.2.12 内未包含 asm jar包,需要单独引入才能正常使用


![WechatIMG43.jpeg](https://upload-images.jianshu.io/upload_images/61189-67c047603605fe7e.jpeg) ![WechatIMG42.jpeg](https://upload-images.jianshu.io/upload_images/61189-0d3ee6247ddcc69a.jpeg)

    上面的错有2种解决方案
        1. 自行下载 asm jar包 导入,但可能会 版本不匹配的情况(不推荐)
        2. 下载 cglib-nodep jar包,nodep版本的是内置 asm jar包,所以不会出现版本不匹配的情况(推荐)
        