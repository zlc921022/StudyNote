# HandlerThread

    HandlerThread本质上就是一个普通Thread,只不过内部替我们建立了Looper.
    简化了我们创建 含有 Looper 的线程的步骤

``` java
@Override
public void run() {
    mTid = Process.myTid();
    Looper.prepare();
    synchronized (this) {
        mLooper = Looper.myLooper();
        notifyAll();
    }
    Process.setThreadPriority(mPriority);
    onLooperPrepared();
    Looper.loop();
    mTid = -1;
}
```