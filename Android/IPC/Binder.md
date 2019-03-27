# Binder 

## 介绍
    Binder是Android系统 进程间通信(IPC)的方式之一
    
    用了动态代理模式
    内存映射

    使用反射来实现
    Binder 分为 Client 和 Server 两个进程,其中发送消息的是Client,接收消息的是Server


## 优点

    Linux已经拥有管道,socket等IPC手段.Android 却还要倚赖Binder来实现进程间通信,那Binder 有什么优势呢

    
