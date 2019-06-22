###### 十五、延时函数(dispatch_after)

dispatch_after能让我们添加进队列的任务延时执行，该函数并不是在指定时间后执行处理，而只是在指定时间追加处理到dispatch_queue

``` 
//第一个参数是time，第二个参数是dispatch_queue，第三个参数是要执行的block
    dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(2 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
        
        NSLog(@"dispatch_after");
    });
```

由于其内部使用的是dispatch_time_t管理时间，而不是NSTimer。
所以如果在子线程中调用，相比performSelector:afterDelay,不用关心runloop是否开启