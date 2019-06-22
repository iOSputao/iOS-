###### 十九、NSThread+runloop实现常驻线程

NSThread在实际开发中比较常用到的场景就是去实现常驻线程。

- 由于每次开辟子线程都会消耗cpu，在需要频繁使用子线程的情况下，频繁开辟子线程会消耗大量的cpu，而且创建线程都是任务执行完成之后也就释放了，不能再次利用，那么如何创建一个线程可以让它可以再次工作呢？也就是创建一个常驻线程。

首先常驻线程既然是常驻，那么我们可以用GCD实现一个单例来保存NSThread

``` 
+ (NSThread *)shareThread {
    
    static NSThread *shareThread = nil;
    
    static dispatch_once_t oncePredicate;
    
    dispatch_once(&oncePredicate, ^{
        
        shareThread = [[NSThread alloc] initWithTarget:self selector:@selector(threadTest) object:nil];

        [shareThread setName:@"threadTest"];
        
        [shareThread start];
    });
    
    return shareThread;
}
```

这样创建的thread就不会销毁了吗？

``` 
[self performSelector:@selector(test) onThread:[ViewController shareThread] withObject:nil waitUntilDone:NO];

- (void)test
{
    NSLog(@"test:%@", [NSThread currentThread]);
}
```

并没有打印，说明test方法没有被调用。
那么可以用runloop来让线程常驻

``` 
+ (NSThread *)shareThread {
    
    static NSThread *shareThread = nil;
    
    static dispatch_once_t oncePredicate;
    
    dispatch_once(&oncePredicate, ^{
        
        shareThread = [[NSThread alloc] initWithTarget:self selector:@selector(threadTest2) object:nil];
        
        [shareThread setName:@"threadTest"];
        
        [shareThread start];
    });
    
    return shareThread;
}

+ (void)threadTest
{
    @autoreleasepool {
        
        NSRunLoop *runLoop = [NSRunLoop currentRunLoop];
        
        [runLoop addPort:[NSMachPort port] forMode:NSDefaultRunLoopMode];
        
        [runLoop run];
    }
}
```

这时候再去调用performSelector就有打印了。