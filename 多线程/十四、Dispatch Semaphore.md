###### 十四、Dispatch Semaphore

GCD 中的信号量是指 Dispatch Semaphore，是持有计数的信号。

Dispatch Semaphore 提供了三个函数

**1.dispatch_semaphore_create：创建一个Semaphore并初始化信号的总量**
**2.dispatch_semaphore_signal：发送一个信号，让信号总量加1**
**3.dispatch_semaphore_wait：可以使总信号量减1，当信号总量为0时就会一直等待（阻塞所在线程），否则就可以正常执行。**

Dispatch Semaphore 在实际开发中主要用于：

- 保持线程同步，将异步执行任务转换为同步执行任务
- 保证线程安全，为线程加锁

**1、保持线程同步：**

``` 
dispatch_semaphore_t semaphore = dispatch_semaphore_create(0);
    
    __block NSInteger number = 0;
    
    dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^{
        
        number = 100;
        
        dispatch_semaphore_signal(semaphore);
    });
    
    dispatch_semaphore_wait(semaphore, DISPATCH_TIME_FOREVER);
    
    NSLog(@"semaphore---end,number = %zd",number);
```

dispatch_semaphore_wait加锁阻塞了当前线程，dispatch_semaphore_signal解锁后当前线程继续执行

**2、保证线程安全，为线程加锁：**

在线程安全中可以将dispatch_semaphore_wait看作加锁，而dispatch_semaphore_signal看作解锁
首先创建全局变量

``` 
 _semaphore = dispatch_semaphore_create(1);
```

注意到这里的初始化信号量是1。

``` 
- (void)asyncTask
{
    
    dispatch_semaphore_wait(_semaphore, DISPATCH_TIME_FOREVER);
    
    count++;
    
    sleep(1);
    
    NSLog(@"执行任务:%zd",count);
    
    dispatch_semaphore_signal(_semaphore);
}
```

异步并发调用asyncTask

``` 
  for (NSInteger i = 0; i < 100; i++) {
        
        dispatch_async(dispatch_get_global_queue(0, 0), ^{
            
            [self asyncTask];
        });
    }
```

然后发现打印是从任务1顺序执行到100，没有发生两个任务同时执行的情况。

原因如下:
在子线程中并发执行asyncTask，那么第一个添加到并发队列里的，会将信号量减1，此时信号量等于0，可以执行接下来的任务。而并发队列中其他任务，由于此时信号量不等于0，必须等当前正在执行的任务执行完毕后调用dispatch_semaphore_signal将信号量加1，才可以继续执行接下来的任务，以此类推，从而达到线程加锁的目的。

