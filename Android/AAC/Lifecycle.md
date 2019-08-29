# Lifecycle

    它能够帮助我们方便的管理 Activity 以及 Fragment 的生命周期。

## Lifecycle & LifecycleRegistry

``` java
public abstract class Lifecycle {

    //注册LifecycleObserver （比如Presenter）
    public abstract void addObserver(LifecycleObserver observer);
    //移除LifecycleObserver 
    public abstract void  removeObserver(LifecycleObserver observer);
    //获取当前状态
    public abstract State getCurrentState();

    public enum Event {
        ON_CREATE,
        ON_START,
        ON_RESUME,
        ON_PAUSE,
        ON_STOP,
        ON_DESTROY,
        ON_ANY
    }
    
    public enum State {
        DESTROYED,
        INITIALIZED,
        CREATED,
        STARTED,
        RESUMED;

        public boolean isAtLeast(State state) {
            return compareTo(state) >= 0;
        }
    }
}
```


    Lifecycle中就是声明了一些抽象方法和两个状态的枚举类, 具体的实现看LifecycleRegistry

    在 SupportActivity(28是ComponentActivity)中调用 ReportFragment # InjectIfNeededIn(), 在ReportFragment 中监听 Activity 的声明周期,然后在对应的方法中对 Lifecycle 事件进行分发


[【AAC 系列二】深入理解架构组件的基石：Lifecycle](https://juejin.im/post/5cd81634e51d453af7192b87)</br>
[3. Jetpack源码解析---用Lifecycles管理生命周期](https://juejin.im/post/5d15bbb86fb9a07f03574e56)</br>
