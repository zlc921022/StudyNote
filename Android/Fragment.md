# Fragment 

## 提出问题



## Fragment 生命周期

|Activity 生命周期|Fragment 生命周期|时机|
|---|---|---|
|onCreate|onAttach|创建|
||onCreate||
||onCreateView||
||onViewCreated||
|onStart|onStart|可见|
|onResume|onResume|获取到焦点|
|onPause|onPause|失去焦点|
|onStop|onStop|不可见|
|onDestroy|onDestroyView|销毁|
||onDestroyView||
||onDetach||