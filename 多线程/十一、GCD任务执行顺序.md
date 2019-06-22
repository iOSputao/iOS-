###### 十一、GCD任务执行顺序

1、串行队列先异步后同步

``` 
    dispatch_queue_t serialQueue = dispatch_queue_create("test", DISPATCH_QUEUE_SERIAL);
    
    NSLog(@"1");
    
    dispatch_async(serialQueue, ^{
        
         NSLog(@"2");
    });
    
    NSLog(@"3");
    
    dispatch_sync(serialQueue, ^{
        
        NSLog(@"4");
    });
    
    NSLog(@"5");
```

打印顺序是13245
原因是:
首先先打印1
接下来将任务2其添加至串行队列上，由于任务2是异步，不会阻塞线程，继续向下执行，打印3
然后是任务4,将任务4添加至串行队列上，因为任务4和任务2在同一串行队列，根据队列先进先出原则，任务4必须等任务2执行后才能执行，又因为任务4是同步任务，会阻塞线程，只有执行完任务4才能继续向下执行打印5
所以最终顺序就是13245。
这里的任务4在主线程中执行，而任务2在子线程中执行。
如果任务4是添加到另一个串行队列或者并行队列，则任务2和任务4无序执行(可以添加多个任务看效果)

**2、performSelector**

``` 
dispatch_async(dispatch_get_global_queue(0, 0), ^{
        
        [self performSelector:@selector(test:) withObject:nil afterDelay:0];
    });
```

这里的test方法是不会去执行的，原因在于

``` 
- (void)performSelector:(SEL)aSelector withObject:(nullable id)anArgument afterDelay:(NSTimeInterval)delay;
```

这个方法要创建提交任务到runloop上的，而gcd底层创建的线程是默认没有开启对应runloop的，所有这个方法就会失效。
而如果将dispatch_get_global_queue改成主队列，由于主队列所在的主线程是默认开启了runloop的，就会去执行(将dispatch_async改成同步，因为同步是在当前线程执行，那么如果当前线程是主线程，test方法也是会去执行的)。