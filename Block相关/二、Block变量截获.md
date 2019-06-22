###### 二、Block变量截获

**1、局部变量截获 是值截获。 比如:**

``` 
    NSInteger num = 3;
    
    NSInteger(^block)(NSInteger) = ^NSInteger(NSInteger n){
        
        return n*num;
    };
    
    num = 1;
    
    NSLog(@"%zd",block(2));
```

这里的输出是6而不是2，原因就是对局部变量num的截获是值截获。
同样，在block里如果修改变量num，也是无效的，甚至编译器会报错。

``` 
NSMutableArray * arr = [NSMutableArray arrayWithObjects:@"1",@"2", nil];
    
    void(^block)(void) = ^{
        
        NSLog(@"%@",arr);//局部变量
        
        [arr addObject:@"4"];
    };
    
    [arr addObject:@"3"];
    
    arr = nil;
    
    block();
```

打印为1，2，3
局部对象变量也是一样，截获的是值，而不是指针，在外部将其置为nil，对block没有影响，而该对象调用方法会影响

**2、局部静态变量截获 是指针截获。**

``` 
   static  NSInteger num = 3;
    
    NSInteger(^block)(NSInteger) = ^NSInteger(NSInteger n){
        
        return n*num;
    };
    
    num = 1;
    
    NSLog(@"%zd",block(2));
```

输出为2，意味着num = 1这里的修改num值是有效的，即是指针截获。
同样，在block里去修改变量m，也是有效的。

**3、全局变量，静态全局变量截获：不截获,直接取值。**

我们同样用clang编译看下结果。

``` 
static NSInteger num3 = 300;

NSInteger num4 = 3000;

- (void)blockTest
{
    NSInteger num = 30;
    
    static NSInteger num2 = 3;
    
    __block NSInteger num5 = 30000;
    
    void(^block)(void) = ^{
        
        NSLog(@"%zd",num);//局部变量
        
        NSLog(@"%zd",num2);//静态变量
        
        NSLog(@"%zd",num3);//全局变量
        
        NSLog(@"%zd",num4);//全局静态变量
        
        NSLog(@"%zd",num5);//__block修饰变量
    };
    
    block();
}
```

编译后

``` 
struct __WYTest__blockTest_block_impl_0 {
  struct __block_impl impl;
  struct __WYTest__blockTest_block_desc_0* Desc;
  NSInteger num;//局部变量
  NSInteger *num2;//静态变量
  __Block_byref_num5_0 *num5; // by ref//__block修饰变量
  __WYTest__blockTest_block_impl_0(void *fp, struct __WYTest__blockTest_block_desc_0 *desc, NSInteger _num, NSInteger *_num2, __Block_byref_num5_0 *_num5, int flags=0) : num(_num), num2(_num2), num5(_num5->__forwarding) {
    impl.isa = &_NSConcreteStackBlock;
    impl.Flags = flags;
    impl.FuncPtr = fp;
    Desc = desc;
  }
};
```

（ impl.isa = &_NSConcreteStackBlock;这里注意到这一句，即说明该block是栈block）
可以看到局部变量被编译成值形式，而静态变量被编成指针形式，全局变量并未截获。而__block修饰的变量也是以指针形式截获的，并且生成了一个新的结构体对象：

``` 
struct __Block_byref_num5_0 {
  void *__isa;
__Block_byref_num5_0 *__forwarding;
 int __flags;
 int __size;
 NSInteger num5;
};
```

该对象有个属性：num5，即我们用__block修饰的变量。
这里__forwarding是指向自身的(栈block)。
一般情况下，如果我们要对block截获的局部变量进行赋值操作需添加__block
修饰符，而对全局变量，静态变量是不需要添加__block修饰符的。
另外，block里访问self或成员变量都会去截获self。
