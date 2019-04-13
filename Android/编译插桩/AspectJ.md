# AspectJ

## 介绍

    内部通过字节码处理技术实现代码注入

## 使用方法

    1. 为方法类添加 @Aspect 注解
    2. 在对应方法中添加 @After("execution(* [类路径 ])")

``` java
@Aspect
public class ActivityAspect {

     @Before("execution(* android.app.Activity.onCreate(..))")
    public void onCreateMethodBefore(org.aspectj.lang.JoinPoint joinPoint) throws Throwable {
        System.out.println("Update onCreateMethod_Before Point = " + joinPoint.getSignature());
    }
    @After("execution(* android.app.Activity.onCreate(..))")
    public void onCreateMethodAfter(org.aspectj.lang.JoinPoint joinPoint) throws Throwable {
        System.out.println("Update onCreateMethod_After Point = " + joinPoint.getSignature());
    }
}
```

运行结果如下图 已插入成功

``` js
com.update.aspectjdemo I/System.out: Update onCreateMethod_Before Point = void com.update.aspectjdemo.MainActivity.onCreate(Bundle)
com.update.aspectjdemo I/System.out: Update onCreateMethod_Now 
Update onCreateMethod_After Point = void com.update.aspectjdemo.MainActivity.onCreate(Bundle)
```

使用逆向工具反编译代码可以插桩前后的区别

* 源代码

![image.png](https://upload-images.jianshu.io/upload_images/61189-8d6ae1ce54d5a1de.png)

* 插桩后代码

![image.png](https://upload-images.jianshu.io/upload_images/61189-6f2d8755995ceda2.png)

适用场景:

    无痕埋点,打印日志之类的都可以这样实现,
