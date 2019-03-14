# Fragment 

## 提出问题



## Fragment 生命周期

    setUserVisibleHint()：设置Fragment可见或者不可见时会调用此方法。在该方法里面可以通过调用getUserVisibleHint()获得Fragment的状态是可见还是不可见的，如果可见则进行懒加载操作。

|Activity 生命周期|Fragment 生命周期|时机|
|---|---|---|
|onCreate|onAttach(Activity activity)|Fragment与Activity已经完成绑定|
||onCreate|初始化Fragment。可通过参数savedInstanceState获取之前保存的值。|
||onCreateView||
||onViewCreated||
|onStart|onStart|可见|
|onResume|onResume|获取到焦点|
|onPause|onPause|失去焦点|
|onStop|onStop|不可见|
|onDestroy|onDestroyView|销毁|
||onDestroy||
||onDetach||