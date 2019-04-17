# ActivityLifecycleCallbacks

    API 14之后，在Application类中，提供了一个应用生命周期回调的注册方法，用来对应用的生命周期进行集中管理，这个接口叫registerActivityLifecycleCallbacks，可以通过它注册自己的ActivityLifeCycleCallback，每一个Activity的生命周期都会回调到这里的对应方法。之前我们想做类似限制制定Activity个数的时候都要自己去添加和计数，有了ActivityLifeCycleCallback接口，所有Activity的生命周期都会在这里回调，我们可以根据条件随心处理。

## 用法

    可以做类似AOP的操作,比如埋点,打印日志
    对 Activity 添加 TitleView 之类的操作


