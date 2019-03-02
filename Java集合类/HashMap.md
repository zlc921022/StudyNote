# HashMap

## 实现

    数组+链表

## Hash实现:
    对 key 的 hash 取出key hashcode
    将 key hashcode 高16位与低16位取异或(让高位和低位都参与运算,为了有效减少Hash碰撞)
    最后对其取模,算出它在数组的index

![](https://ws2.sinaimg.cn/large/006tKfTcly1g0onaujn18j30ys0jcqae.jpg)


