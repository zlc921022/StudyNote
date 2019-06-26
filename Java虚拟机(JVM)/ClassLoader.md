# ClassLoader

## 提出问题

    ClassLoader家族都有那些
    类加载过程
    双亲委派机制


##   ClassLoader家族都有那些

### BootClassLoader（Java的BootStrap ClassLoader）

    用于加载Android Framework层class文件。

### BaseDexClassLoader

    是PathClassLoader和DexClassLoader的父类。

### PathClassLoader（Java的App ClassLoader）

``` java
/**
 * dexPath ：dex路径
 * optimizedDirectory :制定输出dex优化后的odex文件，可以为null
 * libraryPath:动态库路径（将被添加到app动态库搜索路径列表中）
 * parent:制定父类加载器，以保证双亲委派机制从而实现每个类只加载一次。
 */
public PathClassLoader(String dexPath, ClassLoader parent) {
    super(dexPath, null, null, parent);
}

public PathClassLoader(String dexPath, String libraryPath,
        ClassLoader parent) {
    super(dexPath, null, libraryPath, parent);
}
```

### DexClassLoader

    Java的Custom ClassLoader
    可以加载文件系统上的jar,dex,apk;可以从SD卡中加载未安装的apk

``` java
/**
 * dexPath ：dex路径
 * optimizedDirectory :制定输出dex优化后的odex文件，可以为null
 * libraryPath:动态库路径（将被添加到app动态库搜索路径列表中）
 * parent:制定父类加载器，以保证双亲委派机制从而实现每个类只加载一次。
 */
public DexClassLoader(String dexPath, String optimizedDirectory,
        String libraryPath, ClassLoader parent) {
    super(dexPath, new File(optimizedDirectory), libraryPath, parent);
}
```

## 双亲委派机制

    在ClassLoader加载类的时候,都会优先委派给他的父亲 parentLoader 去加载类,一直到底层,如果没有那个ClassLoader能加载,那就会自己加载这个类

    Why
        判断2个类是否相等,需要相同的路径以及相同的ClassLoader,这样能防止用户由于失误将JDK提供的类给覆盖掉
