# Handler机制

## 提出问题

    说下Handler机制

    主线程创建的Handler对象,构造里传入一个子线程的Looper,那么这个handler handleMessage()方法,工作的是线程是主线程的还是子线程的?

    Looper通过MessageQueque取消息，消息队列是先进先出模式,那我延迟发两个消息,第一个消息延迟2个小时,第二个消息延迟1个小时,那么第二个消息需要等3个小时才能取到吗？

    使用 Handler 需要注意什么?

    postRunable post 的内存泄露引用链

    Android中为什么非UI线程不能更新UI

## Handler 机制介绍

    主要有5个类
    Handler 负责发消息和处理消息
    Message 消息对象
    MessageQueue 消息队列,负责存储消息对象 本质是优先队列
    Looper 消息轮询器 负责从MQ中取消息并传给Handler,让其处理
    ThreadLocal 保存Looper

    发送消息

        Handler 使用 sendMessage() 方法发送消息,
        最后调用到 MessageQueue 的 enqueueMessage()方法,向队列里添加消息
        这里有个关键点 Message 有个值 when(when = 当前时间戳+delay),添加消息的时候会将when的大小当做优先级将MessageQueue 重新排列,

    轮训消息

        Looper.loop();

    接收并处理消息

        Looper 会从 MessageQueue 中取出消息
        然后会发现有这个逻辑 msg.target.dispatchMessage(msg); 其中 target 就是Handler
        Handler 的 dispatchMessage() 方法 最终调用的是 handleMessage() 方法

## handleMessage线程问题

![image.png](https://upload-images.jianshu.io/upload_images/61189-c053e62a400b816b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
    
    可以倒着推,上图可以看到 Handler的handleMessage()方法 是在 Handler 的 dispatchMessage()中调用的我们可以查找下 dispatchMessage() 方法调用的时机

![image.png](https://upload-images.jianshu.io/upload_images/61189-becaffb074dd2e9b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](https://upload-images.jianshu.io/upload_images/61189-77a06d8206aa2a22.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

    这里直接给出答案,dispatchMessage() 会再Looper的 loop() 方法中被调用,上面Message的target 就是发送Message的 Handler.
    这个Message 是从 MessageQueue 取出来的,所以他跟 MessageQueue 在同一线程

    全局搜索可以发现 这个MessageQueue是由Looper维护的 而Looper是Handler 构造函数传进来的,

![](https://upload-images.jianshu.io/upload_images/61189-6337151f183cfeea.jpg)

### 结论

    Handler跟其绑定的Looper 在同一个线程

## ThreadLocal

    ThreadLocal 内部有个 ThreadLocalMap 来存储对象 但是是非静态的 所以每个线程都各有一份

## static 关键字修饰

    Java 中 匿名内部类和非静态内部类都会隐式的持有一份对外部类的引用，而静态的内部类则不会包含对外部类的引用。

![image.png](https://upload-images.jianshu.io/upload_images/61189-417b0a040f9754a9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](https://upload-images.jianshu.io/upload_images/61189-30f142348aa79348.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

    如上图这样给匿名类添加static,还会报warn

    有下面3种解决方案,推荐使用第三种

### 创建 Handler对象 传入Callback

![image.png](https://upload-images.jianshu.io/upload_images/61189-47b578ed826894e9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](https://upload-images.jianshu.io/upload_images/61189-c5b1001c70caddb9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

    如上图 Java核心技术第10版给出的定义是:
        如果构造参数的闭小括号后面跟一个开大括号,正在定义的就是匿名内部类。
        所以 该方式是创建了一个对象,而非匿名内部类,所以不会持有外部类的引用

### 为Handler 指定Looper
![image.png](https://upload-images.jianshu.io/upload_images/61189-3f1400c151ace42e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 自己继承Handler定义一个静态类

![image.png](https://upload-images.jianshu.io/upload_images/61189-87cfc55cecd3c657.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

    该方式是创建了静态内部类,这样就不会不会持有外部类的引用

## Android中为什么非UI线程不能更新UI

## Handler 的 delay 可靠吗

    不可靠
    1. 有误差
    2. 需要等待前置任务执行完毕才会执行


## 小TIP:

![](https://upload-images.jianshu.io/upload_images/61189-8a0f9cc3e6728cf3.jpg)

    这里会检测是否有传callback 若有传就不会走handleMessage了