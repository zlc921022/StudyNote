# Service 启动


## 在新进程中启动 Service 
    1. App 向 AMS 发送一个启动 Service 的消息
    2. AMS 检查启动 Service 的进程是否存在
        若不存在 先将 Service 信息保存下来,然后创建新的进程
    3. 新进程启动,通知 AMS,
    4. AMS 将刚才保存的 Service 信息 发送给新进程
    5. 新进程启动 Service

