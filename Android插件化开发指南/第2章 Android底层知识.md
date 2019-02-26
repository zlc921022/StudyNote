
## Binder 

    Binder 分为 Client 和 Server 两个进程,其中发送消息的是Client,接收消息的是Server

## AIDL

    Client 调用 asInterface 方法,作用是判断参数(即IBinder对象),与自己是否在同一进程
        若是 则直接转换,直接使用,接下来则与Binder跨进程通信无关
        若否 则把这个IBinder参数包装成一个Proxy对象,这时调用Stub的sun方法,间接调用Proxy的sum方法

    Proxy 在自己的sum方法中,会使用Parcelable来准备数据,将函数名称,函数参数都写入_data,让_reply接收函数返回值,最后使用 IBinder 的transact方法,就可以将数据传给Binder的Server端了

    Server 通过onTransact方法接收Client进程传来的数据,包括函数名称,函数参数,找到对应的函数,把参数传进去,得到结果,并返回.所以onTransact函数经历了读数据->执行要调用的函数->把执行结果再写数据的过程


## AMS(ActivityManagerService)

    AMS主要负责和所有App的四大组件进行通信

## Activity 工作原理

### App 怎么启动

* Launcher通知AMS,要启动斗鱼App,而且指定要启动斗鱼App的那个界面(首页)
* AMS 通知Launcher收到请求,同时将要启动的首页记录下来
* Launcher当前页面进入 Pause 状态,然后通知AMS,进入休眠状态,可以对要斗鱼App进行操作了
* AMS 检查斗鱼App 是否已经启动了,
    是 唤起斗鱼App 即可
    否 启动一个新的进程,AMS在新进程中创建一个ActivityThread对象,启动其中的main函数
* 斗鱼App启动后,通知AMS 启动完成
* AMS 查找出之前存的启动页面,通知斗鱼App启动那个页面
* 斗鱼App启动首页,创建Context并与首页Activity关联,然后调用首页Activity的onCreate函数

    上述步骤可分2个部分 
        1~3 Launcher与AMS通信
        4~7 斗鱼App与AMS通信

#### Launcher通知AMS
    

