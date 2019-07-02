# Activity

## 提出问题


## Activity 生命周期

|Activity 生命周期|时机|
|---|---|
|onCreate(Bundle savedInstanceState)|创建|
|( onRestart() )|onStop之后|
|onStart()|可见|
|onResume()|获取到焦点|
|onPause()|失去焦点|
|onStop()|不可见|
|onDestroy() |销毁|
|...|...|
|onNewIntent()|||

### onActivityResult

    为什么不设置为回调

    可以通过一个 BridgeActivity 或者 BridgeFragment 做跳板来接收消息,然后设置为回调

    推荐用 Fragment 更加的轻量
