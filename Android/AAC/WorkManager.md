# WorkManager

## 介绍

    WorkManager是google提供的异步执行任务的管理框架, 会根据手机的API版本和应用程序的状态来选择适当的方式执行任务
    当应用在运行的时候会在应用的进程中开一条线程来执行任务, 当退出应用时, WorkManager会选择根据设备的API版本使用适合的算法调用 JobSchedule r或者Firebase JobDispatcher, 或者AlarmManager来执行任务,并不用我们自己去考虑应用复杂的运行状态来进行选择

## 使用 

### 引入依赖

    implementation "android.arch.work:work-runtime:1.0.0-alpha01"

### 创建一个类 继承 Worker

``` java
public class TestWorker extends Worker {
    @Override
    public WorkerResult doWork() {
        Log.e("updata_tag","workkkkker b4");
        SystemClock.sleep(1000);
        Log.e("updata_tag","workkkkker after");
        return WorkerResult.SUCCESS;
    }
}
```

### 创建 Request

``` java
PeriodicWorkRequest request = new PeriodicWorkRequest
        .Builder(TestWorker.class,1, TimeUnit.SECONDS)
        .build();
WorkManager.getInstance().enqueue(request);
```

#### OneTimeWorkRequest 

    任务只执行一次

#### PeriodicWorkRequest

    重复执行任务,知道被取消才停止


[WorkManager完全解析+重构轮询系统](https://juejin.im/post/5c4472ec51882522c03e941d)