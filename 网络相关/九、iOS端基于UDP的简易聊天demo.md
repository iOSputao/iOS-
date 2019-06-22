###### 九、iOS端基于UDP的简易聊天demo

**1、UdpManager**

Udp通信用C语言版和`GCDAsyncUdpSocket`都可以，封装在`UdpManager`中

- `initSocketWithReceiveHandle:(dispatch_block_t)receiveHandle`：初始化socket相关，receiveHandle是接收到消息后的回调

- `sendMessage:(NSString *)message`：发送消息

- `messageArray`:消息列表，包括接收到的和发送出去的消息

``` 
+ (void)initSocketWithReceiveHandle:(dispatch_block_t)receiveHandle;

+ (void)sendMessage:(NSString *)message;

+ (NSMutableArray *)messageArray;
```

消息内容用`MessageModel`,其中role代表消息发送对象，为0即是接收到的消息，1为自己发送的消息

``` 
@interface MessageModel:NSObject

@property (nonatomic, copy) NSString *message;

@property(nonatomic,assign) NSInteger role;

@end
```

**2、ViewController**

控制器里调用`UdpManager`初始化`socket`

``` 
[UdpManager initSocketWithReceiveHandle:^{

        dispatch_async(dispatch_get_main_queue(), ^{
            
            self.title = [NSString stringWithFormat:@"%@---%@",[[UdpManager shareManager] valueForKey:@"_destHost"],[[UdpManager shareManager] valueForKey:@"_destPort"]];
            
            [self reloadData];
        });
    }];
```

在代理方法`textFieldShouldReturn`即点击键盘的发送按钮时发送编辑好的消息

``` 
#pragma mark - UITextFieldDelegate
- (BOOL)textFieldShouldReturn:(UITextField *)textField
{
    if (self.textField.text.length == 0) return YES;
    
    [UdpManager sendMessage:self.textField.text];
    
    [self reloadData];
    
    self.textField.text = nil;
    
    return YES;
}
```

发送或者接收到新消息后都会将消息添加到`messageArray`里，并刷新页面

``` 
- (void)sendMessage:(NSString *)message
{
    NSData *sendData = [message dataUsingEncoding:NSUTF8StringEncoding];
    
    [self.messageArray addObject:[[MessageModel alloc] initWithMessage:message role:1]];
    
#ifdef UseGCDUdpSocket
    
    // 该函数只是启动一次发送 它本身不进行数据的发送, 而是让后台的线程慢慢的发送 也就是说这个函数调用完成后,数据并没有立刻发送,异步发送
    [_receiveSocket sendData:sendData toHost:_destHost port:_destPort withTimeout:60 tag:500];
    
#else
    
    sendto(_listenfd, [sendData bytes], [sendData length], 0, (struct sockaddr *)&_addr, sizeof(struct sockaddr));
    
#endif
}
```

UI就不多做介绍了，控制器里只有一个显示接收和发送消息内容列表的tableView及一个编辑消息的输入框`textField`。大概就这些内容，只是个简易的demo，只实现了接收发送文字消息的功能，并没有做更多优化

**3、测试**

分别用模拟器和真机运行，或者可以配合刚才的python程序测试.
test_host就直接用电脑ip即可

- 然后手机先发送消息到模拟器上，模拟器就可以根据记录下的手机的主机和端口回复消息了。这里手机连外网也是可以的