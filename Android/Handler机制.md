# Handler机制

## 提出问题

    说下Handler机制

    主线程创建的Handler对象,构造里传入一个子线程的Looper,那么这个handler handleMessage()方法,工作的是线程是主线程的还是子线程的?

    Looper通过MessageQueque取消息，消息队列是先进先出模式,那我延迟发两个消息,第一个消息延迟2个小时,第二个消息延迟1个小时,那么第二个消息需要等3个小时才能取到吗？

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

    查看发送消息方法可以看到 handler传消息是直接传给自己对于的MQ,这个MQ是个全局的,需要去找到MQ在那里赋值

![](https://upload-images.jianshu.io/upload_images/61189-2b8245b68a041291.jpg)

    全局搜索可以发现 这个MQ是由Looper维护的 这个Looper是Handler 构造函数传进来的,

![](https://upload-images.jianshu.io/upload_images/61189-6337151f183cfeea.jpg)

    所以Handler跟其绑定的Looper 在同一个线程

ThreadLocal 内部有个 ThreadLocalMap 来存储对象 但是是非静态的 所以每个线程都各有一份

![](https://upload-images.jianshu.io/upload_images/61189-8a0f9cc3e6728cf3.jpg)

这里会检测是否有传callback 若有传就不会走handleMessage了

## static 关键字修饰

    Java 中 匿名内部类和非静态内部类都会隐式的持有一份对外部类的引用，而静态的内部类则不会包含对外部类的引用。

![image.png](https://upload-images.jianshu.io/upload_images/61189-417b0a040f9754a9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](https://upload-images.jianshu.io/upload_images/61189-30f142348aa79348.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

    如上图这样给匿名类添加static,还会报warn

有下面3种解决方案,推荐使用第三种

Handler 传入Callback 即可

![image.png](https://upload-images.jianshu.io/upload_images/61189-47b578ed826894e9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

为Handler 指定Looper
![image.png](https://upload-images.jianshu.io/upload_images/61189-3f1400c151ace42e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

自己继承Handler定义一个静态类

![image.png](https://upload-images.jianshu.io/upload_images/61189-87cfc55cecd3c657.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
