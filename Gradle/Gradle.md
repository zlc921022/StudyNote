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

##### Android插件添加的心任务
    connectedCheck : 在连接设备或模拟器上运行测试
    deviceCheck : 一个占位任务, 专为其他插件在远端设备锁行运行测试
    installDebug 和 installRelease : 在连接设备或模拟器上安装指定版本
    所有的 installTasks 都会有相对应的 uninstall 任务

