# UI优化

## UI绘制原理

    CPU 负责计算显示内容
    GPU 负责栅格化(UI元素绘制到屏幕上)

## 优化工具


## 部分概念

|名称|介绍|转换关系|
|---|---|---|
|px|像素|px = dp * density|
|dpi|像素密度,系统软件指定的单位尺寸的像素数量||
|dp||px = dp * (dpi / 160))|
|density|密度|density = dpi / 160|

## 使用Hierarchy Viewer

    单独版本的 hieararchyviewer 已经被弃用了
    由 Android Device Monitor 来代替
    Android Device Monitor在tools目录下面找到monitor.bat即可

### include

``` xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!" />

    <include
        layout="@layout/layout_test"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content" />
</LinearLayout>

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/ll_container"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="啊哈哈哈" />

</LinearLayout>
```

![image.png](https://upload-images.jianshu.io/upload_images/61189-5b9e9229e908f0d2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

    嵌套 5 层
    注意 使用的是  layout="@layout/layout_test" 没Android前缀

### merge 标签

``` xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!" />

    <include
        layout="@layout/layout_test"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content" />
</LinearLayout>

<merge xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/ll_container"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="啊哈哈哈" />

</merge>
```

![image.png](https://upload-images.jianshu.io/upload_images/61189-59a8594feef0afcc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

    嵌套 4 层

    merge标签常用于减少布局嵌套层次，但是只能用于根布局。

### ViewStub

``` xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <Button
        android:id="@+id/btn"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!" />

    <ViewStub
        android:id="@+id/vs"
        android:layout="@layout/layout_test"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content" />
</LinearLayout>

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/ll_container"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="啊哈哈哈" />

</LinearLayout>
```

#### 遇到的问题

    直接改报错 ViewStub must have a valid layoutResource
       需要使用  android:layout="@layout/layout_test" 有安卓后缀