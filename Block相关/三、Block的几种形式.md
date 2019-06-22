###### 三、Block的几种形式

- 分为全局Block(_NSConcreteGlobalBlock)、栈Block(_NSConcreteStackBlock)、堆Block(_NSConcreteMallocBlock)三种形式
其中栈Block存储在栈(stack)区，堆Block存储在堆(heap)区，全局Block存储在已初始化数据(.data)区

**1、不使用外部变量的block是全局block**

比如：

``` 
    NSLog(@"%@",[^{
        NSLog(@"globalBlock");
    } class]);
```

输出：

``` 
__NSGlobalBlock__
```

**2、使用外部变量并且未进行copy操作的block是栈block**

比如:

``` 
  NSInteger num = 10;
    NSLog(@"%@",[^{
        NSLog(@"stackBlock:%zd",num);
    } class]);
```

输出：

``` 
__NSStackBlock__
```

日常开发常用于这种情况:

``` 
[self testWithBlock:^{
    NSLog(@"%@",self);
}];

- (void)testWithBlock:(dispatch_block_t)block {
    block();

    NSLog(@"%@",[block class]);
}
```

**3、对栈block进行copy操作，就是堆block，而对全局block进行copy，仍是全局block**

- 比如堆1中的全局进行copy操作，即赋值：

``` 
void (^globalBlock)(void) = ^{
        NSLog(@"globalBlock");
    };

 NSLog(@"%@",[globalBlock class]);
```

输出：

``` 
__NSGlobalBlock__
```

仍是全局block

- 而对2中的栈block进行赋值操作：

``` 
NSInteger num = 10;

void (^mallocBlock)(void) = ^{

        NSLog(@"stackBlock:%zd",num);
    };

NSLog(@"%@",[mallocBlock class]);
```

输出：

``` 
__NSMallocBlock__
```

对栈blockcopy之后，并不代表着栈block就消失了，左边的mallock是堆block，右边被copy的仍是栈block
比如:

``` 
[self testWithBlock:^{
    
    NSLog(@"%@",self);
}];

- (void)testWithBlock:(dispatch_block_t)block
{
    block();
    
    dispatch_block_t tempBlock = block;
    
    NSLog(@"%@,%@",[block class],[tempBlock class]);
}
```

输出：

``` 
__NSStackBlock__,__NSMallocBlock__
```

- **即如果对栈Block进行copy，将会copy到堆区，对堆Block进行copy，将会增加引用计数，对全局Block进行copy，因为是已经初始化的，所以什么也不做。**

另外，__block变量在copy时，由于__forwarding的存在，栈上的__forwarding指针会指向堆上的__forwarding变量，而堆上的__forwarding指针指向其自身，所以，如果对__block的修改，实际上是在修改堆上的__block变量。

**即__forwarding指针存在的意义就是，无论在任何内存位置，都可以顺利地访问同一个__block变量**。

- 另外由于block捕获的__block修饰的变量会去持有变量，那么如果用__block修饰self，且self持有block，并且block内部使用到__block修饰的self时，就会造成多循环引用，即self持有block，block 持有__block变量，而__block变量持有self，造成内存泄漏。
比如:

``` 
 __block typeof(self) weakSelf = self;
    
    _testBlock = ^{
        
        NSLog(@"%@",weakSelf);
    };
    
    _testBlock();
```

如果要解决这种循环引用，可以主动断开__block变量对self的持有，即在block内部使用完weakself后，将其置为nil，但这种方式有个问题，如果block一直不被调用，那么循环引用将一直存在。
所以，我们最好还是用__weak来修饰self