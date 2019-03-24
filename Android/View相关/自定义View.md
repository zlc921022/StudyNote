# 自定义View

## View 的绘制过程

    onMeasure() 
    onLayout() 
    onDraw()

## View的绘制
    每个 View 和 ViewGroup 都会先调用 onDraw() 方法来绘制主体，再调用 dispatchDraw() 方法来绘制子 View。

## View的 Draw() 过程

    背景
    主体（onDraw()）
    子 View（dispatchDraw()）
    滑动边缘渐变和滑动条
    前景
    
``` java  
View.java 的 draw() 方法的简化版大致结构（是大致结构，不是源码哦）：
public void draw(Canvas canvas) {
    ...

    drawBackground(Canvas); // 绘制背景（不能重写）
    onDraw(Canvas); // 绘制主体
    dispatchDraw(Canvas); // 绘制子 View
    onDrawForeground(Canvas); // 绘制滑动相关和前景

    ...
}
```

## 给View添加自定义动画 自定义属性 需要设置getter和setter

## 定制Layout内部布局的方式
    
    重写 onMeasure() 来计算内部布局
    重写 onLayout() 来摆放子 View

### 重写 onMeasure() 的三个步骤
    调用每个子View的measure() 来计算子View的尺寸
    计算子View的位置并保存子View的位置和尺寸
    计算自己的尺寸并用setMeasuredDimension()保存

measure

layout


## 测量模式

     1.精确模式（MeasureSpec.EXACTLY）
    
    在这种模式下，尺寸的值是多少，那么这个组件的长或宽就是多少。
    
    2.最大模式（MeasureSpec.AT_MOST）
    
    这个也就是父组件，能够给出的最大的空间，当前组件的长或宽最大只能为这么大，当然也可以比这个小。
    
    3.未指定模式（MeasureSpec.UNSPECIFIED）
    
    这个就是说，当前组件，可以随便用空间，不受限制。

