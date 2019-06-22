#### 六、UI绘制原理

异步绘制：
[self.layer.delegate displayLayer: ]
代理负责生成对应的bitmap
设置该bitmap作为该layer.contents属性的值