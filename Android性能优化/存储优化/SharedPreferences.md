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



## 用来跨进程

    MODE_MULTI_PROCESS 通过这个 Flag 可以实现跨进程

    在SharedPreferenceImpl里面, 没有发现任何对这个Flag的使用; 
    对这个 Flag 的使用 是在 ContextImpl # getSharedPreference()

### 原理

    这个flag保证了在API 11以前的系统上, 如果sp已经被读取进内存,再次获取这个sp的时候,如果有这个flag,会重新读一遍文件,仅此而已!

    所以想要使用这个Flag来实现 跨进程通信,是不可靠的



[彻底搞懂 SharedPreferences](https://juejin.im/entry/597446ed6fb9a06bac5bc630)</br>
[请不要滥用SharedPreference](http://weishu.me/2016/10/13/sharedpreference-advices/)</br>