# hprof文件

    hprof 文件可以展示某一时刻java堆的使用情况,根据这个文件我们可以分析出哪些对象占用大量内存和未在合适时机释放,从而定位内存泄漏问题.

## Android中生成的方式

### 使用 adb 命令

``` java
adb shell am dumpheap <processname> <FileName>
```

### 使用 android.os.Debug.dumpHprofData 方法 

    直接使用 Debug 类提供的 dumpHprofData 方法即可

``` java
Debug.dumpHprofData(heapDumpFile.getAbsolutePath());
```

    Android Studio 自带 Android Profiler 的 Memory 模块的 dump 操作使用的是 adb 生成的方式.
    
### hprof 文件格式转换

    以上两种方法生成的 hprof 文件都是 Dalvik 格式,需要使用 AndroidSDK 提供的 hprof-conv 工具转换成J2SE HPROF格式才能在MAT等标准 hprof 工具中查看.

``` java
hprof-conv dump.hprof converted-dump.hprof 
```