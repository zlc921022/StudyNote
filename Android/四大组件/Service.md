# Service

    bindService 模式
        第一次 bindService()的时候，执行的方法为 onCreate()、onBind()解除绑定的时候会执行 onUnbind()、onDestory()。

    startService 模式
        当第一次调用 startService 的时候执行的方法依次为 onCreate()、onStartCommand()，(onStart()) 当 Service 关闭的时候调用 onDestory 方法。

![](https://upload-images.jianshu.io/upload_images/61189-3d5c0d91270c6953.png)

``` java

@Override
public void onCreate() {
    super.onCreate();
    // 只在 Service 创建的时候调用一次
}

@Override
public int onStartCommand(Intent intent, int flags, int startId) {
    // 其他组件 调用 startService 的时候会被调用 (每次都会调用)
    // 先调用 onStartCommand 后 onStart
    return super.onStartCommand(intent, flags, startId);
}

@Override
public void onStart(Intent intent, int startId) {
    super.onStart(intent, startId);
    // 其他组件 调用 startService 的时候会被调用 (每次都会调用)
    // 先调用 onStartCommand 后 onStart
}

@Override
public IBinder onBind(Intent intent) {
    // 其他组件 调用 bindService 的时候会被调用 (只调用一次)
    return null;
}

@Override
public boolean onUnbind(Intent intent) {
    // 其他组件 调用 unbindService 的时候会被调用 (只调用一次)
    return super.onUnbind(intent);
}
```

## bind 和 start 交叉调用

    Service 的 onCreate 只会调用一次

    bindService
        onBind 也是只会调用一次

    startService
        每调用一次 startService 
            onStartCommand & onStart 都会先后调用

## 如何结束

    stopSelf()

    可以通过广播 来接受结束的消息,让自己主动结束