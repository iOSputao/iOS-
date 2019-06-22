###### 七、分别用C语言、python、GCDAsyncUdpSocket来实现UDP通信

**1、C语言方式**

- 首先初始化`socket`对象，Udp要用`SOCK_DGRAM`
- 然后初始化`sockaddr_in`网络通信对象，如果作为服务端要绑定`socket`对象与通信链接，来接收消息
- 然后开启一个循环，循环调用`recvfrom`来接收消息
- 收到消息后，保存下发消息对象的地址，以便之后回复消息

``` 
- (void)initCSocket
{
    
    char receiveBuffer[1024];
    
    __uint32_t nSize = sizeof(struct sockaddr);
   
    if ((_listenfd = socket(AF_INET, SOCK_DGRAM, 0)) == -1)
    {
     
        perror("socket() error. Failed to initiate a socket");
    }
    
    bzero(&_addr, sizeof(_addr));
    
    _addr.sin_family = AF_INET;
    
    _addr.sin_port = htons(_destPort);
    
    if(bind(_listenfd, (struct sockaddr *)&_addr, sizeof(_addr)) == -1)
    {
        perror("Bind() error.");
    }
    
    _addr.sin_addr.s_addr = inet_addr([_destHost UTF8String]);//ip可是是本服务器的ip，也可以用宏INADDR_ANY代替，代表0.0.0.0，表明所有地址
   
    while(true){
        
        long strLen = recvfrom(_listenfd, receiveBuffer, sizeof(receiveBuffer), 0, (struct sockaddr *)&_addr, &nSize);
        
        NSString * message = [[NSString alloc] initWithBytes:receiveBuffer length:strLen encoding:NSUTF8StringEncoding];

        _destPort = ntohs(_addr.sin_port);
        
        _destHost = [[NSString alloc] initWithUTF8String:inet_ntoa(_addr.sin_addr)];
        
        NSLog(@"来自%@---%zd:%@",_destHost,_destPort,message);
    }
}
```

- **由于开启while循环来一直接收消息，所以为了避免阻塞主线程，这里要将`initCSocket`函数放在子线程中调用**

``` 
dispatch_async(dispatch_get_global_queue(0, 0), ^{
        
        [self initCSocket];
    });
```

- 调用`sendto`方法来发送消息

``` 
- (void)sendMessage:(NSString *)message
{
    NSData *sendData = [message dataUsingEncoding:NSUTF8StringEncoding];
    
    sendto(_listenfd, [sendData bytes], [sendData length], 0, (struct sockaddr *)&_addr, sizeof(struct sockaddr));
}
```

**2、GCDAsyncUdpSocket方式**

- 首先初始化Socket对象
- 绑定端口，调用beginReceiving：方法来接收消息

``` 
- (void)initGCDSocket
{
    _receiveSocket = [[GCDAsyncUdpSocket alloc] initWithDelegate:self
                                                   delegateQueue:dispatch_get_global_queue(0, 0)];
    NSError *error;
    
    // 绑定一个端口(可选),如果不绑定端口, 那么就会随机产生一个随机的唯一的端口
    // 端口数字范围(1024,2^16-1)
    [_receiveSocket bindToPort:test_port error:&error];
    
    if (error) {
        NSLog(@"服务器绑定失败");
    }
    // 开始接收对方发来的消息
    [_receiveSocket beginReceiving:nil];
}
```

- 在代理方法里获取到对方发过来的消息，记录下主机和端口，以便之后回复消息

``` 
#pragma mark - GCDAsyncUdpSocketDelegate
- (void)udpSocket:(GCDAsyncUdpSocket *)sock didReceiveData:(NSData *)data fromAddress:(NSData *)address withFilterContext:(id)filterContext {
    
    NSString *message = [[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding];
    
    _destPort = [GCDAsyncUdpSocket portFromAddress:address];
    
    _destHost = [GCDAsyncUdpSocket hostFromAddress:address];
    
    NSLog(@"来自%@---%zd:%@",_destHost,_destPort,message);
}
```

- 调用`sendData:(NSData *)data toHost:(NSString *)host port:(uint16_t)port withTimeout:(NSTimeInterval)timeout tag:(long)tag`方法来发送消息

``` 
- (void)sendMessage:(NSString *)message
{
    NSData *sendData = [message dataUsingEncoding:NSUTF8StringEncoding];
    
    [_receiveSocket sendData:sendData toHost:_destHost port:_destPort withTimeout:60 tag:500];
}
```

**3、python方式**

python方式就比较简单了

- 初始化socket，绑定端口

``` 
socket = socket(AF_INET, SOCK_DGRAM)

socket.bind(('', port))
```

- 循环接收消息

``` 
while True:
    message, address = socket.recvfrom(2048)
    print address,message
```

- 发送消息

``` 
socket.sendto(message, address)
```