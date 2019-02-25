
Binder 

    Binder 分为 Client 和 Server 两个进程,其中发送消息的是Client,接收消息的是Server

AIDL

    asInterface方法的作用是判断参数(即IBinder对象),与自己是否在同一进程
    若是 则直接转换,直接使用,接下来则与Binder跨进程通信无关
    若否 则把这个IBinder参数包装成一个Proxy对象,这时调用Stub的sun方法,间接调用Proxy的sum方法
AMS

Activity 启动流程