###### 十三、dispatch_group_async

场景：在n个耗时并发任务都完成后，再去执行接下来的任务。比如，在n个网络请求完成后去刷新UI页面。

``` 
dispatch_queue_t concurrentQueue = dispatch_queue_create("test1", DISPATCH_QUEUE_CONCURRENT);

    dispatch_group_t group = dispatch_group_create();
    
    for (NSInteger i = 0; i < 10; i++) {
        
        dispatch_group_async(group, concurrentQueue, ^{
            
            sleep(1);
            
            NSLog(@"%zd:网络请求",i);
        });
    }
    
    dispatch_group_notify(group, dispatch_get_main_queue(), ^{
        
        NSLog(@"刷新页面");
    });
```