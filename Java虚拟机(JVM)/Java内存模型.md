
# Java内存模型

![](https://upload-images.jianshu.io/upload_images/61189-b2d08ae9ab427c36.jpg)


线程共享区:

    方法区: 存储类信息,常量,静态变量
    
    Java堆: 存储对象实例
    
线程独占区:

    虚拟机栈: 存储方法运行时所需数据,称为栈帧
        存的是引用
        
    本地方法区: 为JVM调用到的Native(本地方法)服务
    
    程序计数器