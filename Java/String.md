# String StringBuilder StringBuffer

    String 底层是不可变的char数组
    
    StringBuilder 和 StringBuffer 的话 底层是
    可变的char数组

    append方法 底层会 通过 Array的copy方法 去扩容

    StringBuffer 是线程安全 因为在各种方法都加了 syn.. 关键字修饰
StringBuilder 是线程不安全的 速度快

