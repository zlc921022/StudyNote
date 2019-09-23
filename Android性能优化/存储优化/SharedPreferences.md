# SharedPreferences

## 提出问题

    sp多线程同步问题
    apply commit区别

## 查看源码

    实现逻辑在 SharedPreferencesImpl 类中

``` java
class SharedPreferencesImpl implements SharedPreferences
```

### ContextImpl # getSharedPreferences() 方法

``` java
SharedPreferences getSharedPreferences(File file, int mode)
```

## 多次 apply 会导致界面卡顿

    会先将修改封装成 一个带有await的runnable,并添加进了QueueWork类的一个队列,用作监听
    然后把这个写入任务通过 enqueueDiskWrite() 交给一个只有单个线程的线程池去执行
    
    ActivityThread # handleStopActivity() {
        QueuedWork.waitToFinish();
    }

    Activity 会在Pause(Api 11 以下) 或 Stop(Api 11 以上) 的时候, 会对sp的 apply进行监控, 如果没有写入完毕 就会等待写入任务完成,如果apply任务特别多的话,会导致界面卡顿

## 用来跨进程

    MODE_MULTI_PROCESS 通过这个 Flag 可以实现跨进程

    在SharedPreferenceImpl里面, 没有发现任何对这个Flag的使用; 
    对这个 Flag 的使用 是在 ContextImpl # getSharedPreference()

### 原理

    这个flag保证了在API 11以前的系统上, 如果sp已经被读取进内存,再次获取这个sp的时候,如果有这个flag,会重新读一遍文件,仅此而已!

    所以想要使用这个Flag来实现 跨进程通信,是不可靠的


### 注意事项

    1. 不要存放大的key和value,会引起界面卡顿,频繁GC,占用内存等
    2. 不相干的配置项分开保存
    3. 不要频繁的 apply 和commit,尽量修改完一起提交
    4. apply 比 commit 的效率更高


[彻底搞懂 SharedPreferences](https://juejin.im/entry/597446ed6fb9a06bac5bc630)</br>
[请不要滥用SharedPreference](http://weishu.me/2016/10/13/sharedpreference-advices/)</br>
[一个null引发的SharedPreferences惨案](https://mp.weixin.qq.com/s?__biz=MzA5MzI3NjE2MA==&mid=2650237991&idx=1&sn=7e80d3504c257d13cad7efea104eac5b&chksm=88639d48bf14145e6e9e051c5dde7dbe047b8ac97aaa73df09a3ebd97566dd694e162c1bd584&scene=38#wechat_redirect)</br>
[SharedPreferences 多进程问题](https://www.jianshu.com/p/2096c7fb9f64)</br>