

    Android消息处理机制,一个线程只会有一个 Looper 
    mLooging 对象 在每个 Message 处理前后被调用
    主线程发生卡顿,是在 dispatchMessage 执行了耗时操作

    Looper.getMainLooper().setMessageLogging()
    来将Printer 设置为我们自己的Printer

    匹配 ">>>>> Dispatching to "  阈值时间后执行任务(获取堆栈信息)
    匹配 和 "<<<<< Finished to ",任务之前结束掉

    costTime 通过判断 dispatchMessage 消耗的时间是否超过阈值来对卡顿进行检测
    debug 可以直接弹 弹窗提示
    release 可以发送回后台

    我把核心代码抽取了个简单的帮助类来帮助理解
    还有日志保存到本地以及回传给云端可以自行实现

``` java
public class BlockHelper {

   private Thread currThread;
   private long startTime = 0;
   private Runnable runnable = new Runnable() {
        @Override
        public void run() {
            doSample();
        }
    };

    public BlockHelper init(Thread thread) {
        currThread = thread;
        return this;
    }

    public void start(){
        Looper.getMainLooper().setMessageLogging(new Printer() {
            @Override
            public void println(String x) {
                if (x.contains(">>>>> Dispatching to")) {
                    startTime = SystemClock.uptimeMillis();
                    new Thread(runnable).start();
                } else if (x.contains("<<<<< Finished to")) {
                    long costTime = SystemClock.uptimeMillis() - startTime;
                    Log.e("Update", "Update View cost time " + costTime + " x = " + x);
                }
            }
        });
    }

    private void doSample() {
        StringBuilder stringBuilder = new StringBuilder();

        for (StackTraceElement stackTraceElement : currThread.getStackTrace()) {
            stringBuilder
                    .append(stackTraceElement.toString())
                    .append(" Updateya\n ");
        }
        Log.e("Update", "Update View StackTrace " + stringBuilder.toString());
    }


    private BlockHelper(){

    }

    public static BlockHelper instance(){
        return Holder.INSTANCE;
    }
    private static final class Holder{
        private static final BlockHelper INSTANCE = new BlockHelper();
    }
}
```
    
    输出的日志

``` java
07-17 15:24:31.322 5132-5168/com.update.test E/Update: Update View StackTrace android.os.MessageQueue.nativePollOnce(Native Method) Updateya
     android.os.MessageQueue.next(MessageQueue.java:143) Updateya
     android.os.Looper.loop(Looper.java:122) Updateya
     android.app.ActivityThread.main(ActivityThread.java:5293) Updateya
     java.lang.reflect.Method.invoke(Native Method) Updateya
     java.lang.reflect.Method.invoke(Method.java:372) Updateya
     com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:903) Updateya
     com.android.internal.os.ZygoteInit.main(ZygoteInit.java:698) Updateya
```
http://blog.zhaiyifan.cn/2016/01/16/BlockCanaryTransparentPerformanceMonitor/