# SparseArray & ArrayMap

## SparseArray

    内部通过两个数组来进行数据存储的

        一个存储key (Key 只能是 int ) ,另外一个存储value

    存取都通过二分搜索来查找

    对 HashMap 优化了装箱拆箱的步骤 

## ArrayMap

    内部通过两个数组来进行数据存储的

        一个存储key的hashcode ,另外一个存储key,value
    

[数据结构HashMap（Android SparseArray 和ArrayMap）](https://juejin.im/post/5b2a24dd51882574bc55b974)