# Gradle

[Gradle for Android 第一篇( 从 Gradle 和 AS 开始 )](https://segmentfault.com/a/1190000004229002)

## Gradle 插件的编写

## 遇到的奇奇怪怪的bug

### Unable to load Class

![image.png](https://upload-images.jianshu.io/upload_images/61189-6c18826b13b11a65.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](https://upload-images.jianshu.io/upload_images/61189-a6989c320df04b94.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如上图,检查了好久,发现是 package 信息没写
因为实用 file 形式创建 groovy 文件不会自动补全 package信息的,所以建议先写package 信息在进行逻辑编写


### 提示 Gradle 版本过低

![image.png](https://upload-images.jianshu.io/upload_images/61189-72f021adafbc2f2b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](https://upload-images.jianshu.io/upload_images/61189-cf053f622899d3a6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我一直用的 3.3.2的,生成插件的时候习惯性的升级到3.4.0了,将版本降下来就好了 
或者也可以选择升级你的gradle 版本

