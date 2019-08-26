# Proguard



## 设置混淆字典

### -obfuscationdictionary filename 

    指定一个文本文件用来生成混淆后的名字。默认情况下，混淆后的名字一般为a,b，c这种。通过使用-obfuscationdictionary配置的字典文件，可以使用一些非英文字符做为类名。成员变量名、方法名。字典文件中的空格，标点符号，重复的词，还有以'#'开头的行都会被忽略。需要注意的是添加了字典并不会显著提高混淆的效果，只不过是更不利与人类的阅读。正常的编译器会自动处理他们，并且输出出来的jar包也可以轻易的换个字典再重新混淆一次。最有用的做法一般是选择已经在类文件中存在的字符串做字典，这样可以稍微压缩包的体积

### -classobfuscationdictionary filename 
    
    指定一个混淆类名的字典，字典的格式与-obfuscationdictionary相同

### -packageobfuscationdictionary filename

    指定一个混淆包名的字典，字典格式与-obfuscationdictionary相同

``` java
#混淆字典
-obfuscationdictionary proguard-dic.txt
-classobfuscationdictionary proguard-dic.txt
-packageobfuscationdictionary proguard-dic.txt
```

### 可以通过动态生成混淆字典来实现,每次生成的包都不一样

``` groovy
//生成混淆字典
def proguardDic() {
    final String DIC = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ"
    File file = new File(getProjectDir().path, "proguard-dic.txt")
    file.createNewFile()
    BufferedWriter writer = new BufferedWriter(new FileWriter(file))

    int random1 = Math.round(Math.random() * 9)
    int random2 = Math.round(Math.random() * 9)
    String str = DIC.substring(random1)
    str.toCharArray().toList().forEach {
        writer.write(it)
        writer.newLine()
        writer.writeLine(String.valueOf(it) + random2)
    }
    writer.flush()
    writer.close()
}
```

[Android 开发应该掌握的 Proguard 技巧](https://juejin.im/post/5b6af5655188251a9e171de2)

[安装包立减1M--微信Android资源混淆打包工具](https://mp.weixin.qq.com/s?__biz=MzAwNDY1ODY2OQ==&mid=208135658&idx=1&sn=ac9bd6b4927e9e82f9fa14e396183a8f#rd)
[AndResGuard](https://github.com/shwenzhang/AndResGuard/)
[Android Proguard(混淆)](https://www.jianshu.com/p/60e82aafcfd0)