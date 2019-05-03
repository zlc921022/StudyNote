# Android 打包流程


![16204d3ceedb2328.png](https://upload-images.jianshu.io/upload_images/61189-5178e754aa615226.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

    aapt工具编译res资源文件，把大部分xml文件编译成二进制文件（图片文件除外），同时生成R.Java文件和resources.arsc文件，里面保存了资源的ID和在APK中的路径。
    如果项目中有使用AIDL，那么就会把.aidl文件编译成.java文件。
    将所有.java文件(包括R文件和AIDL生成的.java文件)，通过javac工具生成class文件。
    将生成的.class文件和第三方库的.class文件通过dx工具生成classes.dex文件(如果有分包，那么可能有多个)。
    第1步中的资源文件、dex文件和第三方的非java资源包(.so)，通过apkbuilder工具生成未签名的apk包。
    签名，jarsigner工具，如果是debug模式用默认签名，release模式用开发者的签名。
    对齐，通过zipalign工具对apk中的未压缩资源（图片、视频）进行“对齐操作”，让资源按4字节的边界进行对齐，使得资源访问速度更快。（优化思想类似于内存对齐，



![transformconsume_transform.png](https://upload-images.jianshu.io/upload_images/61189-85778a7046b23933.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
