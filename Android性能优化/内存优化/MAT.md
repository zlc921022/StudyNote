# Memory Analyzer (MAT)

    强大的Java Heap分析工具,查找内存泄露,查看内存占用
    生成整体报告,分析问题
    配合 Android Profiler 深层次的定位内存泄露

    下载链接: https://www.eclipse.org/mat/downloads.php
    
    将 hprof -> conv 需要转换文件格式才能识别

## 使用步骤

    使用命令 hprof-conv 将文件转换为可以查看的文件,并使用MAT打开文件

![image.png](https://upload-images.jianshu.io/upload_images/61189-5e629905d97d74f4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](https://upload-images.jianshu.io/upload_images/61189-597e55f3320db356.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

    点击查看 发现这个Activity 有4个实例 判断他这里发生了内存泄露

![image.png](https://upload-images.jianshu.io/upload_images/61189-c7a08f5f19fb7199.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

    点击可以查看有那些强引用引用着这个Activity

![image.png](https://upload-images.jianshu.io/upload_images/61189-2824b0043ee1143a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

    点击查看 GC Root 查看所有的引用

![image.png](https://upload-images.jianshu.io/upload_images/61189-465279b5a165f84c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](https://upload-images.jianshu.io/upload_images/61189-b5c80918449cd978.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
