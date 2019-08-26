# UncaughtExceptionHandler

## 提出问题

    * 如果想对子线程也做监控,是否需要在子线程也调用 setDefaultUncaughtExceptionHandler() 方法
    * Thread # setDefaultUncaughtExceptionHandler() 只能设置一个 UncaughtExceptionHandler对象,如果其他库也设置了这个监听,如何保证不影响其他库的逻辑?
   
## setDefaultUncaughtExceptionHandler
    源码中实现
``` java
// 使用 static volatile修饰 所以是线程共享的 设置一次全部线程都起效
private static volatile UncaughtExceptionHandler defaultUncaughtExceptionHandler;

public static void setDefaultUncaughtExceptionHandler(UncaughtExceptionHandler eh) {
    defaultUncaughtExceptionHandler = eh;
}
```

## 如果保证不影响其他的库
    将默认的监听保存起来,处理完自己的逻辑在分发给默认的监听就好了
    代码如下
    
``` java
private Thread.UncaughtExceptionHandler defaultUncaughtExceptionHandler;

public void init() {
    defaultUncaughtExceptionHandler = Thread.getDefaultUncaughtExceptionHandler();
    Thread.setDefaultUncaughtExceptionHandler(this);
}

@Override
public void uncaughtException(Thread t, Throwable ex) {
    handleThreadException(t, ex);
    if (defaultUncaughtExceptionHandler != null) {
        defaultUncaughtExceptionHandler.uncaughtException(t, ex);
    }
}
```