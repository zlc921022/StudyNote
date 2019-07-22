# AIDL

    Client 调用 asInterface 方法,作用是判断参数(即IBinder对象),与自己是否在同一进程
        若是 则直接转换,直接使用,接下来则与Binder跨进程通信无关
        若否 则把这个IBinder参数包装成一个Proxy对象,这时调用Stub的sum方法,间接调用Proxy的sum方法

    Proxy 在自己的sum方法中,会使用Parcelable来准备数据,将函数名称,函数参数都写入_data,让_reply接收函数返回值,最后使用 IBinder 的transact方法,就可以将数据传给Binder的Server端了

    Server 通过onTransact方法接收Client进程传来的数据,包括函数名称,函数参数,找到对应的函数,把参数传进去,得到结果,并返回.所以onTransact函数经历了读数据->执行要调用的函数->把执行结果再写数据的过程

## 支持的数据类型

    * Java的基本数据类型 (byte short int long float double boolean char)
    * String
    * CharSequence
    * List 
        艺术探索写的只支持ArrayList, 我测试了下支持所有的List
        List里可以写泛型 但要是 aidl 支持的类型
    * Map 
        Map中的所有元素必须是AIDL支持的类型之一, 或者是一个其他AIDL生成的接口, 或者是定义的parcelable. 
        Map支持泛型

[Android：学习AIDL，这一篇文章就够了(上)](https://blog.csdn.net/luoyanglizi/article/details/51980630)<br/>
[Android：学习AIDL，这一篇文章就够了(下)](https://blog.csdn.net/luoyanglizi/article/details/52029091)<br/>
[android IPC通信（下）－AIDL](https://juejin.im/post/584d11e22f301e00572c779f)<br/>

[github](https://github.com/CodeLiuPu/IPC-demo.git)