

    Android消息处理机制,一个线程只会有一个 Looper 
    mLooging 对象 在每个 Message 处理前后被调用
    主线程发生卡顿,是在 dispatchMessage 执行了耗时操作

    Looper.getMainLooper().setMessageLogging()
    来将Printer 设置为我们自己的Printer

    匹配 ">>>>> Dispatching to "  阈值时间后执行任务(获取堆栈信息)
    匹配 和 "<<<<< Finished to ",任务之前结束掉

``` java
public class App extends Application {
    long startTime = 0;
    @Override
    public void onCreate() {
        super.onCreate();
        Looper.getMainLooper().setMessageLogging(new Printer() {
            @Override
            public void println(String x) {
                if (x.contains(">>>>> Dispatching to")) {
                    startTime = SystemClock.uptimeMillis();
                } else if (x.contains("<<<<< Finished to")) {
                    long costTime = SystemClock.uptimeMillis() - startTime;
                    Log.e("Update", "Update View cost time " + costTime + " x = " + x);
                }
            }
        });
    }
}
```
http://blog.zhaiyifan.cn/2016/01/16/BlockCanaryTransparentPerformanceMonitor/