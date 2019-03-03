# 多进程相关

## 提出问题

    Activity间传递对象为什么需要序列化

## 基础 Serializable接口

    Java提供,只需要实现'Serializable接口'即可,配合下面的Stream流来进行写入与读取即可
    
``` java
public class User implements Serializable {
    // 帮助反序列化
    private static final long serialVersionUID = 1L;
    public int userId;
    public String name;

    public User(int userId,String name){
        this.userId = userId;
        this.name = name;
    }
}

public static void doSerializable() {
   // 序列化过程
   User user = new User(0, "Test");
   try {
       ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream("output.txt"));
       out.writeObject(user);
       out.close();
   } catch (IOException e) {
       e.printStackTrace();
   }

   // 反序列化过程
   try {
       ObjectInputStream in = new ObjectInputStream(new FileInputStream("output.txt"));
       User newUser = (User) in.readObject();
       System.out.println("User " + newUser.name);
       in.close();
   } catch (IOException e) {
       e.printStackTrace();
   } catch (ClassNotFoundException e) {
       e.printStackTrace();
   }
}
```


## 基础 Parcelable接口

    这个是安卓提供的,实现之后可以实现序列化并可以通过Intent以及Binder来传递

## Binder 

    用了动态代理模式

    使用反射来实现
    Binder 分为 Client 和 Server 两个进程,其中发送消息的是Client,接收消息的是Server

## AIDL

    Client 调用 asInterface 方法,作用是判断参数(即IBinder对象),与自己是否在同一进程
        若是 则直接转换,直接使用,接下来则与Binder跨进程通信无关
        若否 则把这个IBinder参数包装成一个Proxy对象,这时调用Stub的sun方法,间接调用Proxy的sum方法

    Proxy 在自己的sum方法中,会使用Parcelable来准备数据,将函数名称,函数参数都写入_data,让_reply接收函数返回值,最后使用 IBinder 的transact方法,就可以将数据传给Binder的Server端了

    Server 通过onTransact方法接收Client进程传来的数据,包括函数名称,函数参数,找到对应的函数,把参数传进去,得到结果,并返回.所以onTransact函数经历了读数据->执行要调用的函数->把执行结果再写数据的过程


