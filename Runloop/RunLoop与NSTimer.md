#### 五、RunLoop与NSTimer

一个比较常见的问题：滑动tableView时，定时器还会生效吗？
默认情况下RunLoop运行在`kCFRunLoopDefaultMode`下，而当滑动tableView时，RunLoop切换到`UITrackingRunLoopMode`，而Timer是在`kCFRunLoopDefaultMode`下的，就无法接受处理Timer的事件。
怎么去解决这个问题呢？把Timer添加到UITrackingRunLoopMode上并不能解决问题，因为这样在默认情况下就无法接受定时器事件了。
所以我们需要把Timer同时添加到`UITrackingRunLoopMode`和`kCFRunLoopDefaultMode`上。
那么如何把timer同时添加到多个mode上呢？就要用到`NSRunLoopCommonModes`了

``` 
[[NSRunLoop currentRunLoop] addTimer:timer forMode:NSRunLoopCommonModes];
```

Timer就被添加到多个mode上，这样即使RunLoop由`kCFRunLoopDefaultMode`切换到`UITrackingRunLoopMode`下，也不会影响接收Timer事件