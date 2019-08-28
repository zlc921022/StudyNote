# ActivityLifecycleCallbacks

    API 14之后，在Application类中，提供了一个应用生命周期回调的注册方法，用来对应用的生命周期进行集中管理，这个接口叫registerActivityLifecycleCallbacks，可以通过它注册自己的ActivityLifeCycleCallback，每一个Activity的生命周期都会回调到这里的对应方法。之前我们想做类似限制制定Activity个数的时候都要自己去添加和计数，有了ActivityLifeCycleCallback接口，所有Activity的生命周期都会在这里回调，我们可以根据条件随心处理。

## 用法

    可以做类似AOP的操作,比如埋点,打印日志
    对 Activity 添加 TitleView 之类的操作

## 原理

### 注册 

    Application # registerActivityLifecycleCallbacks(ActivityLifecycleCallbacks callback)

        用这个方法去注册 callback,然后在 Application 中会有一个List 去维护

### 分发

``` java
Application # dispatchActivityCreated(Activity activity, Bundle savedInstanceState) 
Application # dispatchActivityStarted(Activity activity) 
...
Application # dispatchActivityDestroyed(Activity activity)

```

    在上述方法中分发给Activity 
    其中 dispatchActivity*** 这些方法 会在Activity 对应的生命周期主动回调