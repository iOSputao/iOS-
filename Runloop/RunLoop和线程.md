#### 六、RunLoop和线程

- 线程和RunLoop是一一对应的,其映射关系是保存在一个全局的 Dictionary 里
- 自己创建的线程默认是没有开启RunLoop的

**1、怎么创建一个常驻线程？**

1、为当前线程开启一个RunLoop（第一次调用 [NSRunLoop currentRunLoop]方法时实际是会先去创建一个RunLoop）
2、向当前RunLoop中添加一个Port/Source等维持RunLoop的事件循环（如果RunLoop的mode中一个item都没有，RunLoop会退出）
3、启动该RunLoop

``` 
  @autoreleasepool {
        
        NSRunLoop *runLoop = [NSRunLoop currentRunLoop];
        
        [[NSRunLoop currentRunLoop] addPort:[NSMachPort port] forMode:NSDefaultRunLoopMode];
        
        [runLoop run];
        
    }
```

**2、输出下边代码的执行顺序**

``` 
NSLog(@"1");

dispatch_async(dispatch_get_global_queue(0, 0), ^{
    
    NSLog(@"2");

    [self performSelector:@selector(test) withObject:nil afterDelay:10];
    
    NSLog(@"3");
});

NSLog(@"4");

- (void)test
{
    
    NSLog(@"5");
}
```

答案是1423，test方法并不会执行。
原因是如果是带afterDelay的延时函数，会在内部创建一个 NSTimer，然后添加到当前线程的RunLoop中。也就是如果当前线程没有开启RunLoop，该方法会失效。
那么我们改成:

``` 
dispatch_async(dispatch_get_global_queue(0, 0), ^{
        
        NSLog(@"2");
        
        [[NSRunLoop currentRunLoop] run];
        
        [self performSelector:@selector(test) withObject:nil afterDelay:10];
  
        NSLog(@"3");
    });
```

然而test方法依然不执行。
原因是如果RunLoop的mode中一个item都没有，RunLoop会退出。即在调用RunLoop的run方法后，由于其mode中没有添加任何item去维持RunLoop的时间循环，RunLoop随即还是会退出。
所以我们自己启动RunLoop，一定要在添加item后

``` 
dispatch_async(dispatch_get_global_queue(0, 0), ^{
        
        NSLog(@"2");
        
        [self performSelector:@selector(test) withObject:nil afterDelay:10];
        
        [[NSRunLoop currentRunLoop] run];
  
        NSLog(@"3");
    });
```

**3、怎样保证子线程数据回来更新UI的时候不打断用户的滑动操作？**

当我们在子请求数据的同时滑动浏览当前页面，如果数据请求成功要切回主线程更新UI，那么就会影响当前正在滑动的体验。
我们就可以将更新UI事件放在主线程的`NSDefaultRunLoopMode`上执行即可，这样就会等用户不再滑动页面，主线程RunLoop由`UITrackingRunLoopMode`切换到`NSDefaultRunLoopMode`时再去更新UI

``` 
[self performSelectorOnMainThread:@selector(reloadData) withObject:nil waitUntilDone:NO modes:@[NSDefaultRunLoopMode]];
```

