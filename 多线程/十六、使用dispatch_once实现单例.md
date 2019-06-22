###### 十六、使用dispatch_once实现单例

``` 
+ (instancetype)shareInstance {

    static dispatch_once_t onceToken;
    
    static id instance = nil;
    
    dispatch_once(&onceToken, ^{
        
        instance = [[self alloc] init];
    });
    
    return instance;
}
```