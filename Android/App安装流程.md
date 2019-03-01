## App的安装流程

    Android系统会使用PMS解析Apk中的AndroidManifest文件,包括:
        四大组件的信息,比如静态Receiver 默认启动的Activity
        分配用户Id和用户组Id. 用户id是唯一的,因为Android是Linux系统,用户组Id指的是各种权限,每个权限都在一个用户组中,比如网络访问,分配了那些用户组id,就拥有了那些权限
        在Launcher中生成一个icon,icon中保存了默认启动的Activity的信息
        在App安装过程的最后,将上述信息记录在一个xml文件中,以备下次安装再次使用

    PackageParser
        专门用来解析AndroidManifest文件的