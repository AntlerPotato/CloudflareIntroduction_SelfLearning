![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/8tGdhMKicl00icCkgQgvmTFX4a1hZtcgv0cdtjsH26d8hWOciaayyxLKveyujFJAWlZgGovf7XYnzryPDO2fOwqvw/640?wx_fmt=webp&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

📜前言
----

Cloudflare 的很多服务需要你有域名托管在 Cloudflare DNS 系统才能使用，可以说了解 Cloudflare DNS 是使用他们其他服务的第一步， 你可以在 Cloudflare 购买域名直接使用 Cloudflare 的 DNS 服务，因为绝大多数人域名都不在 Cloudflare 购买，本文主要介绍**其他域名服务商的域名如何使用 Cloudflare DNS**。

本文以腾讯云购买的域名为例，演示如何将域名进行迁移，其他域名服务商的操作步骤雷同，也可以参考本文。

🌐 购买域名
-------

你可以在 Cloudflare 购买域名，进入后台管理界面后，点击左侧的 Domain Registration -> Register Domains, 然后在搜索框输入你想购买的域名查看价格。Cloudflare 域名不算便宜，也不做促销活动，优点在于续费价格不变，如果你打算长期经营一个域名，正好 Cloudflare 有卖可以考虑一下。顺便一嘴，Cloudflare 上没有卖这两年大热的.ai 域名，但是可以托管到 Cloudflare DNS

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/8tGdhMKicl022M6dykgSHlVEqibUmsbBtRTqE1ePXujE8dicCRsCQqKiaOgLFUUVyykbrf81bbr0eDicjDdsoL2WvoA/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

🧾教程
----

### 步骤一：注册 Cloudflare

前往 Cloudflare官网 注册与登录，登录后，点击左侧 Websites，点击页面右侧的 Add a Site

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

### 步骤二：添加域名

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

添加你在别的域名服务商购买的域名，按照提示选择 **Free plan**，无需绑定信用卡，最后会来到这样的一个界面，最下面是 cloudflare 的两个 nameservers，后面会用到。不清楚 DNS 和 NS 关系的朋友可以参考我的博客里的这篇文章 DNS和NS的关系 (https://ljlv.site/archives/dns)

### 步骤三：更改原服务商 Nameserver

将域名直接迁移到 Cloudflare 的这种方式虽然可行，但是不少域名服务商会对这种行为进行限制（例如规定多少个月以后才能迁走），我们不采用这种方式，而是通过更改 Nameserver 实现。前往域名管理的后台，找到你的域名点击管理。此时在 DNS 解析里可以看到 DNS 服务器，点击**“修改 DNS 服务器服务器”**将服务器地址改成 Cloudflare 提供的地址，这里我已经换成了 Cloudflare 的 Nameserver，然后等待 DNS 缓存刷新就大功告成了！

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

### 步骤四：Cloudflare 添加域名解析

接下来就可以到 Cloudflare 添加域名解析，注意！这里有个特殊的地方就是下方红色方框框出来的小黄云，**默认是打开的，打开后就能自动 DDoS 防护和使用 Cloudflare CDN**。关闭的话 Cloudflare 就成了普通的 DNS，只帮你做域名映射，会导致以下问题：

*   直接IP访问：访问者将直接连接到你的原始服务器IP，而不是通过Cloudflare的网络。
    
*   失去CDN功能：无法使用Cloudflare的 CDN，可能会影响网站加载速度，特别是对远距离用户。
    
*   安全防护减弱：失去Cloudflare提供的DDoS保护、Web应用防火墙等安全功能。
    
*   流量分析受限：无法使用Cloudflare的详细流量分析工具。
    

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

### 步骤五：验证 DNS 解析是否生效

我们上 https://www.itdog.cn/ping  测试看看DNS记录是否生效，当你看到 IP 位置变成 Anycast/cloudflare.com 说明已经成功了

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

〰️ Cloudflare DNS 解析流程
----------------------

一般 DNS 服务商的域名解析流程如下图，DNS 解析后流量直接打到源站

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

Cloudflare DNS 解析则完全不同，DNS 解析后流量先打到 Cloudflare 全球 CDN 节点，然后根据你配置的页面路由规则、WAF 设置等再将满足条件的流量打到源站。

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

📝 总结
-----

上面教程的关键是**更改原域名的 Nameserver** 和添加域名解析时**打开小黄云**，还有一点需要确定你的域名 Cloudflare 是否支持解析，可以参考 Cloudflare 的  TLD 政策，Cloudflare 目前支持 200 多个顶级域名，个人经验来看，一些免费的域名可能Cloudflare 无法解析，但付费域名基本都能让 Cloudflare DNS 解析。

总结为以下几点：

*   更改原域名的 Nameserver
    
*   打开小黄云
    
*   参考 Cloudflare 的  TLD（https://www.cloudflare.com/tld-policies/） 政策确定Cloudflare 是否支持解析该域名
    
*   上 https://www.itdog.cn/ping 验证是否生效
    

🐣 彩蛋
-----

有些域名在境内会被墙，例如 vercel.app, workers.dev 等域名，你可以用一个自己的域名在Cloudflare DNS 里添加一个 cname 记录绕过这一层限制 : ) 这里就不展开讲了，你可以自己实操试一下