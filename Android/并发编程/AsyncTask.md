# AsyncTask

## 提出问题

    什么是 AsyncTask
    AsyncTask 的使用方法
    AsyncTask 的内部原理
    AsyncTask 的注意事项

## AsyncTask 介绍

    Android 提供的 用于异步消息处理的类
    本质上就是一个 封装了 线程池 和 Handler 的异步框架

## AsyncTask 使用

### 自定义AsyncTask

``` java
class DemoAsyncTack extends AsyncTask<Void(传入参数),String（执行中阶段行结果）,String(任务完成返回结果)>{
    @Override
    protected void onPreExecute() {
        super.onPreExecute();
        //doInBackground执行前一些初始化的操作都在这里
    }

    @Override
    protected String doInBackground(Void... voids) {
        //后台耗时任务执行中。。。
        return null;
    }
    @Override
    protected void onProgressUpdate(String... values) {
        super.onProgressUpdate(values);
        //后台执行的任务会发回一个或多个阶段性进度结果，这个是可以用来去更新交互页面。
    }
    @Override
    protected void onCancelled() {
        super.onCancelled();
        //在后台任务被取消时回调
    }
    @Override
    protected void onPostExecute(String s) {
        super.onPostExecute(s);
        //耗时任务完成返回结果，刷新ui
    }
}
    
//执行AsycnTask
DemoAsyncTack asyncTack=new DemoAsyncTack();
asyncTack.execute();
```

## 注意问题

    内存泄露

        在 Activity 内部定义的一个 AsyncTask,它属于一个内部类,该类本身和外面的 Activity 是有引用关系的,如果 Activity 要销毁的时候, AsyncTask 还仍然在运行，这会导致 Activity 没有办法完全释放，从而引发内存泄漏。
        及时调用 cancel() 来取消 AsyncTask

    结果丢失

        旋转屏幕等会导致Activity 重新创建,但是 AsyncTask 还引用着之前的 Activity 所以再去更新 UI 的时候 就无效果了.

    并行 or 串行

        2.3之后是串行,但可以并行,需要执行excute()方法,并行会导致线程池不稳定

    所有的 AsyncTask 任务都是被线性调度执行的，他们处在同一个任务队列当中，按顺序逐个执行。假设你按照顺序启动20个 AsyncTask,一旦其中的某个 AsyncTask 执行时间过长，队列中的其他剩余 AsyncTask 都处于阻塞状态，必须等到该任务执行完毕之后才能够有机会执行下一个任务。