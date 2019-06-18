# AMS(ActivityManagerService)

    AMS主要负责和所有App的四大组件进行通信

## Activity 工作原理

### App 怎么启动

* Launcher通知AMS,要启动App,而且指定要启动App的那个界面(首页)
* AMS 通知Launcher收到请求,同时将要启动的首页记录下来
* Launcher当前页面进入 Pause 状态,然后通知AMS,进入休眠状态,可以对要App进行操作了
* AMS 检查App 是否已经启动了,
    是 唤起App 即可
    否 启动一个新的进程,AMS在新进程中创建一个ActivityThread对象,启动其中的main函数
* App启动后,通知AMS 启动完成
* AMS 查找出之前存的启动页面,通知App启动那个页面
* App启动首页,创建Context并与首页Activity关联,然后调用首页Activity的onCreate函数

    上述步骤可分2个部分 
        1~3 Launcher与AMS通信
        4~7 App与AMS通信

## 详细描述

    根Activity的启动

### 第 1 阶段 : Launcher 通知 AMS

#### 点击图标启动 App

     Flag 设置为 Intent.FLAG_ACTIVITY_NEW_TASK
        这样根Activity 就会再新的任务栈中启动

#### Activity # startActivityForResult()

``` java
@Override
public void startActivityForResult(
        String who, Intent intent, int requestCode, @Nullable Bundle options) {
    
    Instrumentation.ActivityResult ar =
        mInstrumentation.execStartActivity(
            this, mMainThread.getApplicationThread(), mToken, who,
            intent, requestCode, options);
}
```

    传递了2个重要的参数
    * 通过ActivityThread的 getApplicationThread方法获取到一个Binder对象,这个对象的类型为ApplicationThread,代表了Launcher所在的App进程
    * mToken也是一个Binder对象,代表Launcher这个Activity也通过Instrumentation传给AMS,AMS查询后就知道是谁向AMS发起了请求
    上述参数是为了以后AMS需要通知Launcher的时候,可以通过这2个参数找到Launcher

    ActivityThread 就是主线程,也就是UI线程,他在App启动时创建,代表了App应用程序,Application就是整个ActivityThread的上下文

    下为 ActivityThread 的 main函数

``` java
public static void main(String[] args) {
    Trace.traceBegin(Trace.TRACE_TAG_ACTIVITY_MANAGER, "ActivityThreadMain");
    SamplingProfilerIntegration.start();
    CloseGuard.setEnabled(false);
    Environment.initForCurrentUser();
}
```

#### Instrumentation # execStartActivity()

    因为根Activity 没有被创建出来,所以调用 Instrumentation 的 execStartActivity
    Instrumentation 主要来监控应用程序 和 系统的交互

``` java
public ActivityResult execStartActivity(
        Context who, IBinder contextThread, IBinder token, Activity target,
        Intent intent, int requestCode, Bundle options) {

    try {
        int result = ActivityManagerNative.getDefault()
            .startActivity(whoThread, who.getBasePackageName(), intent,
                    intent.resolveTypeIfNeeded(who.getContentResolver()),
                    token, target != null ? target.mEmbeddedID : null,
                    requestCode, 0, null, options);
        checkStartActivityResult(result, intent);
    } catch (RemoteException e) {
        throw new RuntimeException("Failure from system", e);
    }
    return null;
}
```

#### AMN(ActivityManagerNative) # getDefault()

    ServiceManager 是一个容器类
    AMN通过getDefault()方法,从ServiceManager中取得一个名为activity的对象,然后将其包装成一个 ActivityManagerProxy 对象(AMP)
    AMP 就是 AMS 的代理对象

#### AMP(ActivityManagerProxy) # startActivity()

### 第 2 阶段 : AMS 处理 Launcher 传来的消息

#### Binder(也就是AMN/AMP)和AMS通信

    每次做不同的事情
    这次是 Launcher 要启动 App, 发送 START_ACTIVITY 的请求给 AMS,同时告诉 AMS 要启动那个 Activity

#### AMS 处理逻辑

    收到信息并检查 App 中的 Manifest文件 中是否存在要启动的Activity
    不存在则抛出 Activity not fount 错误
    存在则进行下一步

#### AMS 通知 Launcher 收到消息

### 第 3 阶段 : Launcher 休眠并通知 AMS 
