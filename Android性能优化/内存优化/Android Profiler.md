# Memory Profiler(Android Profiler)

    实时图标方式展示应用内存使用量
    识别内存泄露,内存抖动
    提供捕获堆转储,强制GC以及跟踪内存分配的能力

## 使用 Memory Profiler 对 Bitmap 占内存分析

![image.png](https://upload-images.jianshu.io/upload_images/61189-8b293c9fb614c5ab.png)

![image.png](https://upload-images.jianshu.io/upload_images/61189-8730dc3799c7f510.png)

    在图中可看到堆中所有的Bitmap对象,列表上显示了Bitmap所占的内存大小,点击后可以查看对应是预览图

    Shallow size就是对象本身占用内存的大小，不包含其引用的对象。
    Retained size是该对象自己的shallow size，加上从该对象能直接或间接访问到对象的shallow size之和。换句话说，retained size是该对象被GC之后所能回收到内存的总和。

## 使用 Memory Profiler 查看内存对象对应的源码

![image.png](https://upload-images.jianshu.io/upload_images/61189-77c6e99f26674efd.png)

## 使用 Memory Profiler 按照包名来查看内存信息

![image.png](https://upload-images.jianshu.io/upload_images/61189-d50ef82ecc79a09a.png)
