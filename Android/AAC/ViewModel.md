# ViewModel


## 使用

### 添加依赖

    implementation "android.arch.lifecycle:extensions:1.1.1"

### 自定义 MyViewModel 继承自 ViewModel 并包含一个LiveData

``` java
public class MyViewModel extends ViewModel {
    private MutableLiveData<List<String>> data;
    public LiveData<List<String>> getData(){
        if (data == null){
            data = new MutableLiveData<>();
        }
        return data;
    }
}
```

### Activity中 使用 ViewModelProviders 来获得ViewModel的实例,并借助LiveData订阅 data 的变化通知

``` java
    MyViewModel model = ViewModelProviders.of(this).get(MyViewModel.class);
```






[【AAC 系列四】深入理解架构组件：ViewModel](https://juejin.im/post/5d0111c1e51d45108126d226)</br>
[ViewModel 凭什么能保存重建数据](https://juejin.im/post/5d53c2636fb9a06ae94d293a)</br>
