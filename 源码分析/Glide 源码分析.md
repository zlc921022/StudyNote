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

