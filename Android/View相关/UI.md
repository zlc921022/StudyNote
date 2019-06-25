# UI

## setContentView()

    PhoneWindow # setContentView()
        installDecor()
            generateDecor()
            generateLayout()

View 如何被添加到屏幕窗口上

    * 创建顶级布局容器 DecorView
    * 在顶层布局中加载基础布局ViewGroup (根据style的不同来确定加载那个)
    * 将ContentView 添加到基础布局中的 FrameLayout 中



