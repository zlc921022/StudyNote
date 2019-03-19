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

    
    

