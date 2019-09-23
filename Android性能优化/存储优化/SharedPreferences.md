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


[彻底搞懂 SharedPreferences](https://juejin.im/entry/597446ed6fb9a06bac5bc630)</br>
[请不要滥用SharedPreference](http://weishu.me/2016/10/13/sharedpreference-advices/)</br>