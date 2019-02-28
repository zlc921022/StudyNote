# 动画相关 HenCoder

## Animation

## 帧动画

    Drawable文件夹放置图片,一张张连续切换,
    图片数量过多,容易出现OOM

## View动画
    
    Translate
    Scale
    Rotate
    Alpha
    
    只是改变View的视图,不改变View的状态,最直观的就是点击事件不会跟着View动画移动
    
## Property动画(属性动画)
    
    ObjectAnimator
    ValueAnimator
    AnimationSet

### ViewPropertyAnimator

    View..animate().translationX(100);
    
    系统实现的 只能使用系统提供的方法,会自动运行
     还可以使用  setDuration() 来设置时长
    setInterpolator() 来设置速度模型(插值器)
    也可以设置监听器  
    
### ObjectAnimator
    
       ObjectAnimator animator =ObjectAnimator.ofFloat(iv_img,"progress",40);
                animator.start();

    可以使用系统自带的方法
    也可以实现自定义,但要注意实现 getter/setter 方法 而且在 setter 方法中要调用 invalidate() 方法来进行重绘,
    使用时 跟系统提供的方法一样,但是需要手动 start()
    
    还可以使用  setDuration() 来设置时长
    setInterpolator() 来设置速度模型(插值器)
    也可以设置监听器
    
    TimeInterpolator 插值器 
    
        根据时间流逝的百分比来计算出当前属性值改变的百分比
       
    TypeEvaluator 估值器
    
        根据当前属性改变的百分比来计算改变后的属性值
