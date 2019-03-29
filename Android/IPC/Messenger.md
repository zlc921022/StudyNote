# Messenger

## 介绍

    Messenger 是一种 轻量级的 IPC 方式,对 AIDL 进行了封装
    Messenger 可以在不同进程中传递对象,在 Messenger 中放入需要传递的对象,就能轻松的实现数据的进程间传递了

## 使用

### Server

``` java
public class ServerService extends Service {

    public static final String KEY_MESSAGE = "message";
    public static final int  KEY_CONN = 1;
    public static final int  KEY_DISCONN = 2;

    Messenger messenger = null;

    private static class MessengerHandler extends Handler {
        @Override
        public void handleMessage(Message msg) {
            super.handleMessage(msg);
            int what = msg.what;
            switch (what) {
                case KEY_CONN:
                    L.e("i received " + msg.getData().getString(KEY_MESSAGE));
                    Messenger client = msg.replyTo;

                    if (client != null) {
                        String data = "i have received your message";
                        Message reply = Message.obtain();
                        Bundle bundle = new Bundle();
                        bundle.putString(KEY_MESSAGE, data);
                        L.e(data);
                        reply.setData(bundle);

                        try {
                            client.send(reply);
                        } catch (RemoteException e) {
                            e.printStackTrace();
                        }
                    }
                    break;
                case KEY_DISCONN:
                    L.e("i received " + msg.getData().getString(KEY_MESSAGE));
                    L.e("client has disconn this conn");
                    break;
                default:
            }
        }
    }

    @Nullable
    @Override
    public IBinder onBind(Intent intent) {
        return messenger.getBinder();
    }

    @Override
    public void onCreate() {
        super.onCreate();
        messenger = new Messenger(new MessengerHandler());
    }
}
```

### Client

``` java
public class MainActivity extends AppCompatActivity implements View.OnClickListener {

    private ServiceConnection serviceConnection;
    private Messenger serverMessenger;
    private Messenger messenger;

    private boolean hasBindService = false;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        initView();
        initData();

    }

    private void initView() {
        findViewById(R.id.conn_handler).setOnClickListener(this);

    }

    private void initData() {
        serviceConnection = new ServiceConnection() {
            @Override
            public void onServiceConnected(ComponentName name, IBinder service) {
                serverMessenger = new Messenger(service);
                communicate();
            }

            @Override
            public void onServiceDisconnected(ComponentName name) {
                serverMessenger = null;
            }
        };
        messenger = new Messenger(new Handler() {
            @Override
            public void handleMessage(Message msg) {
                super.handleMessage(msg);
                L.e("i have received " + msg.getData().getString(ServerService.KEY_MESSAGE));
                Message message = Message.obtain();
                Bundle bundle = new Bundle();
                bundle.putString(ServerService.KEY_MESSAGE, "Ok bye");
                message.setData(bundle);
                L.e("i have send " + message.getData().getString(ServerService.KEY_MESSAGE));

                message.what = ServerService.KEY_DISCONN;

                if (null != serverMessenger) {
                    try {
                        serverMessenger.send(message);
                    } catch (RemoteException e) {
                        e.printStackTrace();
                    }
                }
            }
        });
    }

    private void communicate() {
        String format = "yyyy-MM-dd-HH:mm:ss";
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat(format);
        String currTime = simpleDateFormat.format(System.currentTimeMillis());
        Message message = Message.obtain();
        Bundle bundle = new Bundle();
        bundle.putString(ServerService.KEY_MESSAGE, "i have send handler a message at " + currTime);
        message.setData(bundle);
        L.e("i have send " + message.getData().getString(ServerService.KEY_MESSAGE));
        message.what = ServerService.KEY_CONN;
        message.replyTo = messenger;
        if (null != serverMessenger) {
            try {
                serverMessenger.send(message);
            } catch (RemoteException e) {
                e.printStackTrace();
            }
        }
    }

    @Override
    public void onClick(View v) {
        switch (v.getId()) {
            case R.id.conn_handler:
                if (!hasBindService) {
                    Intent intent = new Intent();
                    String packageName = "com.update.messengerdemo";
                    String className = packageName + ".ServerService";
                    intent.setClassName(packageName, className);
                    bindService(intent, serviceConnection, BIND_AUTO_CREATE);
                    hasBindService = true;
                } else {
                    if (null != serverMessenger) {
                        return;
                    }
                    communicate();
                }
                break;
            default:
        }
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        if (null != serverMessenger) {
            unbindService(serviceConnection);
        }
    }
}
```

