# Gradle插件编写

## 插件编写

### 创建一个类 实现  Plugin接口即可

``` groovy
class AopPlugin implements Plugin<Project> {
    @Override
    void apply(Project project) {
    }
}    
```

#### 判断是否为App

``` groovy
def isApp = project.plugins.hasPlugin(AppPlugin)
```

### 使用自定义的配置参数

#### 创建自定义 DataBean

``` groovy
class AopExtension {
    public static final String EXT_NAME = 'pluginAop'
    String aopClass = ''
}

class AopPlugin implements Plugin<Project> {
    @Override
    void apply(Project project) {
        project.extensions.create(AopExtension.EXT_NAME, AopExtension)
        project.afterEvaluate {
            init(project) 
        }
    }

    static void init(Project project) {
        // 读取自定义的配置参数 并赋值给定义的 DataBean
        AopExtension extension = project.extensions.findByName(AopExtension.EXT_NAME) as AopExtension
        project.logger.error extension.aopClass
    }
}
```

#### 使用

    在主module(app)的 build.gradle文件中添加以下配置
``` groovy 
pluginAop {
    aopClass = 'com.helloya'
}
```

#### 输出
``` java
com.helloya
```

### 监听Task构建开始和结束

#### 需要实现 TaskExecutionListener 接口

### 接口描述 

    用来监控 Task 的Listener

[TaskExecutionListener](https://docs.gradle.org/current/javadoc/org/gradle/api/execution/TaskExecutionListener.html)</br>

### 代码实现

``` groovy 
class TaskListener implements TaskExecutionListener {

    long startTime

    /**
     * 在 task执行前 调用
     */
    @Override
    void beforeExecute(Task task) {
        startTime = System.currentTimeMillis()
    }

    /**
     * 在 task执行后 调用
     */
    @Override
    void afterExecute(Task task, TaskState taskState) {
        long ms = System.currentTimeMillis() - startTime
        println "${task.path} spend ${ms}ms"
    }
}
```

#### 需要实现 BuildListener 接口

### 接口描述 

    用来监控构建的Listener

[BuildListener](https://docs.gradle.org/current/javadoc/org/gradle/BuildListener.html)</br>

### 内部实现

``` groovy 
public interface BuildListener {
    void buildStarted(Gradle var1);

    void settingsEvaluated(Settings var1);

    void projectsLoaded(Gradle var1);

    void projectsEvaluated(Gradle var1);

    void buildFinished(BuildResult var1);
}
```


## 遇到的奇奇怪怪的bug

### Unable to load Class

![image.png](https://upload-images.jianshu.io/upload_images/61189-6c18826b13b11a65.png)

![image.png](https://upload-images.jianshu.io/upload_images/61189-a6989c320df04b94.png)

如上图,检查了好久,发现是 package 信息没写
因为实用 file 形式创建 groovy 文件不会自动补全 package信息的,所以建议先写package 信息在进行逻辑编写

### 提示 Gradle 版本过低

![image.png](https://upload-images.jianshu.io/upload_images/61189-72f021adafbc2f2b.png)

![image.png](https://upload-images.jianshu.io/upload_images/61189-cf053f622899d3a6.png)

我一直用的 3.3.2的,生成插件的时候习惯性的升级到3.4.0了,将版本降下来就好了 
或者也可以选择升级你的gradle 版本

### 上传 Maven 提示 出错
    
    具体信息:
        com.novoda.gradle.release.AndroidLibrary$LibraryUsage.getGlobalExcludes()Ljava/util/Set

![image.png](https://upload-images.jianshu.io/upload_images/61189-dfe0cd2221f5fd94.png)

![image.png](https://upload-images.jianshu.io/upload_images/61189-4e899ec4553ac41a.png)

    去github 查看后发现是 该版本只支持 Gradle 的4.4版本,修改后就可以了

![image.png](https://upload-images.jianshu.io/upload_images/61189-f46267d91f8c6e67.png)

[Gradle for Android 第一篇( 从 Gradle 和 AS 开始 )](https://segmentfault.com/a/1190000004229002)</br>
[拥抱 Android Studio 之四：Maven 仓库使用与私有仓库搭建](http://kvh.io/cn/embrace-android-studio-maven-deploy.html)</br>
## Gradle 插件的编写
[手把手图文并茂教你发布Android开源库](https://blog.csdn.net/hejjunlin/article/details/52452220)</br>
[【Android】函数插桩（Gradle + ASM）](https://www.jianshu.com/p/16ed4d233fd1)</br>
[在AndroidStudio中自定义Gradle插件](https://blog.csdn.net/huachao1001/article/details/51810328)</br>
[构建神器Gradle](http://jiajixin.cn/2015/08/07/gradle-android/)</br>
[鸿洋推荐 Gradle学习资料](https://mp.weixin.qq.com/s?__biz=MzAxMTI4MTkwNQ==&mid=2650828850&idx=1&sn=b5be1ab7fb2fc85fcee1bf490be52446&chksm=80b7a4acb7c02dbad6735def8eb36fefd306eb0b4ffd2e9ac0bd94b292476c0d258962024b27&mpshare=1&scene=23&srcid=&sharer_sharetime=1568280695544&sharer_shareid=fbe42eb3d0b49110b240c829132445bf#rd)</br>