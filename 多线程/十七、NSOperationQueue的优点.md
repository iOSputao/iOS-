###### 十七、NSOperationQueue的优点

NSOperation、NSOperationQueue 是苹果提供给我们的一套多线程解决方案。实际上 NSOperation、NSOperationQueue 是基于 GCD 更高一层的封装，完全面向对象。但是比 GCD 更简单易用、代码可读性也更高。

1、可以添加任务依赖，方便控制执行顺序

2、可以设定操作执行的优先级

3、任务执行状态控制:isReady,isExecuting,isFinished,isCancelled

如果只是重写NSOperation的main方法，由底层控制变更任务执行及完成状态，以及任务退出
如果重写了NSOperation的start方法，自行控制任务状态
系统通过KVO的方式移除isFinished==YES的NSOperation

4、可以设置最大并发量
