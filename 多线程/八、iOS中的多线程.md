###### 八、iOS中的多线程

**主要有三种：NSThread、NSoperationQueue、GCD**

**1. NSThread：轻量级别的多线程技术**

是我们自己手动开辟的子线程，如果使用的是初始化方式就需要我们自己启动，如果使用的是构造器方式它就会自动启动。只要是我们手动开辟的线程，都需要我们自己管理该线程，不只是启动，还有该线程使用完毕后的资源回收

``` 
  NSThread *thread = [[NSThread alloc] initWithTarget:self selector:@selector(testThread:) object:@"我是参数"];
    // 当使用初始化方法出来的主线程需要start启动
    [thread start];
    // 可以为开辟的子线程起名字
    thread.name = @"NSThread线程";
    // 调整Thread的权限 线程权限的范围值为0 ~ 1 。越大权限越高，先执行的概率就会越高，由于是概率，所以并不能很准确的的实现我们想要的执行顺序，默认值是0.5
    thread.threadPriority = 1;
    // 取消当前已经启动的线程
    [thread cancel];
    // 通过遍历构造器开辟子线程
    [NSThread detachNewThreadSelector:@selector(testThread:) toTarget:self withObject:@"构造器方式"];
```

* performSelector...只要是NSObject的子类或者对象都可以通过调用方法进入子线程和主线程，其实这些方法所开辟的子线程也是NSThread的另一种体现方式。
在编译阶段并不会去检查方法是否有效存在，如果不存在只会给出警告

``` 
    //在当前线程。延迟1s执行。响应了OC语言的动态性:延迟到运行时才绑定方法
        [self performSelector:@selector(aaa) withObject:nil afterDelay:1];
      // 回到主线程。waitUntilDone:是否将该回调方法执行完在执行后面的代码，如果为YES:就必须等回调方法执行完成之后才能执行后面的代码，说白了就是阻塞当前的线程；如果是NO：就是不等回调方法结束，不会阻塞当前线程
        [self performSelectorOnMainThread:@selector(aaa) withObject:nil waitUntilDone:YES];
      //开辟子线程
        [self performSelectorInBackground:@selector(aaa) withObject:nil];
      //在指定线程执行
        [self performSelector:@selector(aaa) onThread:[NSThread currentThread] withObject:nil waitUntilDone:YES]
```

需要注意的是：如果是带afterDelay的延时函数，会在内部创建一个 NSTimer，然后添加到当前线程的Runloop中。也就是如果当前线程没有开启runloop，该方法会失效。在子线程中，需要启动runloop(注意调用顺序)

``` 
[self performSelector:@selector(aaa) withObject:nil afterDelay:1];
[[NSRunLoop currentRunLoop] run];
```

而performSelector:withObject:只是一个单纯的消息发送，和时间没有一点关系。所以不需要添加到子线程的Runloop中也能执行

**2、GCD 对比 NSOprationQueue**

我们要明确NSOperationQueue与GCD之间的关系
GCD是面向底层的C语言的API，NSOpertaionQueue用GCD构建封装的，是GCD的高级抽象。

1、GCD执行效率更高，而且由于队列中执行的是由block构成的任务，这是一个轻量级的数据结构，写起来更方便
2、GCD只支持FIFO的队列，而NSOperationQueue可以通过设置最大并发数，设置优先级，添加依赖关系等调整执行顺序
3、NSOperationQueue甚至可以跨队列设置依赖关系，但是GCD只能通过设置串行队列，或者在队列内添加barrier(dispatch_barrier_async)任务，才能控制执行顺序,较为复杂
4、NSOperationQueue因为面向对象，所以支持KVO，可以监测operation是否正在执行（isExecuted）、是否结束（isFinished）、是否取消（isCanceld）

* 实际项目开发中，很多时候只是会用到异步操作，不会有特别复杂的线程关系管理，所以苹果推崇的且优化完善、运行快速的GCD是首选
* 如果考虑异步操作之间的事务性，顺序行，依赖关系，比如多线程并发下载，GCD需要自己写更多的代码来实现，而NSOperationQueue已经内建了这些支持
* 不论是GCD还是NSOperationQueue，我们接触的都是任务和队列，都没有直接接触到线程，事实上线程管理也的确不需要我们操心，系统对于线程的创建，调度管理和释放都做得很好。而NSThread需要我们自己去管理线程的生命周期，还要考虑线程同步、加锁问题，造成一些性能上的开销