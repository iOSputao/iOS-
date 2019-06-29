iOS面试题持续更新中！记得star一下哦！

如果你也有遇到什么难题，或者对于行业的困惑，欢迎加入iOS开发者交流群：551346706 ！随时欢迎你的到来！

![如果对你有帮助，希望你们记得给这个小哥哥一点点辛苦费！](https://upload-images.jianshu.io/upload_images/8654141-b42889f31de20789.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
谢谢你的打赏！

#### Objective_C语言特性
[（戳这里跳转到Githup）](https://github.com/iOSputao/iOS-/tree/master/Objective_C%E8%AF%AD%E8%A8%80%E7%89%B9%E6%80%A7)
>* 分类
>* 扩展
>* 代理（Delegate）
>* 通知（NSNotification）
>* KVO (Key-value observing)
>* KVC(Key-value coding)
>* 属性关键字

#### runloop
[（戳这里跳转到Githup）](https://github.com/iOSputao/iOS-/tree/master/Runloop)

>* RunLoop概念
>* RunLoop的数据结构
>* RunLoop的Mode
>* RunLoop的实现机制
>* RunLoop与NSTimer
>* RunLoop和线程
>* 讲一下 Observer ？
>* autoreleasePool 在何时被释放？
>* 解释一下 事件响应 的过程？
>* 解释一下 手势识别 的过程？
>* 解释一下 GCD 在 Runloop 中的使用？
>* 解释一下 NSTimer。
>* AFNetworking 中如何运用 Runloop?
>* PerformSelector 的实现原理？
>* 利用 runloop 解释一下页面的渲染的过程？
>* 如何使用 Runloop 实现一个常驻线程？这种线程一般有什么作用？
>* 为什么 NSTimer 有时候不好使？
>* PerformSelector:afterDelay:这个方法在子线程中是否起作用？为什么？怎么解决？
>* 什么是异步绘制？
>* 分类和类拓展的区别?

#### runtime
[（戳这里跳转到Githup）](https://github.com/iOSputao/iOS-/tree/master/runtime)

>* objc在向一个对象发送消息时，发生了什么？
>* objc中向一个nil对象发送消息将会发生什么？
>* objc中向一个对象发送消息[obj foo]和objc_msgSend()函数之间有什么关系？
>* 什么时候会报unrecognized selector的异常？
>* 能否向编译后得到的类中增加实例变量？能否向运行时创建的类中添加实例变量？为什么？
>* 给类添加一个属性后，在类结构体里哪些元素会发生变化？
>* 一个objc对象的isa的指针指向什么？有什么作用？
>* [self class] 与 [super class]
>* runtime如何通过selector找到对应的IMP地址？
>* _objc_msgForward函数是做什么的，直接调用它将会发生什么？
>*  runtime如何实现weak变量的自动置nil？知道SideTable吗？
>* isKindOfClass 与 isMemberOfClass
>* 使用runtime Associate方法关联的对象，需要在主对象dealloc的时候释放么？
>* 什么是method swizzling（俗称黑魔法)
>* Compile Error / Runtime Crash / NSLog…?
>* 实例对象的数据结构？
>* 类对象的数据结构？
>* 元类对象的数据结构?
>* Category 的实现原理？
>* 如何给 `Category` 添加属性？关联对象以什么形式进行存储？
>* Category 有哪些用途？
>* Category 和 Extension 有什么区别
>* 说一下 Method Swizzling? 说一下在实际开发中你在什么场景下使用过?
>* 如何实现动态添加方法和属性？
>* 说一下对 `isa` 指针的理解， 对象的`isa` 指针指向哪里？`isa` 指针有哪两种类型？
>* Obj-C 中的类信息存放在哪里？
>* 一个 NSObject 对象占用多少内存空间？
>* 说一下对 class_rw_t 的理解？
>* 说一下对 class_ro_t 的理解？
>* 分类和类拓展的区别?
>* 如何运用 Runtime 字典转模型？
>* 如何运用 Runtime 进行模型的归解档
>* 在 Obj-C 中为什么叫发消息而不叫函数调用？
>* 分类和类拓展的区别?
>* 说一下 Runtime 的方法缓存？存储的形式、数据结构以及查找的过程？
>* 是否了解 Type Encoding?
>* Objective-C 如何实现多重继承？
>* Category 可不可以添加实例对象？为什么？
>* Obj-c对象、类的本质是通过什么数据结构实现的？
>* Category 在编译过后，是在什么时机与原有的类合并到一起的？
>* 代码题（一）
>* 代码题（二）

#### UI相关
[（戳这里跳转到Githup）](https://github.com/iOSputao/iOS-/tree/master/UI%E7%9B%B8%E5%85%B3)

>* UIView与CALayer
>* 事件传递与视图响应链 
>* 图像显示原理
>* UI卡顿掉帧原因
>* 滑动优化方案
>* UI绘制原理
>* 离屏渲染

#### Block相关面试题
[（戳这里跳转到Githup）](https://github.com/iOSputao/iOS-/tree/master/Block%E7%9B%B8%E5%85%B3)

>* 什么是Block？
>* Block变量截获
>* Block的几种形式

#### 多线程
[（戳这里跳转到Githup）](https://github.com/iOSputao/iOS-/tree/master/%E5%A4%9A%E7%BA%BF%E7%A8%8B)

>* 进程
>*  线程
>*  进程和线程的关系
>*  多进程
>*  多线程
>* 任务
>* 队列
>* iOS中的多线程
>* GCD---队列
>* 死锁
>* GCD任务执行顺序
>* dispatch_barrier_async
>* dispatch_group_async
>* Dispatch Semaphore
>* 延时函数(dispatch_after)
>* 使用dispatch_once实现单例
>* NSOperationQueue的优点
>* NSOperation和NSOperationQueue
>* NSThread+runloop实现常驻线程
>* 自旋锁与互斥锁

#### 内存管理
[（戳这里跳转到Githup）](https://github.com/iOSputao/iOS-/tree/master/%E5%86%85%E5%AD%98%E7%AE%A1%E7%90%86)

>* 内存布局
>* 内存管理方案
>* MRC（手动引用计数）和ARC(自动引用计数)
>* 循环引用
>* 讲一下 iOS 内存管理的理解
>* 使用自动引用计数应遵循的原则
>* ARC自动内存管理的原则
>* 访问 __weak 修饰的变量，是否已经被注册在了 @autoreleasePool 中？为什么？
>* ARC 的 retainCount 怎么存储的？
>* 简要说一下 @autoreleasePool 的数据结构？
>* __weak 和 _Unsafe_Unretain 的区别？
>* 为什么已经有了 ARC ,但还是需要 @AutoreleasePool 的存在？
>* __weak 属性修饰的变量，如何实现在变量没有强引用后自动置为 nil ？
>* 说一下对 retain,copy,assign,weak,_Unsafe_Unretain 关键字的理解。
>* ARC 在编译时做了哪些工作
>* ARC 在运行时做了哪些工作？
>* 函数返回一个对象时，会对对象 autorelease 么？为什么？
>* 说一下什么是 悬垂指针？什么是 野指针?
>* 内存管理默认的关键字是什么？
>* 内存中的5大区分别是什么？
>* 是否了解 深拷贝 和 浅拷贝 的概念，集合类深拷贝如何实现
>* BAD_ACCESS 在什么情况下出现?
>* 讲一下 @dynamic 关键字？
>* autoReleasePool 什么时候释放?
>* retain、release 的实现机制？
>* 能不能简述一下 `Dealloc` 的实现机制
>* 在 MRC 下如何重写属性的 Setter 和 Getter?
>* 在 Obj-C 中，如何检测内存泄漏？你知道哪些方式？

#### 算法面试题
[（戳这里跳转到Githup）](https://github.com/iOSputao/iOS-/tree/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E4%B8%8E%E7%AE%97%E6%B3%95)

>* 不用中间变量,用两种方法交换A和B的值
>* 求最大公约数
>* 模拟栈操作
>* 排序算法
>* 折半查找（二分查找）
>* 集合结构 线性结构 树形结构 图形结构
>* 数据结构的存储
>* 单向链表\双向链表\循环链表
>* 二叉树／平衡二叉树
>* 过河经典问题，超详细解析
>* 字符串反转
>* 有序数组合并
>* HASH算法
>* 查找两个子视图的共同父视图
>* 求无序数组中的中位数

#### 网络相关
[（戳这里跳转到Githup）](https://github.com/iOSputao/iOS-/tree/master/%E7%BD%91%E7%BB%9C%E7%9B%B8%E5%85%B3)

>* 请求报文和响应报文
>* HTTP的请求方式
>* HTTP的特点
>* HTTPS和HTTP的区别
>* HTTPS的连接建立流程
>* 对称加密和非对称加密
>* 分别用C语言、python、GCDAsyncUdpSocket来实现UDP通信
>* 利用python实现Udp通信demo
>* iOS端基于UDP的简易聊天demo
>* UDP的特点
>* UDP的报文结构
>* UDP差错检测
>* TCP的特点和报文结构
>* 三次握手
>* 四次挥手
>* 可靠数据传输
>* 流量控制
>* 拥塞控制
>* DNS
>* DNS服务器
>* DNS解析过程
>* DNS记录和报文
>* DNS解析安全问题
>* Cookie
>* Session
>* Cookie 和Session 的区别：
>* 网络层和传输层的区别
>* IP协议
>* IP数据报分片
>* IPv4编址
>* IPv6数据报格式
>* 从IPv4到IPv6的迁移

#### 性能优化
[（戳这里跳转到Githup）](https://github.com/iOSputao/iOS-/tree/master/%E6%80%A7%E8%83%BD%E4%BC%98%E5%8C%96)

>* 基本概念
>* 怎么检测离屏渲染：
>* 怎么检测图层混合：
>* 光栅化
>* 入门级
>* 中级
>* 高级

#### Animation
[（戳这里跳转到Githup）](https://github.com/iOSputao/iOS-/tree/master/Animation)

>* 简要说一下常用的动画库。
>* 请说一下对 CALayer 的认识
>* CALayer 的 Contents 有几下几个主要的属性

#### 75道程序员逻辑思维面试题[（戳这里跳转到Githup）](https://github.com/iOSputao/iOS-/blob/master/75%E9%81%93%E7%A8%8B%E5%BA%8F%E5%91%98%E9%80%BB%E8%BE%91%E6%80%9D%E7%BB%B4%E9%9D%A2%E8%AF%95.md)

#### 持续更新中，记得收藏关注哦！.....

更新时间：2019年6月26号（添加网络相关面试题）

更新时间：2019年6月27号（完善网络相关面试题）

更新时间：2019年6月29号15:09（添加内存管理方面面试题）

更新时间：2019年6月29号16:10（添加Runtime相关面试题）

更新时间：2019年6月29号16:29（添加Runloop相关面试题）
