# AIDL

    Client 调用 asInterface 方法,作用是判断参数(即IBinder对象),与自己是否在同一进程
        若是 则直接转换,直接使用,接下来则与Binder跨进程通信无关
        若否 则把这个IBinder参数包装成一个Proxy对象,这时调用Stub的sum方法,间接调用Proxy的sum方法

    Proxy 在自己的sum方法中,会使用Parcelable来准备数据,将函数名称,函数参数都写入_data,让_reply接收函数返回值,最后使用 IBinder 的transact方法,就可以将数据传给Binder的Server端了

    Server 通过onTransact方法接收Client进程传来的数据,包括函数名称,函数参数,找到对应的函数,把参数传进去,得到结果,并返回.所以onTransact函数经历了读数据->执行要调用的函数->把执行结果再写数据的过程
