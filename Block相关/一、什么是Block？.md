###### 一、什么是Block？

Block是将函数及其执行上下文封装起来的对象。

比如：

``` 
NSInteger num = 3;
    
    NSInteger(^block)(NSInteger) = ^NSInteger(NSInteger n){
        
        return n*num;
    };
    
    block(2);
```

通过clang -rewrite-objc WYTest.m命令编译该.m文件，发现该block被编译成这个形式:

``` 
    NSInteger num = 3;

    NSInteger(*block)(NSInteger) = ((NSInteger (*)(NSInteger))&__WYTest__blockTest_block_impl_0((void *)__WYTest__blockTest_block_func_0, &__WYTest__blockTest_block_desc_0_DATA, num));

    ((NSInteger (*)(__block_impl *, NSInteger))((__block_impl *)block)->FuncPtr)((__block_impl *)block, 2);
```

其中WYTest是文件名，blockTest是方法名，这些可以忽略。
其中__WYTest__blockTest_block_impl_0结构体为

``` 
struct __WYTest__blockTest_block_impl_0 {
  struct __block_impl impl;
  struct __WYTest__blockTest_block_desc_0* Desc;
  NSInteger num;
  __WYTest__blockTest_block_impl_0(void *fp, struct __WYTest__blockTest_block_desc_0 *desc, NSInteger _num, int flags=0) : num(_num) {
    impl.isa = &_NSConcreteStackBlock;
    impl.Flags = flags;
    impl.FuncPtr = fp;
    Desc = desc;
  }
};
```

__block_impl结构体为

``` 
struct __block_impl {
  void *isa;//isa指针，所以说Block是对象
  int Flags;
  int Reserved;
  void *FuncPtr;//函数指针
};
```

block内部有isa指针，所以说其本质也是OC对象
block内部则为:

``` 
static NSInteger __WYTest__blockTest_block_func_0(struct __WYTest__blockTest_block_impl_0 *__cself, NSInteger n) {
  NSInteger num = __cself->num; // bound by copy


        return n*num;
    }
```

所以说 Block是将函数及其执行上下文封装起来的对象
既然block内部封装了函数，那么它同样也有参数和返回值。