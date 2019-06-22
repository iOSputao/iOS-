#### 五、KVO (Key-value observing)

KVO是观察者模式的另一实现。
使用了isa混写(isa-swizzling)来实现KVO

使用setter方法改变值KVO会生效，使用setValue:forKey即KVC改变值KVO也会生效，因为KVC会去调用setter方法

``` 
- (void)setValue:(id)value
{
    [self willChangeValueForKey:@"key"];
    
    [super setValue:value];
    
    [self didChangeValueForKey:@"key"];
}
```

- 那么通过直接赋值成员变量会触发KVO吗？
不会，因为不会调用setter方法，需要加上
willChangeValueForKey和didChangeValueForKey方法来手动触发才行