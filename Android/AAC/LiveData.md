# LiveData

    - LiveData 的实现基于观察者模式
    - LiveData 跟 LifecycleOwner 绑定, 能感知生命周期变化, 并且只会在 LifecycleOwner 处于 Active 状态（STARTED/RESUMED）下通知数据改变
    - LiveData 会自动在 DESTROYED 的状态下移除 Observer , 取消订阅, 所以不用担心内存泄露


##基本使用

``` java
MutableLiveData<String> liveString = new MutableLiveData<>();
liveString.observe(this, new Observer<String>() {
    @Override
    public void onChanged(@Nullable String s) {
        Log.e(TAG, "onChanged " + s);
    }
});

liveString.postValue("post hello");
liveString.setValue("set world");
```
[4. Jetpack源码解析—LiveData的使用及工作原理](https://juejin.im/post/5d247b036fb9a07eee5ef3df)</br>
[【AAC 系列三】深入理解架构组件：LiveData](https://juejin.im/post/5ce54c2be51d45106343179d)</br>
[重学安卓：RxJava 才不是 LiveData 的对手！](https://juejin.im/post/5d6763dc5188257a5f39635b)</br>