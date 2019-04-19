# ButterKnife


### 遇到的bug

#### 空指针

    1. 可能是没有调用 ButterKnife # bind 方法

    2. 主工程 未添加 annotationProcessor 'com.jakewharton:butterknife-compiler:10.1.0' 
    
    要抽取base库,所以将ButterKnife抽取到base库中了,测试的时候 发现一直报 NullPoint,检查了BaseActivity, ButterKnife # bind 是有调用的,发现是 annotationProcessor 'com.jakewharton:butterknife-compiler:10.1.0' 没有配置
    这个是ButterKnife注解处理器 必须配置在对应库中注解才会生效 不添加会报空指针
