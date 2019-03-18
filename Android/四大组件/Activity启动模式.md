# Activity 启动模式

## Standard 标准模式

    每次启动一个Activity都会又一次创建一个新的实例入栈,无论这个实例是否存在.

### 生命周期

    每次被创建的实例Activity 的生命周期符合典型情况,它的onCreate,onStart,onResume都会被调用.

## SingleTop 栈顶复用模式

    分两种处理情况: 
    须要创建的Activity已经处于栈顶时,此时会直接复用栈顶的Activity.不会再创建新的Activity;
    若须要创建的Activity不处于栈顶,此时会又一次创建一个新的Activity入栈,同Standard模式一样.

### 生命周期

    若情况一中栈顶的Activity被直接复用时,它的onCreate,onStart不会被系统调用,由于它并没有发生改变. 可是一个新的方法 onNewIntent会被回调(Activity被正常创建时不会回调此方法).

## SingleTask 栈内复用模式

    若须要创建的Activity已经处于栈中时,此时不会创建新的Activity,而是将存在栈中的Activity上面的其他Activity所有销毁,使它成为栈顶。

### 生命周期

    同 SingleTop 模式中的情况一同样.仅仅会又一次回调Activity中的 onNewIntent方法

## SingleInstance 单实例模式

    SingleInstance比較特殊,是全局单例模式,是一种加强的SingleTask模式.它除了具有它所有特性外，还加强了一点：具有此模式的Activity仅仅能单独位于一个任务栈中.

    这个经常使用于系统中的应用，比如Launch,锁屏键的应用等等.整个系统中仅仅有一个!所以在我们的应用中一般不会用到.了解即可.


https://www.cnblogs.com/claireyuancy/p/7387696.html