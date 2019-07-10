# Kotlin

## main函数

    用kt编写静态main方法

    1. 写在类内

``` java
class Test {
    companion object {
        @JvmStatic
        fun main(args: Array<String>) {
            println("HelloWorld")
        }
    }
}
```

    2. 写在类外

``` java
class Test {
}

fun main(args: Array<String>) {
    println("HelloWorld")
}
```

## 定义函数

### 有返回值

``` java
fun sum(a: Int, b: Int): Int {
    return a + b
}
```

    将表达式作为函数体,返回值类型自动推断的函数

``` java
fun sum1(a: Int, b: Int) = a + b
```

### 有 无意义 返回值

``` java
fun printSum(a: Int, b: Int):Unit{
    println("sum of $a and $b is ${a+b}")
}
```

### Unit 返回类型可以省略

``` java
fun printSum(a: Int, b: Int){
    println("sum of $a and $b is ${a+b}")
}
```

## 定义变量

``` java
val a:Int = 1 // 立即赋值
val b = 2 // 自动推断出 "Int" 类型
val c:Int // 如果没有初始 值类型不能省略
c = 3
```

### TIPS:

    var:
        可变变量
        这是一个可以通过重新分配来更改为另一个值的变量
        这种声明变量的方式和Java中声明变量的方式一样。
    val:
        只读变量
        这种声明变量的方式相当于java中的final变量
        一个val创建的时候必须初始化，因为以后不能被改变。

## 字符串模板

    $a ${a+b}

## 条件表达式

``` java
fun maxOf(a: Int, b: Int): Int {
    if (a > b) {
        return a
    } else {
        return b
    }
}

fun maxOf(a: Int, b: Int): Int {
    return if (a > b) a else b
}

fun maxOf(a: Int, b: Int) = if (a > b) a else b
```

## 使用可空值  以及 null 检测

    当某个变量的值可以为 null 的时候,必须在声明处的类型后添加 ? 来标识该引用可为 null

## 类型检测 自动类型转换

    is 

    a is String 

    类似 Java 的instanceof 和 强转

## for 循环

``` java
val items = listOf("apple", "banana")
for (item in items)
    println(item)

val items = listOf("apple", "banana")
for (index in items.indices)
    println(items[index])
```

## while 循环

``` java
var index = 0;
while (index < 5) {
    println("hello index = $index")
    index++
}
```

## Break Continue 标签
    跟java类似 能跳出最直接包围的那层循环
    配合 label 例如 abc@ loop@ 使用,可以动态控制需要跳出的层数

```java
var length = 10
abc@for(i in 1..length){
    println("first i = $i")
    for(j in 1..length){
        println("second j = $j")
        for(k in 1..length){
            if (k == 2){
                break@abc
            }
            println("last k = $k")
        }
    }
}
```

## when 表达式

``` java
var obj: Any = 2 // 若使用不同类型做判断 需要加 Any
when (obj) {
    1 -> println("hell0")
    "world" -> println("world")
}
```

## 使用区间(range)

使用 in 运算符 来检测某个数字是否在指定区间内

``` java
val x = 9
if (x in 1..10) {
    println("fits in range")
} else {
    println("not fits in range")
}
```

``` java
for (i in 1 .. 10) {} // 闭区间 包含10 1~10
for (i in 1 until 10) {} // 开区间 不包含10
for (i in 2 .. 10 step 2) {} // 闭区间 step 是步长
for (i in 10 downTo 1) {} // 闭区间 包含10 10~1
if (x in 1..10) {} // 判断 x 是否在1~10区间内
```

## 使用集合

``` java
val items = listOf("apple", "banana")
for (item in items)
    println(item)
```

## ???

### if not null

``` java
val files = File("Test").listFiles()
println(files?.size())
```

### if null 执行一个语句

``` java
val values = ...
val email = values["email] ? println("empty")
```

## TIPS

    1. == 和 ===
        == 比较的是数值
        === 比较的是地址

        kt 也会对基本类型进行装箱
        比如Int
        -128 - 127 直接赋值 地址是一样的
        如果不在这个区间内 会重新创建对象 所以地址不一样

## 类与继承

    kt 默认生成的类是final 若需要继承 需要添加 open 关键字

    方法重写也一样 如果该方法支持重写 也需要添加 open 关键字

    标记为 override 的成员是开放的,支持在子类中重写

    可以用 var修饰 覆盖一个 val覆盖 的属性

## 扩展

### 扩展函数

``` java
fun Context.getTest(){
    Log.e("Update","Hello Context")
}
```

需要写到顶层函数(类外) 任何类都行 最好是Utils类,方便管理

扩展函数不会替换类中存在的方法 可以加问号去覆盖null的方法

``` java
class Test {

    companion object {
        @JvmStatic
        fun main(args: Array<String>) {
            var test = Test()
            println(test.hello())
            println(test.world())
        }
    }

    fun world(): String {
        return "World Test"
    }

}

fun Any?.hello(): String {
    return "Hello Any"
}

fun Any?.world(): String {
    return "World Any"
}

输出结果:
    Hello Any
    Hello Test
```

## 嵌套类

    嵌套类无法访问外部类的属性

``` java
class Outer {
    private val bar: Int = 1
    class Nested {
        fun foo() = 2
    }
}

val demo = Outer.Nested().foo() // == 2
```

## 内部类

    内部类可以访问外部类的属性

``` java
class Outer {
    private val bar: Int = 1
    inner class Inner {
        fun foo() = bar
    }
}

val demo = Outer().Inner().foo() // == 1
```

## 匿名内部类

``` java
interface ITest {
    fun say()
}

var test = object :ITest{
    override fun say() {
        println("Hello ")
    }
}
test.say()
```

### 使用lambda表达式

    如果对象是 函数式 Java 接口（即具有单个抽象方法的 Java 接口）的实例,可以使用 lambda表达式

``` java
// kotlin 接口
interface ITest {
    fun say()
}

// Java 接口
public interface IListener {
    void wahaha();
}

// 报错 提示 interface-does-not-have-constructors
var test = ITest { println("hello") }

// 正常执行
var listener = IListener { println("wahahhah") }
```

    结论就是 这种方式只支持Java,不支持kotlin 


## 对象声明

https://blog.csdn.net/xlh1191860939/article/details/79460601