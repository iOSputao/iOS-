###### 九、GCD---队列

iOS中，有GCD、NSOperation、NSThread等几种多线程技术方案。

而GCD共有三种队列类型：
main queue：通过dispatch_get_main_queue()获得，这是一个与主线程相关的串行队列。

global queue：全局队列是并发队列，由整个进程共享。存在着高、中、低三种优先级的全局队列。调用dispath_get_global_queue并传入优先级来访问队列。

自定义队列：通过函数dispatch_queue_create创建的队列。