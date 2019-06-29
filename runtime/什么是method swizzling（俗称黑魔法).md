**什么是method swizzling（俗称黑魔法)**

简单说就是进行方法交换

在Objective-C中调用一个方法，其实是向一个对象发送消息，查找消息的唯一依据是selector的名字。利用Objective-C的动态特性，可以实现在运行时偷换selector对应的方法实现，达到给方法挂钩的目的。
每个类都有一个方法列表，存放着方法的名字和方法实现的映射关系，selector的本质其实就是方法名，IMP有点类似函数指针，指向具体的Method实现，通过selector就可以找到对应的IMP。
换方法的几种实现方式

 - 利用 method_exchangeImplementations 交换两个方法的实现 
 - 利用 class_replaceMethod替换方法的实现 
 - 利用 method_setImplementation 来直接设置某个方法的IMP
![enter description here](./images/1561035874952.png)
