# MeasureSpec 

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
