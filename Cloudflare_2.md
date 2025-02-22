![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/8tGdhMKicl00icCkgQgvmTFX4a1hZtcgv0cdtjsH26d8hWOciaayyxLKveyujFJAWlZgGovf7XYnzryPDO2fOwqvw/640?wx_fmt=webp&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

📜 前言
-----

现代化的网站基本要求支持 https 协议，如果你的网站不支持，浏览器还会警告用户这是不安全的网站（例如 Chrome），所以建一个网站时 SSL 证书就成了必备品。如何获取 SSL 证书也是一个问题，去年腾讯云提供一年的免费 SSL 证书，一年到期后还能继续申请，最高可申请 50 张证书，今年证书有效期就变成了 3 个月，而且还不是泛域名证书。什么意思呢？如果你给 `debill.com`域名申请了 SSL 证书，那这个证书只能给该域名使用，子域名 `blog.debill.cc` 需要申请新的证书，非常麻烦。然而我们的赛博佛祖 Cloudflare 居然提供 **15 年免费的泛域名 SSL 证书**？！今天就来介绍 Cloudflare SSL/TLS，帮大家厘清一些关键概念避免一些小坑，这些坑都是德彪我踩了很多次总结出来的，希望大家多多点赞转发支持。

🧾 教程
-----

我们进入 Cloudflare 的管理后台，选择左侧的 `Websites` 选择你的域名就会进入下面的界面，可以发现左边栏跟刚进入管理时不一样，里面有很多跟网站相关的选项我们后面的文章会讲到，今天主要关注 SSL/TLS 选项。

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

### 🔏 加密模式选择

点击 `SSL/TLS` 下的 `Overview`就会进入加密模式的选择，我们前面（[Cloudflare从入门到精通 | 1. DNS快速入手](http://mp.weixin.qq.com/s?__biz=Mzk0NTcxMzU3NQ==&mid=2247483693&idx=1&sn=4fab10bd79e211965a7b0c61d4b5863d&chksm=c31079e5f467f0f3a81e7f3079d023461232afe1462ad4af931be61ec9705b44b37d942bdc5a&scene=21#wechat_redirect)）提到，客户端流量会先打到 Cloudflare CDN 上，然后 Cloudflare 再将流量打到源站，所以 Cloudflare 提供了 4 种加密模式：

*   **Off**: 客户端到 Cloudflare，Cloudflare 到源站完全不加密。不推荐使用
    
*   **Flexible**: 客户端到 Cloudflare 加密传输，Cloudflare 到源站完全不加密。不推荐使用
    
*   **Full**: 客户端到 Cloudflare，Cloudflare 到源站加密。Cloudflare 到源站之间使用你自己提供的 SSL 证书, 但是 Cloudflare 不验证你的证书的可靠性和可用性。**推荐使用**
    
*   **Full (Stirct)：**客户端到 Cloudflare，Cloudflare 到源站完全加密。Cloudflare 到源站之间使用 Cloudflare 提供的 SSL 证书**。推荐使用**
    

我们看网站上鼠标悬浮在不同选项时的动画就能很好理解了

### 📜 边缘证书 - Edge Certifcates

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

客户端和 Cloudflare 之间的通讯可以通过 SSL 加密访问，所以有 Edge Certificates 的说法，即客户端和 Cloudflare 之间通讯使用的 SSL 证书。点击左侧的 `Edge Certifcates` 选项可以看到非常多的配置项，我们挑重点的配置讲解。 

#### 通用证书

Cloudflare 给我们提供了免费的**通用证书（Universal SSL）**也允许付费计划用户上传自己的边缘证书，如果没有特别需求，我们直接用通用证书即可，你域名托管在 Cloudflare 后选择 Full 或者 Full (Stirct)就会自动帮你配置。免费计划需要注意以下几点：

*   只支持根域名和二级域名的 SSL 证书，多级域名证书不支持，要升级得付钱。例如：`blog.debill.cc` 可以使用边缘证书，`good.blog.debill.cc` 则不可以
    
*   Cloudflare 可以更改 certificate authority (CA) 而不提前通知你，如果你想选择特定 CA，要使用 advanced certificate
    

#### Always Use HTTPS

打开这个选项后，你根域名和子域名下的所有 http 请求都会被重定向成 https 请求。建议打开

#### HTTP Strict Transport Security (HSTS)

HSTS 保护 HTTPS Web 服务器免受降级攻击。这些攻击可以将请求重定向到攻击的服务器，从而导致数据安全问题。使用 HSTS 需要打开 https 加密，这个选项看你自己需要选择开关。

#### Minimum TLS Version

仅允许来自支持所选 TLS 协议版本或更高版本的访问者的 HTTPS 连接。一般推荐 TLS 1.1  以上版本

#### TLS 1.3

是否支持 TLS 1.3 协议。建议打开。

#### Automatic HTTPS Rewrites

防止“混合内容错误”（mixed content errors）。这里说一下什么是混合内容错误，假设用户使用 https 协议访问你的网页，你的网页里正好有其他资源的引用链接（常见 JavaScript 代码的引用），如果这个引用使用的是 http 协议访问，那么用户访问你网页的时候浏览器（例如Chrome）的控制台会报 “混合内容错误”。打开这个开关后，Cloudflare 会自动帮你重写网页里的引用 http 为 https，前提是该资源提供方支持 https 协议

### 📜  Origin Server

点击左侧 `Origin Server` 选项，这个页面给你提供源站 SSL 证书的地方，如果你源站没有 SSL 证书点击 Create 创建一个。

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

可以看到 Cloudflare 非常良心，提供的证书默认是支持 **泛域名证书**的，而且最长支持年限是15年，我网站挂了这个证书都没挂 🤣  

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

这里需要注意！Cloudflare 提供的 Origin Certificate **只用于 Cloudflare 回源访问，不是通用 SSL 证书**

这种方式即保障了安全性（只有 Cloudflare 能跟你源站 SSL 握手）也有一定限制，看你自己选择

🕳️ 避坑指南
--------

德彪在这个 SSL 配置上踩了不少坑，遇到好几次 SSL 握手失败，错误 5xx 等，博客都整挂了好几次 ☹️。下面是一些避坑指南：

*   如果你源站已经有 SSL 证书，没必要折腾再使用 Cloudflare 提供的 Origin Certificate，加密模式选择 `Full`就行，但你得自己解决 SSL 证书的续期问题。
    
*   如果你用了 Cloudflare 提供的 Origin Certificate 访问网站还是错误 `5xx` 将加密模式从 Full (Strict)切换到 `Full`，并检查你的 TLS 加密算法是否选择错误
    
*   如果上面的方法还是没办法解决，那就不要用 Origin Certificate
    

📝 总结
-----

Cloudflare SSL/TLS 选项的概念和配置很多，配错就肯定导致网站访问失败，需要注意以下几点：

*   加密模式的选择。一般使用 `**Full**` 或者 `**Full (Strict)**`
    
*   边缘证书使用通用证书（`**Universal SSL**`）即可，其他配置根据个人需求开关
    
*   源证书（`**Origin Certificate**`） **只用于 Cloudflare 回源访问，不是通用 SSL 证书**