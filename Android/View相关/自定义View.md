# 自定义View

## View 的绘制过程

    MeasureSpec

    onMeasure() 
    onLayout() 
    onDraw()

## MeasureSpec

    MeasureSpec的值由SpecSize(测量值)和SpecMode(测量模式)共同组成.
    它是由布局参数和父容器的测量属性一起决定的

    分三种类型

    EXACTLY:
        表示精确模式,一般当childView设置其宽高为精确值,match_parent (同时父容器也是这种模式) 的情况

    AT_MOST:
        表示最大值模式,一般当childView设置其宽高为wrap_content,match_parent(同时父容器也是这种模式)的情况

    UNSPECIFIED:
        表示子视图可以想要任何尺寸,一般用于系统内部,开发时很少使用

## onMeasure()

### 定制Layout内部布局的方式
    
    重写 onMeasure() 来计算内部布局
    重写 onLayout() 来摆放子 View

### 重写 onMeasure() 的三个步骤
    调用每个子View的measure() 来计算子View的尺寸
    计算子View的位置并保存子View的位置和尺寸
    计算自己的尺寸并用setMeasuredDimension()保存

## onLayout()

## onDraw()

    每个 View 和 ViewGroup 都会先调用 onDraw() 方法来绘制主体，再调用 dispatchDraw() 方法来绘制子 View。

### View的 Draw() 过程

    背景
    主体（onDraw()）
    子 View（dispatchDraw()）
    滑动边缘渐变和滑动条
    前景
    
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




