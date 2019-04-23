# AndroidX的坑

    开了个新项目,用的是最新的AndroidX 支持库,这里记录下 踩到的坑

1. Glide引入

    使用Glide 4.9.0 的时候,发现不兼容 AndroidX,不能直接用

    首先报这个错

![DB4F9639850AA314BAE206D2165FCAE8.png](https://upload-images.jianshu.io/upload_images/61189-46422dbda443332d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

网上搜的是这样添加support 依赖,但是会报下面的错,问题还是没有解决

![A5A46B1ACA68B5AAE3C86625ED2EBC69.png](https://upload-images.jianshu.io/upload_images/61189-556e5225163bed96.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![2BD5477E50CF3A29E600C532CAD84FD0.png](https://upload-images.jianshu.io/upload_images/61189-18d81a3b696f02ce.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

最后发现是少配置了 几个配置参数,按照下面的配置就ok了

![image.png](https://upload-images.jianshu.io/upload_images/61189-59f55cdac3cb01ee.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![A9B5833A8516A1FF9968197F9594A81C.png](https://upload-images.jianshu.io/upload_images/61189-357ddda03cb48fff.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)