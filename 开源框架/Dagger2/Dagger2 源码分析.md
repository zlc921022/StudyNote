# Dagger2 源码分析

1. 使用

Android Studio3.0使用

1.Dagger2的引入 
    
    网上找到的是这样的 需要修改2个文件
    
    1.1.1 首先是project的build.gradle
        dependencies {  
            classpath 'com.android.tools.build:gradle:2.1.0'  
            classpath 'com.neenbedankt.gradle.plugins:android-apt:1.8'  
        }  
    
    1.1.2 然后是module的build.gradle文件
    
        apply plugin: 'com.neenbedankt.android-apt'  
            
        compile 'com.google.dagger:dagger:2.6'  
        apt 'com.google.dagger:dagger-compiler:2.6'
    
    ---------------------------------
    
    ps :我按照上面的配置出现了这个错误

        Warning:android-apt plugin is incompatible with future version of Android Gradle plugin. Please use 'annotationProcessor' configuration instead.
	
    这样的方式在Studio2.+的时候是可以的,3.0已经不兼容了 需要按照下面的方式来配置
    
    ---------------------------------
    
    1.2.1 Studio3.0以上版本只需要修改这一个地方就可以
        compile 'com.google.dagger:dagger:2.6'
        annotationProcessor 'com.google.dagger:dagger-compiler:2.6'
        
以上配置完毕就可以开始使用dagger2来进行开发了


@Module
@Commponent{modules = }
@Inject
@ContributesAndroidInjector
@Provides


@Scope
作用域
@Singleton App 中只有一个实例
@ActivityScope Activity中只有一个实例

@Inject
要求在构造函数上使用

@Inject 注解的两个功能基本上已经明确了，一是 为了提供被注入的对象，二是提供注入到当前类的对象。

@Provides 注解的功能就是标注提供被注入的对象的方法，是为了弥补@Inject 注解出现的

https://www.jianshu.com/p/2f3b3de1ce08

https://blog.csdn.net/user11223344abc/article/details/83450237

https://blog.csdn.net/qq_17766199/article/details/73030696

### 遇到的小问题

    在base 库中使用dagger 要在 上层库上使用 
    需要在上层库中添加

    annotationProcessor 'com.google.dagger:dagger-compiler:2.16'

    不然在上层库中不会生成对应的 Component 类