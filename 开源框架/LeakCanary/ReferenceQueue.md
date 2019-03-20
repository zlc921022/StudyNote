# ReferenceQueue

## 介绍

    eferenceQueue可翻译为引用队列,换言之就是存放引用的队列,保存的是Reference对象.
    其作用在于 Reference对象所引用的对象被GC回收时 , 该Reference对象将会被加入引用队列中（ReferenceQueue）的队列末尾.

## 监控 弱引用 

    弱引用在定义的时候可以指定引用对象和一个 ReferenceQueue,弱引用对象在垃圾回收器执行回收方法时,如果原对象只有这个弱引用对象引用着,那么会回收原对象,并将弱引用对象加入到 ReferenceQueue,通过 ReferenceQueue 的 poll 方法,可以取出这个弱引用对象,获取弱引用对象本身的一些信息.
## 常用方法

    public Reference poll()：从队列中取出一个元素，队列为空则返回null； 
    public Reference remove()：从队列中出对一个元素，若没有则阻塞至有可出队元素； 
    public Reference remove(long timeout)：从队列中出对一个元素，若没有则阻塞至有可出对元素或阻塞至超过timeout毫秒；

## 模拟代码

``` java
private void testSoftReference(){
    System.out.println("testSoftReference");
    ReferenceQueue referenceQueue = new ReferenceQueue();
    Object o = new Object();

    SoftReference softReference = new SoftReference<Object>(o,referenceQueue);

    // 去除强引用
    o = null;

    Runtime.getRuntime().gc();

    try {
        Thread.sleep(100);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }

    Reference ref = null;

    while( (ref = referenceQueue.poll()) != null){
        System.out.println("=====testSoftReference=======  \n ref in queue");
    }
}

private void testWeakReference(){
    System.out.println("testWeakReference");
    ReferenceQueue referenceQueue = new ReferenceQueue();
    Object o = new Object();

    WeakReference weakReference = new WeakReference<Object>(o,referenceQueue);

    // 去除强引用
    o = null;

    Runtime.getRuntime().gc();

    try {
        Thread.sleep(100);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }

    Reference ref = null;

    while( (ref = referenceQueue.poll()) != null){
        System.out.println("=====testWeakReference=======  \n ref in queue");
    }
}
```

### 输出

``` js
testSoftReference
testWeakReference
=====testWeakReference=======  
 ref in queue
```