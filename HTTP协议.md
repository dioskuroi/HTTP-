# HTTP协议
## 1. 概念
- HTTP流程

![HTTP流程图](/Users/xujun/Documents/学习笔记/Http协议/http_process.png)

如图所示，当在地址栏输入了URL后，浏览器会先去本地文件中寻找是否有永久重定向需求，然后在访问缓存是否可以使用，如果缓存无法使用就继续解析DNS，然后拿到服务器ip后与服务器建立tcp连接，最后进行请求和响应的操作。

---
- 网络模型

![5层网络模型](/Users/xujun/Documents/学习笔记/Http协议/five_model.png)

如图所示，网络模型分为5层。 物理层，数据链路层，网络层，传输层，应用层。

---

- 低三层

物理层主要作用是定义物理设备如何传输数据（也就是硬件设备）。
数据链路层在通信当实体间建立数据链路连接（也就是传输0101的二进制数据）。
网络层为数据在结点之间传输创建逻辑链路（寻找ip地址对应的服务器的逻辑关系）。

---

- 传输层

向用户提供可靠的端到端（End-to-End）服务。
传输层向高层屏蔽了下层数据通信的细节（我们并不需要关心数据到底在网络中怎么传输的，因为这些细节传输层已经帮我们封装好了）。

---

- 应用层

为应用软件提供了很多服务，构建与tcp协议之上，屏蔽网络传输相关细节。

---

- TCP三次握手

![TCP三次握手](/Users/xujun/Documents/学习笔记/Http协议/three_hands.jpg)

TCP三次握手是为了防止服务器开启一些无用的连接，所以需要进行三次握手来验证客户端的请求是否正常。
第一次握手客户端发送[SYN] Seq=0，SYN为标志位，
第二次握手服务端返回[SYN,ACK] Seq=0 Ack=1，SYN，ACK为标志位，Ack等于客户端的Seq+1，
第三次握手客户端发送[ACK] Seq=1 Ack=1，ACK为标志位，Seq等于服务端返回的Ack，Ack等于服务端返回的Seq+1。

## 2. URI、URL、URN
- URI
    + Uniform Resource Identifier 统一资源标志符。
    + 用来唯一标识互联网上的信息资源。
    + URI 包括了 URL 和 URN。

---

- URL
    + Uniform Resource Locator 统一资源定位符。
    + http://www.abc.com:80/path?query=data#hash

---

- URN
    + 永久统一资源定位符。
    + 在资源移动后还能被找到。目前还没有非常成熟的使用方案。

---

## 3. CORS跨域预请求

- 当利用 CORS 进行跨域请求的时候，如果不使用以下方式，需要先进行预请求（会先发送一个OPTIONS请求），服务端验证后才能发送真正的请求。（比如你需要发送一个自定义的请求头 `X-Test-Cors`，那服务端需要设置 `Access-Control-Allow-Headers: 'X-Test-Cors'`）

- 请求方法
    + 当发送跨域请求时，get、head、post默认不需要预请求。而其他当请求方式需要先发送预请求。

---

- Content-Type
    + 当发送跨域请求时，Content-Type 为 text/plain、multipart/form-data、application/x-www-form-urlencoded 时，不需要预请求。也就是说你在表单提交当时候不需要预请求，其他情况都需要预请求。

---

- 其他限制
    + 当存在自定义的请求头时，也需要先发送预请求。
    + XMlHttpRequestUpload 对象均没有注册任何事件监听器，不需要发送预请求。
    + 请求中没有使用 ReadableStream 对象，不需要发送预请求。

- 利用`Access-Control-Max-Age: 1000`可以设置当服务端验证过预请求之后，1000秒之内不需要再进行预请求，可以直接发起正式请求。