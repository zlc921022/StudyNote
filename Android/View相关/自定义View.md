# 自定义View

## View 的绘制过程

    MeasureSpec

    onMeasure() 
    onLayout() 
    onDraw()

## MeasureSpec 

    MeasureSpec的值由SpecSize(测量值)和SpecMode(测量模式)共同组成.
    它是由布局参数和父容器的测量属性一起决定的
    这里测量 其实用的是位运算

    32 位 int 型的值

    SpecMode(前 2 位) + SpecSize(后 30 位)

``` java
private static final int MODE_SHIFT = 30;
  
private static final int MODE_MASK  = 0x3 << MODE_SHIFT;
    1100 0000 0000 0000 0000 0000 0000 0000

~MODE_MASK =  // ~ 对 MODE_MASK 取反
    0011 1111 1111 1111 1111 1111 1111 1111

public static final int UNSPECIFIED = 0 << MODE_SHIFT;
    0000 0000 0000 0000 0000 0000 0000 0000 
    父容器不对 View 做任何的限制,系统内部使用

public static final int EXACTLY     = 1 << MODE_SHIFT;
    0100 0000 0000 0000 0000 0000 0000 0000 
    父容器检测出 View 的大小, View的大小就是 SpecSize
    LayoutParmas 的 match_parent & 固定大小

public static final int AT_MOST     = 2 << MODE_SHIFT;
    1000 0000 0000 0000 0000 0000 0000 0000 
    父容器指定一个可用大小, View的大小不能超过这个值
    LayoutParmas 的 wrap_content
    
public static int getMode(int measureSpec) {
    // 返回前2位
    return (measureSpec & MODE_MASK);
}

public static int getSize(int measureSpec) {
    // 这里是取 后30位
    return (measureSpec & ~MODE_MASK);
}
```

    分三种类型

    EXACTLY:
        表示精确模式,一般当childView设置其宽高为精确值,match_parent (同时父容器也是这种模式) 的情况

    AT_MOST:
        表示最大值模式,一般当childView设置其宽高为wrap_content,match_parent(同时父容器也是这种模式)的情况

    UNSPECIFIED:
        表示子视图可以想要任何尺寸,一般用于系统内部,开发时很少使用

## onMeasure()

    计算内部布局

    measure() 方法被父View 调用,在measure() 中做一些准备和优化后,调用 onMeasure() 来进行实际的自我测量

    View
        View 在 onMeasure() 中会计算自己的尺寸并用 setMeasuredDimension() 方法保存

    ViewGroup
        调用每个子View的measure() 来计算子View的尺寸
        计算子View的位置并保存子View的位置和尺寸
        计算自己的尺寸并用setMeasuredDimension()保存

## onLayout()

    layout() 方法被父View 调用,在layout() 中它会保存父View传进来的自己的位置和尺寸,并调用 onLayout() 来进行实际的内部布局

    View
        由于没有子View,所以View的onLayout() 什么也不做
    
    ViewGroup
        在 onLayout() 方法中调用自己所有子View的layot() 方法,把它们的尺寸和位置传给它们,让他们完成自我布局

## onDraw()

    每个 View 和 ViewGroup 都会先调用 onDraw() 方法来绘制主体，再调用 dispatchDraw() 方法来绘制子 View。

    View
        View绘制自身(含背景,内容)
        绘制装饰(滚动指示器,滚动条,和前景)

    ViewGroup

        背景
        主体（onDraw()）
        子 View（dispatchDraw()）
        滑动边缘渐变和滑动条 和 前景
 
``` java  
View.java 的 draw() 方法的简化版大致结构(是大致结构，不是源码哦)
public void draw(Canvas canvas) {
    ...

    drawBackground(Canvas); // 绘制背景（不能重写）
    onDraw(Canvas); // 绘制主体
    dispatchDraw(Canvas); // 绘制子 View
    onDrawForeground(Canvas); // 绘制滑动相关和前景
    ...
}
```
