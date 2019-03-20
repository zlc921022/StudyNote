# ANR

    Application Not Response 应用无响应,一般是因为在主线程进行耗时操作而导致主线程卡死

## ANR 日志获取

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

