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