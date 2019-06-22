#### 二、RunLoop的数据结构

`NSRunLoop(Foundation)`是`CFRunLoop(CoreFoundation)`的封装，提供了面向对象的API
RunLoop 相关的主要涉及五个类：

`CFRunLoop`：RunLoop对象
`CFRunLoopMode`：运行模式
`CFRunLoopSource`：输入源/事件源
`CFRunLoopTimer`：定时源
`CFRunLoopObserver`：观察者

**1、CFRunLoop**

由`pthread`(线程对象，说明RunLoop和线程是一一对应的)、`currentMode`(当前所处的运行模式)、`modes`(多个运行模式的集合)、`commonModes`(模式名称字符串集合)、`commonModelItems`(Observer,Timer,Source集合)构成

**2、CFRunLoopMode**

由name、source0、source1、observers、timers构成

**3、CFRunLoopSource**

分为source0和source1两种

- `source0:`
即非基于port的，也就是用户触发的事件。需要手动唤醒线程，将当前线程从内核态切换到用户态


- `source1:`
基于port的，包含一个 mach_port 和一个回调，可监听系统端口和通过内核和其他线程发送的消息，能主动唤醒RunLoop，接收分发系统事件。
具备唤醒线程的能力

**4、CFRunLoopTimer**

基于时间的触发器，基本上说的就是NSTimer。在预设的时间点唤醒RunLoop执行回调。因为它是基于RunLoop的，因此它不是实时的（就是NSTimer 是不准确的。 因为RunLoop只负责分发源的消息。如果线程当前正在处理繁重的任务，就有可能导致Timer本次延时，或者少执行一次）。

**5、CFRunLoopObserver**

监听以下时间点:`CFRunLoopActivity`


- `kCFRunLoopEntry`             
RunLoop准备启动

- `kCFRunLoopBeforeTimers`      
RunLoop将要处理一些Timer相关事件

- `kCFRunLoopBeforeSources`     
RunLoop将要处理一些Source事件

- `kCFRunLoopBeforeWaiting`        
RunLoop将要进行休眠状态,即将由用户态切换到内核态

- `kCFRunLoopAfterWaiting`
RunLoop被唤醒，即从内核态切换到用户态后

- `kCFRunLoopExit`
RunLoop退出

- `kCFRunLoopAllActivities`
监听所有状态

**6、各数据结构之间的联系**

线程和RunLoop一一对应， RunLoop和Mode是一对多的，Mode和source、timer、observer也是一对多的