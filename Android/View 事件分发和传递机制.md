# View 事件分发和传递机制


    interceptTouchEvent 拦截
    dispatchTouchEvent 分发
    onTouchEvent 处理

    Activity 和 View 没有 interceptTouchEvent() 方法









## 用白话描述

### 老板 - Activity：

```
客户通知老板你有项目了。老板的dispatchEvent()会被调用。
老板.dispatchTouchEvent(){
    //老板先通知主管去处理，
    如果主管给的回复是：老板你不用管接下去的事。我们会处理的。
    if(主管.dispatchTouchEvent()){
        return true;//就直接结束了。
    }
    
    //手下的人说这个app开发不了，只能老板出马做事(跟客户去沟通去)
    return 老板.onTouchEvent();
}
```

### 主管 - ViewGroup

```
老板通知了主管有个app要你们部门去开发。主管的dispatchTouchEvent()会被调用
主管.dispatchTouchEvent(){
    //主管把这个活拦下来准备自己来开发这个app
    if(主管.interceptTouchEvent()){
        return 主管.onTouchEvent();//主管也有做事能力
    }else{
        //主管不拦截，主管也可以去通知开发人员,
        //如果开发人员回馈说主管你别管了。我们这个app能做好
        if(开发人员.dispatchTouchEvent()){
            return true; //直接就结束了。
        }else{
            //如果手下的开发人员也反馈给主管说搞不定。
            //就只能主管自己出来做事了。
            return 主管.onTouchEvent();
        }
    }
}
```

### 开发人员 - View

```
主管通知了开发人员有个app要开发。开发人员的dispatchTouchEvent()会被调用
开发人员.dispatchTouchEvent(){
    return 开发人员.onTouchEvent();
}
```

requestDisallowInteceptTouchEvent
