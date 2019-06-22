###### 十八、NSOperation和NSOperationQueue

- **操作（Operation）：**

执行操作的意思，换句话说就是你在线程中执行的那段代码。
在 GCD 中是放在 block 中的。在 NSOperation 中，使用 NSOperation 子类 NSInvocationOperation、NSBlockOperation，或者自定义子类来封装操作。

- **操作队列（Operation Queues）：**

这里的队列指操作队列，即用来存放操作的队列。不同于 GCD 中的调度队列 FIFO（先进先出）的原则。NSOperationQueue 对于添加到队列中的操作，首先进入准备就绪的状态（就绪状态取决于操作之间的依赖关系），然后进入就绪状态的操作的开始执行顺序（非结束执行顺序）由操作之间相对的优先级决定（优先级是操作对象自身的属性）。

操作队列通过设置最大并发操作数（maxConcurrentOperationCount）来控制并发、串行。

NSOperationQueue 为我们提供了两种不同类型的队列：主队列和自定义队列。主队列运行在主线程之上，而自定义队列在后台执行。
