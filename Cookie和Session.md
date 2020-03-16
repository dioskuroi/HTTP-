# Cookie 和 Session

## Cookie

- 通过 max-age 和 expires 可以设置 Cookie 过期时间。
- 通过设置 Secure 可以使 Cookie 只在 https 的时候发送。
- 通过设置 HttpOnly，可以禁止 javascript 通过 document.cookie 来访问 Cookie。
- 通过设置 domian，当一级域名设置了 Cookie 后，二级域名也可以访问一级域名设置当 Cookie，注意：Cookie 不能跨域设置。（比如：设置了 test.com 的 Cookie，在 a.test.com 和 b.test.com 中也能访问到 test.com 中的 Cookie）。

## 设置 Cookie

```javascript
    // Cookie 是可以同时设置多个的
    response.writeHead(200, {
        'Set-Cookie': ['id=123; max-age=2', 'abc=456; domain=test.com']
    })

```