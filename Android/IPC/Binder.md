# Binder 

## 介绍

    Binder是Android系统 进程间通信(IPC)的方式之一
    
    用了动态代理模式
    内存映射

    Binder Client 
        只需要知道自己要使用的 Binder 的名字及其在 ServiceManager 中的引用即可获取该 Binder 的引用，得到引用后就可以像普通方法调用一样调用 Binder 实体的方法;

    Binder Server 
        在生成一个 IBinder 实体时会为其绑定一个名称并传递给 Binder Driver，Binder Driver 会在内核空间中创建相应的 Binder 实体节点和节点引用，并将引用传递给 ServiceManager。ServiceManager 会将该 Binder 的名字和引用插入一张数据表中，这样 Binder Client 就能够获取该 Binder 实体的引用，并调用上面的方法;

    ServiceManager 
        相当于 DNS服务器，负责映射 Binder 名称及其引用,其本质同样是一个标准的 Binder Server；

    Binder Driver 
        则相当于一个路由器

## 优点

    Linux已经拥有管道,socket等IPC手段.Android 却还要倚赖Binder来实现进程间通信,那Binder 有什么优势呢

    
[Binder学习指南 weishu](http://weishu.me/2016/01/12/binder-index-for-newer/)<br/>
[Android Bander设计与实现 - 设计篇](https://blog.csdn.net/universus/article/details/6211589)<br/>
[Android进程间通信（IPC）机制Binder简要介绍和学习计划](https://blog.csdn.net/luoshengyang/article/details/6618363)<br/>
[写给 Android 应用工程师的 Binder 原理剖析](https://zhuanlan.zhihu.com/p/35519585)<br/>
