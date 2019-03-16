# Fragment 

## 提出问题

    Fragment 生命周期
    getActivity()空指针
    异常：Can not perform this action after onSaveInstanceState
    Fragment重叠异常-----正确使用hide、show的姿势
    Fragment嵌套的那些坑
    未必靠谱的出栈方法remove()
    多个Fragment同时出栈的深坑BUG
    深坑 Fragment转场动画

## Fragment 生命周期

    setUserVisibleHint()：设置Fragment可见或者不可见时会调用此方法。在该方法里面可以通过调用getUserVisibleHint()获得Fragment的状态是可见还是不可见的，如果可见则进行懒加载操作。

|Activity 生命周期|Fragment 生命周期|时机|
|---|---|---|
|onCreate|onAttach(Activity activity)|Fragment与Activity绑定|
||onCreate|初始化Fragment。可通过参数savedInstanceState获取之前保存的值。|
||onCreateView||
||onViewCreated||
|onStart|onStart|可见|
|onResume|onResume|获取到焦点|
|onPause|onPause|失去焦点|
|onStop|onStop|不可见|
|onDestroy|onDestroyView|销毁|
||onDestroy||
||onDetach|Fragment与Activity解除绑定|


https://www.jianshu.com/p/d9143a92ad94