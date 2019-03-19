# LeakCanary

## 使用

    使用见 https://github.com/square/leakcanary

## 简述

    1、RefWatcher.watch()创建了一个KeyedWeakReference用于去观察对象。

    2、然后，在后台线程中，它会检测引用是否被清除了，并且是否没有触发GC。

    3、如果引用仍然没有被清除，那么它将会把堆栈信息保存在文件系统中的.hprof文件里。

    4、HeapAnalyzerService被开启在一个独立的进程中，并且HeapAnalyzer使用了HAHA开源库解析了指定时刻的堆栈快照文件heap dump。

    5、从heap dump中，HeapAnalyzer根据一个独特的引用key找到了KeyedWeakReference，并且定位了泄露的引用。

    6、HeapAnalyzer为了确定是否有泄露，计算了到GC Roots的最短强引用路径，然后建立了导致泄露的链式引用。

    7、这个结果被传回到app进程中的DisplayLeakService，然后一个泄露通知便展现出来了。

    在一个Activity执行完onDestroy()之后，将它放入WeakReference中，然后将这个WeakReference类型的Activity对象与ReferenceQueque关联。这时再从ReferenceQueque中查看是否有没有该对象，如果没有，执行gc，再次查看，还是没有的话则判断发生内存泄露了。最后用HAHA这个开源库去分析dump之后的heap内存。

[LeakCanary源码分析](https://blog.csdn.net/u013000152/article/details/80239240)</br>
[看完这篇 LeakCanary 原理分析，又可以虐面试官了！](https://juejin.im/entry/5c3fefe66fb9a049a979fb77)</br>
[007 LeakCanary 内存泄漏原理完全解析](https://juejin.im/post/5c054e91e51d45242906ed68)</br>