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

    Linux已经拥有管道,socket等IPC手段.
    Android 却还要倚赖Binder来实现进程间通信, 那Binder 有什么优势呢?

### 性能

    Socket 传输效率低,开销大,主要用于跨网络的进程间通信 和 本机上进程间的低速通信
    消息队列和管道采用 存储-转发方式 即数据先从发送方缓存区拷贝到内核开辟的缓存区中,然后再从内核缓存区拷贝到接收方缓存区中,至少有 2次 拷贝过程
    共享内存 虽然无需拷贝,但控制复杂,难以使用
    Binder 只需要一次数据拷贝,性能上仅次于共享内存

### 稳定性

    Binder 基于 C/S 架构 客户端有什么需求直接丢给 服务端完成,架构清晰,职责明确 又相互独立,稳定性好
    共享内存虽然无需拷贝 但是控制负责,难以使用
    从稳定性角度讲,Binder机制优于内存共享

### 安全性

[Binder学习指南 weishu](http://weishu.me/2016/01/12/binder-index-for-newer/)<br/>
[Android Bander设计与实现 - 设计篇](https://blog.csdn.net/universus/article/details/6211589)<br/>
[Android进程间通信（IPC）机制Binder简要介绍和学习计划](https://blog.csdn.net/luoshengyang/article/details/6618363)<br/>
[写给 Android 应用工程师的 Binder 原理剖析](https://zhuanlan.zhihu.com/p/35519585)<br/>
