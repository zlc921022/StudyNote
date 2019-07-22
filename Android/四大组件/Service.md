# Service

    bindService 模式
        第一次 bindService()的时候，执行的方法为 onCreate()、onBind()解除绑定的时候会执行 onUnbind()、onDestory()。

    startService 模式
        当第一次调用 startService 的时候执行的方法依次为 onCreate()、onStartCommand()，(onStart()) 当 Service 关闭的时候调用 onDestory 方法。

![](https://upload-images.jianshu.io/upload_images/61189-3d5c0d91270c6953.png)

``` java

@Override
public void onCreate() {
    super.onCreate();
    // 只在 Service 创建的时候调用一次
}

@Override
public int onStartCommand(Intent intent, int flags, int startId) {
    // 其他组件 调用 startService 的时候会被调用 (每次都会调用)
    // 先调用 onStartCommand 后 onStart
    return super.onStartCommand(intent, flags, startId);
}

@Override
public void onStart(Intent intent, int startId) {
    super.onStart(intent, startId);
    // 其他组件 调用 startService 的时候会被调用 (每次都会调用)
    // 先调用 onStartCommand 后 onStart
}

@Override
public IBinder onBind(Intent intent) {
    // 其他组件 调用 bindService 的时候会被调用 (只调用一次)
    return null;
}

@Override
public boolean onUnbind(Intent intent) {
    // 其他组件 调用 unbindService 的时候会被调用 (只调用一次)
    return super.onUnbind(intent);
}
```

## bind 和 start 交叉调用

    Service 的 onCreate 只会调用一次

    bindService
        onBind 也是只会调用一次

    startService
        每调用一次 startService 
            onStartCommand & onStart 都会先后调用

## 如何结束

    stopService()

        不推荐使用stopService来关闭服务，因为它就像强制结束进程一样是种不负责任的做法，这种时候服务往往不能正常的退出。

        startService 启动 可以结束
        bindService 启动 无法结束

    stopSelf()
        Android推荐使用 Service 的 stopSelf 这个方法来关闭服务
        也就是服务自己关闭自己,这样就能保证服务里所有的流程和状态在关闭时得到处理和保存
        可以通过广播 EventBus 和 RxBus 等来做消息的传递
        
        startService 启动 可以结束
        bindService 启动 无法结束

    unbindService()

        通过 bindService 启动的 需要使用这个来结束

## bindService

``` java
public class MainActivity extends AppCompatActivity implements View.OnClickListener {
    private static final String TAG = "Update TestService";

    Activity activity;
    ServiceConnection mConnection;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        activity = this;
        setContentView(R.layout.activity_main);
        mConnection = new ServiceConnection() {
            @Override
            public void onServiceConnected(ComponentName name, IBinder service) {
                TestService.TestBinder  testBinder = (TestService.TestBinder) service;
                TestService testService = testBinder.getService();
                log("onServiceConnected " +testService.getName());
            }

            @Override
            public void onServiceDisconnected(ComponentName name) {
                log("onServiceDisconnected");
            }
        };
        findViewById(R.id.btn1).setOnClickListener(this);
        findViewById(R.id.btn2).setOnClickListener(this);
    }

    @Override
    public void onClick(View v) {
        Intent intent = new Intent(activity, TestService.class);
        switch (v.getId()) {
            case R.id.btn1:
                bindService(intent, mConnection, Context.BIND_AUTO_CREATE);
                break;
            case R.id.btn2:
                unbindService(mConnection);
                break;
        }
    }

    private void log(String msg) {
        Log.e(TAG, msg);
    }
}

public class TestService extends Service {
    private static final String TAG = "Update TestService";

    private final IBinder mBinder = new TestBinder();

    @Override
    public void onCreate() {
        super.onCreate();
        log("onCreate");
    }

    @Override
    public void onStart(Intent intent, int startId) {
        super.onStart(intent, startId);
        log("onStart");
    }

    @Override
    public int onStartCommand(Intent intent, int flags, int startId) {
        log("onStartCommand");
        return super.onStartCommand(intent, flags, startId);
    }

    @Override
    public IBinder onBind(Intent intent) {
        log("onBind");
        return mBinder;
    }

    @Override
    public boolean onUnbind(Intent intent) {
        log("onUnbind");
        return super.onUnbind(intent);
    }

    @Override
    public void onDestroy() {
        log("onDestroy");
        super.onDestroy();
    }

    private void log(String msg) {
        Log.e(TAG, msg);
    }

    public String getName(){
        return "Helloya";
    }

    class TestBinder extends Binder {
        TestService getService(){
            return TestService.this;
        }
    }
}
```