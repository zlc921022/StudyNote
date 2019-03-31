# ANR

    Application Not Response 应用无响应,一般是因为在主线程进行耗时操作而导致主线程卡死


## 造成 ANR 原因

    Service Timeout: 比如前台服务在20s内未执行完成
    BroadcastQueue Timeout: 比如前台广播在10s内未执行完成
    ContentProvider Timeout: 内容提供者,在publish过超时10s
    InputDispatching Timeout: 输入事件分发超时5s,包括按键和触摸事件

## Service超时监测机制

    Service运行在应用程序的主线程，如果Service的执行时间超过20秒，则会引发ANR。


## ANR 问题定位

### 获取 ANR 日志

``` java
adb shell kill -S QUIT PID 
adb pull /data/anr/traces.txt
```
traces.txt日志文件分析

一般traces.txt日志输出格式如下，本实例是在主线程中强行Sleep导致的ANR日志：

``` java
DALVIK THREADS:
(mutexes: tll=0 tsl=0 tscl=0 ghl=0)

"main" prio=5 tid=1 TIMED_WAIT
  | group="main" sCount=1 dsCount=0 obj=0xa4cc8bd8 self=0xb8b70eb0
  | sysTid=1635 nice=0 sched=0/0 cgrp=apps handle=-1217155008
  | state=S schedstat=( 41426029 22606846 158 ) utm=1 stm=2 core=2
  at java.lang.VMThread.sleep(Native Method)
  at java.lang.Thread.sleep(Thread.java:1013)
  at java.lang.Thread.sleep(Thread.java:995)
  at com.update.demo.MainActivity.startSleep(MainActivity.java:18)
  at com.update.demo.MainActivity.onCreate(MainActivity.java:11)
  at android.app.Activity.performCreate(Activity.java:5231)
  at android.app.Instrumentation.callActivityOnCreate(Instrumentation.java:1087)
  at android.app.ActivityThread.performLaunchActivity(ActivityThread.java:2148)
  at android.app.ActivityThread.handleLaunchActivity(ActivityThread.java:2233)
  at android.app.ActivityThread.access$800(ActivityThread.java:135)
  at android.app.ActivityThread$H.handleMessage(ActivityThread.java:1196)
  at android.os.Handler.dispatchMessage(Handler.java:102)
  at android.os.Looper.loop(Looper.java:136)
  at android.app.ActivityThread.main(ActivityThread.java:5001)
  at java.lang.reflect.Method.invokeNative(Native Method)
  at java.lang.reflect.Method.invoke(Method.java:515)
  at com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:785)
  at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:601)
  at dalvik.system.NativeStart.main(Native Method)

```

从上述内容可定位到是  MainActivity第18行的startSleep方法 如下图 修复即可

![](https://upload-images.jianshu.io/upload_images/61189-7b01cb37d0ad23c6.png)


[深入理解ANR](https://mp.weixin.qq.com/s?__biz=MzIyNTY4NjU0OQ==&mid=2247486156&idx=1&sn=c994406210364387393271748c590ded&chksm=e87aafb6df0d26a05ddeb10835eafc6dbb3a65a0bfc8b20f0c8cdab70f1b4b9ab375cbcfedf5&mpshare=1&scene=23&srcid=0322F041ehtZ5w1s5cR6fg1V#rd)</br>
[理解Android ANR的触发原理](http://gityuan.com/2016/07/02/android-anr/)</br>
[看完这篇 Android ANR 分析，就可以和面试官装逼了！](https://mp.weixin.qq.com/s?__biz=MzIwMTAzMTMxMg==&mid=2649493643&idx=1&sn=34b51d1f61bd2ecaa8fd0a2d39c4d1d1&chksm=8eec9b74b99b126246acc4547597dfe55c836b8f689b2d1a65bdf1ee2054ced2fc070bfa2678&mpshare=1&scene=23&srcid=03215dAQAhO0AmVSCH1Dd2Vw#rd)</br>
[Android ANR问题总结](https://www.jianshu.com/p/fa962a5fd939)</br>

