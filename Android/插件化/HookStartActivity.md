# HookStartActivity

## Activity启动步骤

### 第1步 Activity1 通知 AMS, 要启动 Activity2

    Launcher/Activity # startActivity()
    Launcher/Activity # startActivityForResult()
    Instrumentation # execStartActivity()
    AMN/AMP # startActivity()

    AMS

#### 可以Hook的点

    Activity 的 startActivityForResult 方法
    Activity 的 mInstrumentation 字段 
    AMN 的 getDefault方法 获取到的对象

### 第2步 AMS 通知 App进程, 要启动 Activity2

    ApplicationThreadProxy # scheduleLaunchActivity()

    ApplicationThread # scheduleLaunchActivity()
    ActivityThread # sendMessage()
    H # handleMessage()
    ActivityThread # handleLaunchActivity()
    ActivityThread # callAcvitityOnCreate()
    Instrumentation # onCreate()

#### 可以Hook的点

    H 的 mCallback 字段
    ActivityThread 的 mInstrumentation 字段,对应的 newActivity 方法和 callAcvitityOnCreate 方法

## ActivityThread

    28之前 Activity的生命周期 都是通过 H 来做事件分发
    
    28 之后 都放到下面方法中了
        handleRelaunchActivity()
        通过 各种判断来进行各种状态方法的调用
        performLaunchActivity()
        handleLaunchActivity()
        performDestroyActivity

        
       