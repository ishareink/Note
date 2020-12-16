# HTTP

## HTTP HTTPS区别

(1) HTTP 是超文本传输协议，信息是明文传输，存在安全风险的问题。HTTPS 则解决 HTTP 不安全 的缺陷，在 TCP 和 HTTP 网络层之间加入了 SSL/TLS 安全协议，使得报文能够加密传输。

(2) HTTP 连接建立相对简单， TCP 三次握手之后便可进行 HTTP 的报文传输。而 HTTPS 在 TCP 三次握手之后，还需进行 SSL/TLS 的握手过程，才可进入加密报文传输。

(3) HTTP 的端口号是 80，HTTPS 的端口号是 443。

(4) HTTPS 协议需要向 CA（证书权威机构）申请数字证书，来保证服务器的身份是可信的。

## 对称加密和非对称加密

- 对称加密：密钥只有一个，加密解密为同一个密码，且加解密速度快，典型的对称加密算法有DES、AES等；
- 非对称加密：密钥成对出现（且根据公钥无法推知私钥，根据私钥也无法推知公钥），加密解密使用不同密钥（公钥加密需要私钥解密，私钥加密需要公钥解密），相对对称加密速度较慢，典型的非对称加密算法有RSA、DSA等。

## HTTPS加密算法和原理

HTTPS 采用的是对称加密和非对称加密结合的「混合加密」

**方式**： 在通信建立前采用非对称加密的方式交换「会话秘钥」，后续就不再使用非对称加密。 在通信过程中全部使用对称加密的「会话秘钥」的方式加密明文数据。 

**采用「混合加密」的方式的原因**： 对称加密只使用一个密钥，运算速度快，密钥必须保密，无法做到安全的密钥交换。 非对称加密使用两个密钥：公钥和私钥，公钥可以任意分发而私钥保密，解决了密钥交换问题但 速度慢。

## http1.0 http1.1 http1.2

HTTP1.0最早在网页中使用是在1996年，那个时候只是使用一些较为简单的网页上和网络请求上，而HTTP1.1则在1999年才开始广泛应用于现在的各大浏览器网络请求中，同时HTTP1.1也是当前使用最为广泛的HTTP协议。 主要区别主要体现在：

1. **长连接** : **在HTTP/1.0中，默认使用的是短连接**，也就是说每次请求都要重新建立一次连接。HTTP 是基于TCP/IP协议的,每一次建立或者断开连接都需要三次握手四次挥手的开销，如果每次请求都要这样的话，开销会比较大。因此最好能维持一个长连接，可以用个长连接来发多个请求。**HTTP 1.1起，默认使用长连接** ,默认开启Connection： keep-alive。 **HTTP/1.1的持续连接有非流水线方式和流水线方式** 。流水线方式是客户在收到HTTP的响应报文之前就能接着发送新的请求报文。与之相对应的非流水线方式是客户在收到前一个响应后才能发送下一个请求。
2. **错误状态响应码** :在HTTP1.1中新增了24个错误状态响应码，如409（Conflict）表示请求的资源与资源的当前状态发生冲突；410（Gone）表示服务器上的某个资源被永久性的删除。
3. **缓存处理** :在HTTP1.0中主要使用header里的If-Modified-Since,Expires来做为缓存判断的标准，HTTP1.1则引入了更多的缓存控制策略例如Entity tag，If-Unmodified-Since, If-Match, If-None-Match等更多可供选择的缓存头来控制缓存策略。
4. **带宽优化及网络连接的使用** :HTTP1.0中，存在一些浪费带宽的现象，例如客户端只是需要某个对象的一部分，而服务器却将整个对象送过来了，并且不支持断点续传功能，HTTP1.1则在请求头引入了range头域，它允许只请求资源的某个部分，即返回码是206（Partial Content），这样就方便了开发者自由的选择以便于充分利用带宽和连接。

## http状态码

- 1xx 1xx 类状态码属于提示信息，是协议处理中的一种中间状态，实际用到的比较少。
- 2xx 2xx 类状态码表示服务器成功处理了客户端的请求，也是我们最愿意看到的状态。
  - 「200 OK」是最常见的成功状态码，表示一切正常。如果是非 HEAD 请求，服务器返回的响应头都 会有 body 数据。
  - 「204 No Content」也是常见的成功状态码，与 200 OK 基本相同，但响应头没有 body 数据。 
  - 「206 Partial Content」是应用于 HTTP 分块下载或断点续传，表示响应返回的 body 数据并不是资源 的全部，而是其中的一部分，也是服务器处理成功的状态。
- 3xx 3xx 类状态码表示客户端请求的资源发送了变动，需要客户端用新的 URL 重新发送请求获取资源， 也就是重定向。 
  - 「301 Moved Permanently」表示永久重定向，说明请求的资源已经不存在了，需改用新的 URL 再次 访问。 
  - 「302 Found」表示临时重定向，说明请求的资源还在，但暂时需要用另一个 URL 来访问。 
  - 301 和 302 都会在响应头里使用字段 Location ，指明后续要跳转的 URL，浏览器会自动重定向新的 URL。 「304 Not Modified」不具有跳转的含义，表示资源未修改，重定向已存在的缓冲文件，也称缓存重定 向，用于缓存控制。
- 4xx 4xx 类状态码表示客户端发送的报文有误，服务器无法处理，也就是错误码的含义。 
  - 「400 Bad Request」表示客户端请求的报文有错误，但只是个笼统的错误。
  - 「403 Forbidden」表示服务器禁止访问资源，并不是客户端的请求出错。 
  - 「404 Not Found」表示请求的资源在服务器上不存在或未找到，所以无法提供给客户端。 
- 5xx 5xx 类状态码表示客户端请求报文正确，但是服务器处理时内部发生了错误，属于服务器端的错误 码。
  - 「500 Internal Server Error」与 400 类型，是个笼统通用的错误码，服务器发生了什么错误，我们并 不知道。 
  - 「501 Not Implemented」表示客户端请求的功能还不支持，类似“即将开业，敬请期待”的意思。
  - 「502 Bad Gateway」通常是服务器作为网关或代理时返回的错误码，表示服务器自身工作正常，访问 后端服务器发生了错误。 
  - 「503 Service Unavailable」表示服务器当前很忙，暂时无法响应服务器，类似“网络服务正忙，请稍 后重试”的意思。