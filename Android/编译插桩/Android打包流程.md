# Android 打包流程

## 1 编译打包流程介绍

![build.png](https://upload-images.jianshu.io/upload_images/61189-3cdc2fb9eb46af2f.png)

### 1.1 Java -> class

    aapt工具编译res资源文件，把大部分xml文件编译成二进制文件(图片文件除外),同时生成R.Java文件和resources.arsc文件，里面保存了资源的ID和在APK中的路径。
    如果项目中有使用AIDL，那么就会把.aidl文件编译成.java文件。
    将所有.java文件(包括R文件和AIDL生成的.java文件)，通过javac工具生成class文件。

### 1.2 class -> dex

    将生成的.class文件和第三方库的.class文件通过dx工具生成classes.dex文件(如果有分包，那么可能有多个)。

### 1.3 dex -> apk

    第1步中的资源文件、dex文件和第三方的非java资源包(.so)，通过apkbuilder工具生成未签名的apk包。

### 1.4 sign

    签名，jarsigner工具，如果是debug模式用默认签名，release模式用开发者的签名。

    apksigner

    V1签名(jarsigner):
        来自JDK(jarsigner), 对zip压缩包的每个文件进行验证, 签名后还能对压缩包修改(移动/重新压缩文件)
        对V1签名的apk/jar解压,在META-INF存放签名文件(MANIFEST.MF, CERT.SF, CERT.RSA),
        其中MANIFEST.MF文件保存所有文件的SHA1指纹(除了META-INF文件), 由此可知: V1签名是对压缩包中单个文件签名验证

    V2签名:
    来自Google(apksigner), 对zip压缩包的整个文件验证, 签名后不能修改压缩包(包括zipalign),
    对V2签名的apk解压,没有发现签名文件,重新压缩后V2签名就失效, 由此可知: V2签名是对整个APK签名验证
    V2签名优点很明显:
    签名更安全(不能修改压缩包)
    签名验证时间更短(不需要解压验证),因而安装速度加快
    注意: apksigner工具默认同时使用V1和V2签名,以兼容Android 7.0以下版本

### 1.5 zipalign

    对齐，通过zipalign工具对apk中的未压缩资源（图片、视频）进行“对齐操作”，让资源按4字节的边界进行对齐，使得资源访问速度更快。（优化思想类似于内存对齐，

## 2 Transform 

### 2.1 介绍

    上述的过程 每一个步骤 其实都是一个Transform
    每个 Transform 都是一个 Gradle Task
    Android编译器中的 TaskManager 会将每个 Transform 串起来

    关于 TaskManager 如何管理 可以查看 TaskManager # createPostCompilationTasks() 方法

### 2.2 过滤机制

#### 2.2.1 ContentType

    中文翻译为数据类型

    插件开发中,我们常用的就是 CLASSES 和 RESOURCES 两种类型,需要注意的是 CLASSES 类型 已经包含了 class 文件以及 jar 文件

#### 2.2.2 Scope

    作用域

    TransformManager.SCOPE_FULL_PROJECT

![transformconsume_transform.png](https://upload-images.jianshu.io/upload_images/61189-85778a7046b23933.png)

[Android的编译打包流程详解](https://www.jianshu.com/p/019c735050e0)</br>
[Android APK打包流程](https://juejin.im/post/5cd0046fe51d453aa5635f98)</br>
[一篇文章看明白 Android v1 & v2 签名机制](https://blog.csdn.net/freekiteyu/article/details/84849651)</br>
[改善android性能工具篇【zipalign】](https://www.cnblogs.com/hnlshzx/p/3483995.html)</br>