#OkHttp 源码分析

```

2-1 okhttp框架流程分析
2-2 okhttp同步请求方法
2-3 okhttp异步请求方法
2-4 okhttp同步请求流程和源码分析
2-5 okhttp异步请求流程和源码分析-1
2-6 okhttp异步请求流程和源码分析-2
2-7 okhttp任务调度核心类dispatcher解析-1
2-8 okhttp任务调度核心类dispatcher解析-2
2-9 okhttp拦截器流程
2-10 okhttp拦截器链介绍
2-11 okhttp之RetryAndFollowUpInterceptor解析
2-12 okhttp之BridgeInterceptor解析
2-13 okhttp缓存策略源码分析：put方法
2-14 okhttp缓存策略源码分析：get方法
2-15 okhttp拦截器之CacheInterceptor解析
2-16 okhttp拦截器之ConnectInterceptor解析-1
2-17 okhttp拦截器之ConnectInterceptor解析-2
2-18 okhttp连接池：put,get方法
2-19 okhttp连接池：connection回收
2-20 okhttp拦截器之CallServerInterceptor解析
2-21 okhttp面试: Socket-1
2-22 okhttp面试: Socket-2
2-23 okhttp面试: HttpClient&HttpUrlConnection
2-24 okhttp面试: OkHttp来实现WebSocket连接
2-25 okhttp面试: WebSocket&轮询相关
2-26 okhttp面试: Http缓存、Etag等标示作用
2-27 okhttp面试: 断点续传原理&Okhttp如何实现
2-28 okhttp面试：多线程下载
2-29 okhttp面试：文件上传&Okhttp如何处理文件上传
2-30 okhttp面试：如何解析Json类型数据
2-31 okhttp面试：Https／对称加密&不对称加密

```

## 提出问题

    OkHttp缓存机制
    OkHttp路由管理
    OkHttp请求流程
    OkHttp的OkIo

使用流程


###同步请求

``` java
    String url = "https://www.baidu.com/";
    OkHttpClient okHttpClient = new OkHttpClient();
    
    Request request = new Request.Builder()
          .url(url)
          .build();
          
    Call call = okHttpClient.newCall(request);
    
    Response response = call.execute();
```
* 创建`OkHttpClient`和`Request`对象
* 将`Request`封装成`Call`对象
* 调用`Call`的子类`RealCall`的`exectue`方法执行同步请求
    
    会阻塞当前线程

同步请求会将请求放入同步队列当中
```java
runningSyncCalls.add(call);
```



###异步请求

``` java
    String url = "https://www.baidu.com/";
    OkHttpClient okHttpClient = new OkHttpClient();
    
    Request request = new Request.Builder()
          .url(url)
          .build();
          
    Call call = okHttpClient.newCall(request);
    
    Response response = call.enqueue(new Callback() {
                @Override
                public void onFailure(Call call, IOException e) {
                    
                }

                @Override
                public void onResponse(Call call, Response response) throws IOException {

                }
            });
```

* 创建`OkHttpClient`和`Request`对象
* 将`Request`封装成`Call`对象
* 调用`Call`的`enqueue`方法执行异步请求,具体由AsyncCall来实现异步

    会开启一个新的工作线程来进行操作,Callback 是在工作线程(子线程)中回调
最终调用的是Dispatcher来进行分发
里面有正在执行异步请求的队列,等待异步请求的队列,还有线程池

最后会finish 
将call从队列中删除

核心类

``` java

Dispatcher exqueue()

getResponseWithInterceptorChain
```

缓存策略

任务调度

是Dispatcher来进行分发
里面有正在执行异步请求的队列,等待异步请求的队列,还有线程池
最后会finish 
将call从队列中删除

Dispatcher用于维护同步异步的请求状态,并维护一个线程池,用于执行网络请求

## 拦截器链

* 发起请求前对Request进行相应的处理
* 调用下一个拦截器,获取他的Response
* 对Response进行处理并返回给上一个拦截器

### RetryAndFollowUpInterceptor

失败重连 重定向

* 创建 StreamAllocation 对象(建立执行Http请求)所需要的所有的网络组件,分配Stream
* RealInterceptorChain.proceed 进行网络请求
* 根据异常结果或者响应结果判断是否要进行重新请求
* 调用下一个拦截器,对response进行处理,返回给上一个拦截器


### BridgeInterceptor

桥接 设置内容长度 编码方式,主要是添加头部的作用

* 负责将用户构建的一个Request请求转化为能够进行网络访问的请求
* 讲这个符合网络请求的Request进行网络请求
* 讲网络请求返回的响应Response转化为用户可用的Response

### CacheInterceptor

缓存的使用 DiskLruCache(这个类很重要)

put
会对方法进行判断 不缓存非Get方法的响应(只缓存Get方法的响应)
存入的key 使用的是url的md5并toHex了的key

最终是writeTo方法
    不仅存入了响应的头部信息,还有请求的头部信息


get

remove


### ConnectInterceptor

打开与服务器之间的链接

* 获取 Interceptor 传过来的StreamAllocation,streamAllocation.newStream()
* 创建用于网络IO的 RealConnection 对象和 服务器交互最为关键的HttpCodec等对象
* 将上面的对象传递给后面的拦截器

findHealthyConnection 这个方法很重要 

### CallServerInterceptor

用于将网络请求写入io流中

``` java
RealInterceptorChain realChain = (RealInterceptorChain) chain;
    HttpCodec httpCodec = realChain.httpStream();
    StreamAllocation streamAllocation = realChain.streamAllocation();
    RealConnection connection = (RealConnection) realChain.connection();
    Request request = realChain.request();
```

连接池:

ConnectionPool 
在时间限制内来复用 connection ,还有对连接的清理回收使用了GC回收算法

get

put
