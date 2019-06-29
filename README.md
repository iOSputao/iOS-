#### 持续更新中，记得收藏关注哦！.....
一些面试题总结，希望对你有帮助，同时希望你能来一起完善它！
如果你也在面试中遇到什么问题，或者想要找到更多的交流资源！
iOS开发者的交流群：551346706 ！ 欢迎你的加入

#### 目录：

#### [Objective_C语言特性](https://github.com/iOSputao/iOS-/tree/master/Objective_C%E8%AF%AD%E8%A8%80%E7%89%B9%E6%80%A7)

* 一、分类
* 二、扩展
* 三、代理（Delegate）
* 四、通知（NSNotification）
* 五、KVO (Key-value observing)
* 六、KVC(Key-value coding)
* 七、属性关键字

#### [runloop](https://github.com/iOSputao/iOS-/tree/master/Runloop)

* 一、RunLoop概念
* 二、RunLoop的数据结构
* 三、RunLoop的Mode
* 四、RunLoop的实现机制
* 五、RunLoop与NSTimer
* 六、RunLoop和线程

#### [runtime](https://github.com/iOSputao/iOS-/tree/master/runtime)

* objc在向一个对象发送消息时，发生了什么？
* objc中向一个nil对象发送消息将会发生什么？
* objc中向一个对象发送消息[obj foo]和objc_msgSend()函数之间有什么关系？
* 什么时候会报unrecognized selector的异常？
* 能否向编译后得到的类中增加实例变量？能否向运行时创建的类中添加实例变量？为什么？
* 给类添加一个属性后，在类结构体里哪些元素会发生变化？
* 一个objc对象的isa的指针指向什么？有什么作用？
* [self class] 与 [super class]
* runtime如何通过selector找到对应的IMP地址？
* _objc_msgForward函数是做什么的，直接调用它将会发生什么？
*  runtime如何实现weak变量的自动置nil？知道SideTable吗？
* isKindOfClass 与 isMemberOfClass
* 使用runtime Associate方法关联的对象，需要在主对象dealloc的时候释放么？
* 什么是method swizzling（俗称黑魔法)
* Compile Error / Runtime Crash / NSLog…?
* 代码题（一）
* 代码题（二）

#### [UI相关](https://github.com/iOSputao/iOS-/tree/master/UI%E7%9B%B8%E5%85%B3)

* 一、UIView与CALayer
* 二、事件传递与视图响应链 
* 三、图像显示原理
* 四、UI卡顿掉帧原因
* 五、滑动优化方案
* 六、UI绘制原理
* 七、离屏渲染

#### [Block相关面试题](https://github.com/iOSputao/iOS-/tree/master/Block%E7%9B%B8%E5%85%B3)

* 一、什么是Block？
* 二、Block变量截获
* 三、Block的几种形式

#### [多线程](https://github.com/iOSputao/iOS-/tree/master/%E5%A4%9A%E7%BA%BF%E7%A8%8B)

* 一、 进程
* 二、 线程
* 三、 进程和线程的关系
* 四、 多进程
* 五、 多线程
* 六、任务
* 七、队列
* 八、iOS中的多线程
* 九、GCD---队列
* 十、死锁
* 十一、GCD任务执行顺序
* 十二、dispatch_barrier_async
* 十三、dispatch_group_async
* 十四、Dispatch Semaphore
* 十五、延时函数(dispatch_after)
* 十六、使用dispatch_once实现单例
* 十七、NSOperationQueue的优点
* 十八、NSOperation和NSOperationQueue
* 十九、NSThread+runloop实现常驻线程
* 二十、自旋锁与互斥锁

#### [内存管理](https://github.com/iOSputao/iOS-/tree/master/%E5%86%85%E5%AD%98%E7%AE%A1%E7%90%86)

* 一、内存布局
* 二、内存管理方案
* 三、MRC（手动引用计数）和ARC(自动引用计数)
* 四、循环引用
* 五、讲一下 iOS 内存管理的理解
* 六、使用自动引用计数应遵循的原则
* 七、ARC自动内存管理的原则
* 八、访问 __weak 修饰的变量，是否已经被注册在了 @autoreleasePool 中？为什么？
* 九、ARC 的 retainCount 怎么存储的？
* 十、简要说一下 @autoreleasePool 的数据结构？
* 十一、__weak 和 _Unsafe_Unretain 的区别？
* 十二、为什么已经有了 ARC ,但还是需要 @AutoreleasePool 的存在？
* 十三、__weak 属性修饰的变量，如何实现在变量没有强引用后自动置为 nil ？
* 十四、说一下对 retain,copy,assign,weak,_Unsafe_Unretain 关键字的理解。
* 十五、ARC 在编译时做了哪些工作
* 十六、ARC 在运行时做了哪些工作？
* 十七、函数返回一个对象时，会对对象 autorelease 么？为什么？
* 十八、说一下什么是 悬垂指针？什么是 野指针?
* 十九、内存管理默认的关键字是什么？
* 二十、内存中的5大区分别是什么？
* 二十一、是否了解 深拷贝 和 浅拷贝 的概念，集合类深拷贝如何实现
* 二十二、BAD_ACCESS 在什么情况下出现?
* 二十三、讲一下 @dynamic 关键字？
* 二十四、autoReleasePool 什么时候释放?
* 二十五、retain、release 的实现机制？
* 二十六、能不能简述一下 `Dealloc` 的实现机制
* 二十七、在 MRC 下如何重写属性的 Setter 和 Getter?
* 二十八、在 Obj-C 中，如何检测内存泄漏？你知道哪些方式？

#### [算法面试题](https://github.com/iOSputao/iOS-/tree/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E4%B8%8E%E7%AE%97%E6%B3%95)

* 1、不用中间变量,用两种方法交换A和B的值
* 2、求最大公约数
* 3、模拟栈操作
* 4、排序算法
* 5、折半查找（二分查找）
* 6、集合结构 线性结构 树形结构 图形结构
* 7、数据结构的存储
* 8、单向链表\双向链表\循环链表
* 9、二叉树／平衡二叉树
* 10、过河经典问题，超详细解析
* 11、字符串反转
* 12、有序数组合并
* 13、HASH算法
* 14、查找两个子视图的共同父视图
* 15、求无序数组中的中位数

#### [网络相关](https://github.com/iOSputao/iOS-/tree/master/%E7%BD%91%E7%BB%9C%E7%9B%B8%E5%85%B3)

* 一、请求报文和响应报文
* 二、HTTP的请求方式
* 三、HTTP的特点
* 四、HTTPS和HTTP的区别
* 五、HTTPS的连接建立流程
* 六、对称加密和非对称加密
* 七、分别用C语言、python、GCDAsyncUdpSocket来实现UDP通信
* 八、利用python实现Udp通信demo
* 九、iOS端基于UDP的简易聊天demo
* 十、UDP的特点
* 十一、UDP的报文结构
* 十二、UDP差错检测
* 十三、TCP的特点和报文结构
* 十四、三次握手
* 十五、四次挥手
* 十六、可靠数据传输
* 十七、流量控制
* 十八、拥塞控制
* 十九、DNS
* 二十、DNS服务器
* 二十一、DNS解析过程
* 二十二、DNS记录和报文
* 二十三、DNS解析安全问题
* 二十四、Cookie
* 二十五、Session
* 二十六、Cookie 和Session 的区别：
* 二十七、网络层和传输层的区别
* 二十八、IP协议
* 二十九、IP数据报分片
* 三十、IPv4编址
* 三十一、IPv6数据报格式
* 三十二、从IPv4到IPv6的迁移

#### [性能优化](https://github.com/iOSputao/iOS-/tree/master/%E6%80%A7%E8%83%BD%E4%BC%98%E5%8C%96)

* 1.基本概念
* 2.怎么检测离屏渲染：
* 3.怎么检测图层混合：
* 4.光栅化
* 5.入门级
* 6.中级
* 7.高级

#### [75道程序员逻辑思维面试题](https://github.com/iOSputao/iOS-/blob/master/75%E9%81%93%E7%A8%8B%E5%BA%8F%E5%91%98%E9%80%BB%E8%BE%91%E6%80%9D%E7%BB%B4%E9%9D%A2%E8%AF%95.md)


