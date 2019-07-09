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

