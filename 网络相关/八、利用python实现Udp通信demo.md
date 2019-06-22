###### 八、利用python实现Udp通信demo

- 创建两个python文件，分别作为客户端和服务端，然后同时运行

- 客户端

``` 
from socket import *
host = '127.0.0.1'
port = 12000
socket = socket(AF_INET, SOCK_DGRAM)
while True:
    message = raw_input('input message ,print 0 to close :\n')
    socket.sendto(message, (host, port))
    if message == '0':
        socket.close()
        break
    receiveMessage, serverAddress = socket.recvfrom(2048)
    print receiveMessage,serverAddress

```

- 服务端

``` 
from socket import *
port = 12000
socket = socket(AF_INET, SOCK_DGRAM)
socket.bind(('', port))
print 'server is ready to receive'
count = 0
while True:
    message, address = socket.recvfrom(2048)
    print address,message
    count = count + 1
    if message == '0':
        socket.close()
        break
    else:
        message = raw_input('input message ,print 0 to close :\n')
        socket.sendto(message, address)
```

- 客户端打印

``` 
/usr/local/bin/python2.7 /Users/wangyong/Desktop/other/python/UDPClient.py
input message ,print 0 to close :
hello，服务端
hello，客户端 ('10.208.61.53', 12000)
input message ,print 0 to close :
结束通信吧我们
好的 ('10.208.61.53', 12000)
input message ,print 0 to close :
0

Process finished with exit code 0
```

- 服务端打印

``` 
/usr/local/bin/python2.7 /Users/wangyong/Desktop/other/python/UDPServer.py
server is ready to receive
('10.208.61.53', 53500) hello，服务端
input message ,print 0 to close :
hello，客户端
('10.208.61.53', 53500) 结束通信吧我们
input message ,print 0 to close :
好的
('10.208.61.53', 53500) 0

Process finished with exit code 0

```