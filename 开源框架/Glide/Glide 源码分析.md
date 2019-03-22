# Glide 源码分析

## 提出问题

    Glide 缓存机制
    Glide 如何管理图片的生命周期

``` java
Glide
     .with(activity)
     .load("imageUrl")
     .into(new ImageView(activity));
```
      
## with
需要传入一个Context

有重载方法,需要我们自己根据上下文来选择使用对应的方法
获取到对应的 RequestManager 对象,管理图片请求的

``` java
 public static RequestManager with(Context context) {
        RequestManagerRetriever retriever = RequestManagerRetriever.get();
        return retriever.get(context);
    }
```

Glide的生命周期

``` java
 public interface LifecycleListener {
        void onStart();
        void onStop();
        void onDestroy();
    }
```

 即传入Application类型的参数，和传入非Application类型的参数。
 
 Application对象的生命周期即应用程序的生命周期，因此Glide并不需要做什么特殊的处理，它自动就是和应用程序的生命周期是同步的，如果应用程序关闭的话，Glide的加载也会同时终止。
 
 非Application参数的情况。不管你在Glide.with()方法中传入的是Activity、FragmentActivity、v4包下的Fragment、还是app包下的Fragment，最终的流程都是一样的，那就是会向当前的Activity当中添加一个隐藏的Fragment。


## load
需要传入一个 资源链接 可以是网络链接 也可以是本地图片地址


## into
参数可以是ImageView


缓存 
key是 url 宽高之类的
value 是自定义的一个类包含 resource对象 key等参数

## 缓存机制

    当 获取 内存缓存 时，会通过两个方法分别从上述两块区域进行缓存获取

        loadFromCache()：从 使用了 LruCache算法机制的内存缓存获取 缓存
        loadFromActiveResources()：从 使用了 弱引用机制的内存缓存获取 缓存

## 监听Activity的生命周期 监听到onPause 之后是如何回收的呢

    RequestManager.pauseRequests();

![](https://upload-images.jianshu.io/upload_images/61189-07fcc637508d0f6b.png)

    第一行是判断是否在主进程
    跟进第二行

![](https://upload-images.jianshu.io/upload_images/61189-7f0bc389ea318e22.png)

    这里主要逻辑走在 request.pause();

点进Request 查看 发现是个接口 

![image.png](https://upload-images.jianshu.io/upload_images/61189-bba0c9530e79d2f0.png)

点开他的实现类 并搜索pause
![image.png](https://upload-images.jianshu.io/upload_images/61189-f25c8836264b22a7.png)

![image.png](https://upload-images.jianshu.io/upload_images/61189-fc0b888c88ed5ca9.png)

发现 主要逻辑还是在 clear方法中

![image.png](https://upload-images.jianshu.io/upload_images/61189-3ba8b102487bef04.png)

重点看 releaseResource() 一层层点进去

![image.png](https://upload-images.jianshu.io/upload_images/61189-28ebe9c53cabc6ba.png)

![image.png](https://upload-images.jianshu.io/upload_images/61189-e2a1e4f6000e73d7.png)

![image.png](https://upload-images.jianshu.io/upload_images/61189-4189ddb5f13e85e9.png)

![image.png](https://upload-images.jianshu.io/upload_images/61189-29d485941454aafc.png)

![image.png](https://upload-images.jianshu.io/upload_images/61189-d1503da46e0fb4dd.png)


这里可以看到 最后会将图片缓存到内存中

## 遇到的问题

    You cannot start a load for a destroyed activity

    加载图片的时候,Activity已经被Destroyed

    解决方法:

        在加载图片之前,先进行Activity是否Destroy的判断.