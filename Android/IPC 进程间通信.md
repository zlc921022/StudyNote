# IPC 进程间通信

* 基础 Serializable接口

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


* 基础 Parcelable接口

    这个是安卓提供的,实现之后可以实现序列化并可以通过Intent以及Binder来传递
    
* Binder

用了动态代理模式

使用反射来实现

