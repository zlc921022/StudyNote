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



### Window 
        是一个抽象类,提供了绘制窗口的一组通用API
### PhoneWindow 
        是Window的具体继承实现类,而且该类内部包含了一个DecorView对象
        该DecorView对象是所有引用窗口(Activity界面)的根View
### DecorView
        是PhoneWindow的内部类,是FrameLayout的子类,是对FrameLayout进行功能性的修饰,是所有应用窗口的根View
