# AMS(ActivityManagerService)

    AMS主要负责和所有App的四大组件进行通信

## Activity 工作原理

### App 怎么启动

* Launcher通知AMS,要启动斗鱼App,而且指定要启动斗鱼App的那个界面(首页)
* AMS 通知Launcher收到请求,同时将要启动的首页记录下来
* Launcher当前页面进入 Pause 状态,然后通知AMS,进入休眠状态,可以对要斗鱼App进行操作了
* AMS 检查斗鱼App 是否已经启动了,
    是 唤起斗鱼App 即可
    否 启动一个新的进程,AMS在新进程中创建一个ActivityThread对象,启动其中的main函数
* 斗鱼App启动后,通知AMS 启动完成
* AMS 查找出之前存的启动页面,通知斗鱼App启动那个页面
* 斗鱼App启动首页,创建Context并与首页Activity关联,然后调用首页Activity的onCreate函数

    上述步骤可分2个部分 
        1~3 Launcher与AMS通信
        4~7 斗鱼App与AMS通信

#### Launcher通知AMS

    ActivityThread 就是主线程,也就是UI线程,他在App启动时创建,代表了App应用程序,Application就是整个ActivityThread的上下文

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

        上述代码传递了2个重要的参数
        * 通过ActivityThread的 getApplicationThread方法获取到一个Binder对象,这个对象的类型为ApplicationThread,代表了Launcher所在的App进程
        * mToken也是一个Binder对象,代表Launcher这个Activity也通过Instrumentation传给AMS,AMS查询后就知道是谁向AMS发起了请求
        上述参数是为了以后AMS需要通知Launcher的时候,可以通过这2个参数找到Launcher


    下为 ActivityThread 的 main函数

``` java
 public static void main(String[] args) {
        Trace.traceBegin(Trace.TRACE_TAG_ACTIVITY_MANAGER, "ActivityThreadMain");
        SamplingProfilerIntegration.start();
        CloseGuard.setEnabled(false);
        Environment.initForCurrentUser();
    }
```

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
