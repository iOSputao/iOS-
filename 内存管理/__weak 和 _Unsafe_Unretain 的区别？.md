## `__weak` 和 `_Unsafe_Unretain` 的区别？

weak 修饰的指针变量，在指向的内存地址销毁后，会在 `Runtime` 的机制下，自动置为 `nil`。

`_Unsafe_Unretain`不会置为 `nil`，容易出现 `悬垂指针`，发生崩溃。但是 `_Unsafe_Unretain` 比 `__weak` 效率高。



