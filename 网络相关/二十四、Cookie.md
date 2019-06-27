###### 二十四、Cookie

这里有说到，HTTP协议是无状态的，服务器中没有保存客户端的状态，客户端必须每次带上自己的状态去请求服务器
基于HTTP这种特点，就产生了`cookie/session`

**1、用户与服务器的交互：Cookie**

`cookie`主要是用来记录用户状态，区分用户，**状态保存在客户端**。

* 1.首次访问`amazon`时，客户端发送一个HTTP请求到服务器端 。服务器端发送一个HTTP响应到客户端，其中包含`Set-Cookie`头部
* 2.客户端发送一个HTTP请求到服务器端，其中包含`Cookie`头部。服务器端发送一个HTTP响应到客户端
* 3.隔段时间再去访问时，客户端会直接发包含`Cookie`头部的HTTP请求。服务器端发送一个HTTP响应到客户端

cookie技术有4个组件：

* 1.在HTTP响应报文中的一个`cookie`首部行
* 2.在HTTP请求报文中的一个`cookie`首部行
* 3.在用户端系统中保留一个`cookie`文件，并由用户的浏览器进行管理
* 4.位于Web站点的一个后端数据库

也就是说，`cookie`功能需要浏览器的支持。如果浏览器不支持`cookie`（如大部分手机中的浏览器）或者把`cookie`禁用了，`cookie`功能就会失效。

**2、cookie的修改和删除**

在修改`cookie`的时候，只需要新`cookie`覆盖旧`cookie`即可，在覆盖的时候，由于`Cookie`具有不可跨域名性，注意`name、path、domain`需与原`cookie`一致
删除`cookie`也一样，设置`cookie`的过期时间`expires`为过去的一个时间点，或者`maxAge = 0`(Cookie的有效期,单位为秒)即可

**3、cookie的安全**

事实上，`cookie`的使用存在争议，因为它被认为是对用户隐私的一种侵害，而且`cookie`并不安全
HTTP协议不仅是`无状态`的，而且是`不安全`的。使用HTTP协议的数据不经过任何加密就直接在网络上传播，有被截获的可能。使用HTTP协议传输很机密的内容是一种隐患。

* 如果不希望`Cookie`在HTTP等非安全协议中传输，可以设置Cookie的`secure`属性为`true`。浏览器只会在HTTPS和SSL等安全协议中传输此类Cookie。
* 此外，`secure`属性并不能对Cookie内容加密，因而不能保证绝对的安全性。如果需要高安全性，需要在程序中对Cookie内容加密、解密，以防泄密。
* 也可以设置`cookie`为**HttpOnly**，如果在cookie中设置了**HttpOnly**属性，那么通过js脚本将无法读取到`cookie`信息，这样能有效的防止**XSS（跨站脚本攻击）**攻击
