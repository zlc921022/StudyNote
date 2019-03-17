# Service

    bindService模式
        第一次 bindService()的时候，执行的方法为 onCreate()、onBind()解除绑定的时候会执行 onUnbind()、onDestory()。

    startService模式
        当第一次调用 startService 的时候执行的方法依次为 onCreate()、onStartCommand()，(onStart()) 当 Service 关闭的时候调用 onDestory 方法。

![](https://upload-images.jianshu.io/upload_images/61189-3d5c0d91270c6953.png)