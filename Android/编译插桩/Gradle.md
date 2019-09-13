# Gradle

## 基本自定义构建
### Gradle文件

#### settings 文件

    在初始化阶段执行,并且定义了哪些模块应该包含在构件内

#### 根目录下 build.gradle

    allprojects 代码块可用来声明那些需要被用于所有模块的属性

#### 模块下的 build.gradle

    可以覆盖顶层 build.gradle 文件中的任何属性

### 任务入门

#### 基础任务

基础插件下的行为:
    assemable : 集合项目输出
    clean : 清理项目的输出
    check : 运行所有的检查,通常是单元测试和集成测试
    build : 同时运行 assemable 和 check

#### Android任务
    assemable : 为每个构建版本创建一个Apk
    clean : 删除所有的构建内容, 例如 Apk文件
    check : 运行Lint检查, 如果Lint发现一个问题,则可终止构建
    build : 同时运行 assemable 和 check

##### Android插件添加的新任务
    connectedCheck : 在连接设备或模拟器上运行测试
    deviceCheck : 一个占位任务, 专为其他插件在远端设备锁行运行测试
    installDebug 和 installRelease : 在连接设备或模拟器上安装指定版本
    所有的 installTasks 都会有相对应的 uninstall 任务

### 自定义构建

#### 操控 manifest 条目

    applicationId
    minSdkVersion
    targetSdkVersion
    versionCode
    versionName

#### BuildConfig 和 资源

``` groovy
android {
    buildTypes {
        debug {
            buildConfigField("boolean", "LOG_DEBUG", "true")
            resValue "string", "app_test", "Example DEBUG"
        }
        release {
            buildConfigField("boolean", "LOG_DEBUG", "false")
            resValue "string", "app_test", "Example relea"
        }
    }
}
```

#### 项目范围的设置
    给根目录下的 build.gradle 文件添加一个含有自定义属性的 ext 代码块
``` groovy
ext {
    versionCode = 6
    versionName = "1.0.1"
}
android {
    versionCode parent.ext.versionCode
    versionName parent.ext.versionName
}
```

## 创建构建Variant

### 构建类型

#### variant 过滤器

``` groovy
android {
    android.applicationVariants.all { variant ->
         variant.outputs.all { output ->
            String flavorName = variant.flavorName.capitalize()
            outputFileName = "${flavorName}_${buildType.name}_${getAppVersionName()}_${getTime()}.apk"
        }
    }
}
```

## 管理多模块构建

### 加速多模块构建

    如果想并行构建项目
    需要在项目根目录的 gradle.properties文件中配置 parallel 属性
    org.gradle.parallel= true
    Gradle 会基于可用的CPU 内核,来选择正确的线程数量,为了防止出现同一模块同时执行两个任务的问题,每个线程只拥有一个完整的模块