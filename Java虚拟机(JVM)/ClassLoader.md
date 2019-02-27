# ClassLoader

## 提出问题

    ClassLoader家族都有那些
    类加载过程
    双亲委派机制


##   ClassLoader家族都有那些

    PathClassLoader 
    DexClassLoader 用于加载Dex文件,可以加载外部的Dex文件

## 双亲委派机制

    在ClassLoader加载类的时候,都会优先委派给他的父亲 parentLoader 去加载类,一直到底层,如果没有那个ClassLoader能加载,那就会自己加载这个类

    Why
        判断2个类是否相等,需要相同的路径以及相同的ClassLoader,这样能防止用户由于失误将JDK提供的类给覆盖掉

    