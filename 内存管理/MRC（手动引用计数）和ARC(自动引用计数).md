###### 三、MRC（手动引用计数）和ARC(自动引用计数)

**1、MRC：alloc，retain，release，retainCount,autorelease,dealloc**
**2、ARC：**

- ARC是LLVM和Runtime协作的结果
- ARC禁止手动调用retain，release，retainCount,autorelease关键字
- ARC新增weak，strong关键字

**3、引用计数管理：**

- alloc: 经过一系列函数调用，最终调用了calloc函数，这里并没有设置引用计数为1
- retain: 经过两次哈希查找，找到其对应引用计数值，然后将引用计数加1(实际是加偏移量)
- release：和retain相反，经过两次哈希查找，找到其对应引用计数值，然后将引用计数减1
- dealloc:

**4、弱引用管理：**

- 添加weak变量:通过哈希算法位置查找添加。如果查找对应位置中已经有了当前对象所对应的弱引用数组，就把新的弱引用变量添加到数组当中；如果没有，就创建一个弱引用数组，并将该弱引用变量添加到该数组中。

- 当一个被weak修饰的对象被释放后，weak对象怎么处理的？
清除weak变量，同时设置指向为nil。当对象被dealloc释放后，在dealloc的内部实现中，会调用弱引用清除的相关函数，会根据当前对象指针查找弱引用表，找到当前对象所对应的弱引用数组，将数组中的所有弱引用指针都置为nil。

**5、自动释放池：**

在当次runloop将要结束的时候调用objc_autoreleasePoolPop，并push进来一个新的AutoreleasePool

AutoreleasePoolPage是以栈为结点通过双向链表的形式组合而成，是和线程一一对应的。
内部属性有parent，child对应前后两个结点，thread对应线程  ，next指针指向栈中下一个可填充的位置。

- AutoreleasePool实现原理？

编译器会将 @autoreleasepool {} 改写为：

``` 
void * ctx = objc_autoreleasePoolPush;
    {}
objc_autoreleasePoolPop(ctx);
```

- objc_autoreleasePoolPush：
把当前next位置置为nil，即哨兵对象,然后next指针指向下一个可入栈位置，
AutoreleasePool的多层嵌套，即每次objc_autoreleasePoolPush，实际上是不断地向栈中插入哨兵对象。

- objc_autoreleasePoolPop:
根据传入的哨兵对象找到对应位置。
给上次push操作之后添加的对象依次发送release消息。
回退next指针到正确的位置。