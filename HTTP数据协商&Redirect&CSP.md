# HTTP 数据协商 & Redirect & Content-Security-Policy

## 1. 概念

- 当浏览器和服务器进行通信的时候，服务器不知道需要浏览器需要什么格式的数据资源，此时浏览器和服务器就需要数据协商，来指明浏览器到底需要什么样的数据格式的资源。

------

## 2. Accept

- Accept 是请求时告诉服务器我想要什么样的数据。
- Accept：数据类型
- Accept-Encoding：代表数据是怎么样的编码方式来进行传输，主要是用来限制服务端的压缩方式。
- Accept-language：根据这个来返回数据是什么语言的
- User-Agent：客户端的一些相关的信息，用来判断需要返回移动端的页面还是 PC 端的页面。

------

## 3. Content

- Content-Type：服务端告诉客户端返回的数据格式。Content-type 的值是 MIME-types 规定的这些值。（服务端设置 `X-Content-Type-Options: nosniff` 的作用是在早期的 IE 浏览器中，如果你设置的 Content-Type 不对或者根本没有设置 Content-Type，那么它会自动预测你的数据类型，这会导致网站会受到一些攻击，比如你要的是一段文本格式的 js 代码，结果 IE 帮你自动转成了 text/javascript 格式，这个响应头就是用来限制浏览器自动预测数据类型的。）
- Content-Encoding：对应 Accept-Encoding，服务端用了什么样的压缩方式。
- Content-language：服务端返回的数据的语言

------

## 4. Redirect

- 当需要让客户端重定向时，服务端需要返回状态码`302`或者`301`，并且需要在 headers 中指定跳转的路由`Location: /new`（当不写域名时，浏览器默认为同源跳转）

------

## 5. Content-Security-Policy

> HTML中“nonce”的属性能够将某些内联script和style元素“列入白名单” ，同时避免使用CSP unsafe-inline指令（这将允许全部内联script/ style），因此仍然保留不允许内联script/ style一般的关键CSP功能。

- Content-Security-Policy(内容安全策略)：作用是限制资源的获取、报告资源获取越权。
- 限制方式：
  - default-src：限制全局跟链接请求有关东西的作用范围。
  - 制定资源类型的限制范围：connect-src（请求地址的限制）、manifest-src、img-src（图片加载地址限制）、font-src、media-src、style-src、frame-src、script-src。比如：`Content-Security-Policy: default-src http: https:; form-action \'self\'; report-uri /report`设置了所有的资源必须通过 http 或者是 https 这种外链的方式进行加载，不能使用内嵌的 script 和 style，并且还限制了表单提交的路径只能是同域的路径，这样就可以有效防止 xss 攻击。最后加上 report-uri 之后，当加载了限制以外的资源时，让浏览器自动通知服务器，报告这个情况。
- Content-Security-Policy-Report-Only：当加载了限制以外当资源时，资源还是会被加载，但是会通知服务器报告这个情况。
- 还可以在 html 的 meta 标签里写内容安全策略，如`<meta http-equiv="Content-Security-Policy" content="default-src 'self'" />` 注意：在 meta 标签中不能写 report-uri，所以我们还是推荐在 http 的 header 中写 CSP。