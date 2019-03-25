# StrictMode(严苛模式)

    严苛模式,Android提供的一种运行时检测机制
    方便强大,容易被忽视
    包含:
        线程策略和虚拟机策略检测

    线程策略

        自定义的耗时调用 detectCustomSlowCalls()
        磁盘读取操作 detectDiskReads
        磁盘写入操作 detectDiskWrites
        网络请求操作 detectNetwork

    虚拟机策略

        Activity泄露 detectActivityLeaks()
        Sqlite对象泄露 detectLeakedSqlliteObjects
        未关闭的Closable对象泄露 使用detectLeakedClosableObjects()开启
        检测实例数量 setClassInstanceLimit

    监控输出

        日志形式打印 penaltyLog()
        弹窗形式输出 penaltyDialog()
        崩溃形式输出 penaltyDeath()


``` java
StrictMode.setThreadPolicy(new StrictMode.ThreadPolicy.Builder()
            .detectCustomSlowCalls()
            .detectDiskReads()
            .detectDiskWrites()
            .detectNetwork()
            .penaltyLog()
            .build());
StrictMode.setVmPolicy(new StrictMode.VmPolicy.Builder()
            .detectActivityLeaks()
            .detectLeakedClosableObjects()
            .setClassInstanceLimit(MainActivity.class,1)
            .penaltyLog()
            .build());
```