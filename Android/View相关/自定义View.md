# 自定义View

## View 的绘制过程

    MeasureSpec

    onMeasure() 
    onLayout() 
    onDraw()

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
