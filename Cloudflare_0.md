![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/8tGdhMKicl00icCkgQgvmTFX4a1hZtcgv0cdtjsH26d8hWOciaayyxLKveyujFJAWlZgGovf7XYnzryPDO2fOwqvw/640?wx_fmt=webp&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

**本文大纲**

> 📜 前言
> 
> ❓Cloudflare 是谁？
> 
>     公司背景
> 
>     服务概述
> 
> 🔧 核心服务
> 
>      DNS 解析
> 
>     CDN
> 
>     DDoS 防护
> 
>     SSL/TLS 安全服务
> 
>     WAF 应用防火墙
> 
>     页面规则
> 
>     网站分析
> 
> 📦 产品
> 
>     Email Routing
> 
>     Turnsile
> 
>     R2
> 
>     Workers
> 
>     Workers KV
> 
>     D1
> 
>      Pages
> 
>     Tunnel
> 
>     ZeroTrust
> 
>     Workers AI
> 
>     AI Gateway
> 
>     Zaraz
> 
> 🌟 优势与特点
> 
>     安全
> 
>     便宜量大
> 
> 📝 总结
> 
> 🧑‍💻 更新日志
> 
>     2024.06.10

📜 前言
-----

博主不知不觉中用了很多 Cloudflare 的服务，但是发现还有很多人不知道赛博佛祖 Cloudflare，索性开个专栏介绍 Cloduflare 的产品和服务，这篇是 Cloudflare 简介篇，也可以看作一个目录后续会持续更新，那我们开始吧。

* * *

❓Cloudflare 是谁？
---------------

### 公司背景

Cloudflare由Matthew Prince、Lee Holloway和Michelle Zatlyn于2009年共同创立，总部位于美国旧金山。Cloudflare是一家提供**网络性能和安全服务**的公司，其使命是帮助建设一个更好的互联网。Cloudflare通过其全球分布的网络基础设施，提供多种服务，包括**内容分发网络 (CDN)、DDoS防护、DNS管理、SSL/TLS加密、Web应用防火墙 (WAF)** 等。这些服务旨在提升网站的性能、安全性和可靠性。**Cloudflare 拥有遍布全球250多个城市的网络,据称可以覆盖全球95%的互联网用户。**经过十几年的不断努力，Cloudflare 也在 2019 年与纽交所成功上市（虽说 22 年以来股价有点惨不忍睹...）

### 服务概述

Cloudflare提供内容分发网络 (CDN)、DDoS防护、DNS管理、SSL/TLS加密、Web应用防火墙 (WAF)等核心服务，2020 年以来还推出了许多的新产品，例如对象存储 R2，KV 存储 workers kv，边缘服务器 Workers 等。这些产品广受开发者的好评，使用Cloudflare生态的产品，可以轻松构建安全、可靠的现代web应用

🔧 核心服务
-------

### DNS 解析

我们可以在 Cloudflare 买域名自动托管在 Cloudflare 的 DNS，也可以在别的域名服务商购买域名，再迁移到 Cloudflare 里 ，相比其他厂商，Cloudflare 的 DNS 强大之处有以下几点：

*   更快的解析速度。Cloudflare的DNS 解析速度非常快，通常在全球范围内的 DNS 性能测试中名列前茅。其 Anycast 网络和高效的缓存机制使得 DNS 查询响应时间极短，显著提升了用户访问网站的速度。
    
*   更高的安全性。Cloudflare 在 DNS安全性方面投入了大量资源，提供DNSSEC、DDoS防护、DoH和DoT等高级安全功能，这些都是许多其他DNS服务提供商无法完全匹配的。也就是说如果你用了他家的 DNS，直接就给你 DDoS 防护了，非常香！
    
*   更好的隐私保护。Cloudflare的1.1.1.1公共DNS解析服务强调隐私保护，承诺不记录用户数据
    
*   更好的服务生态。能跟自家 WAF、CDN 等服务无缝对接
    

顺便提一下 Cloudflare 本身也是域名服务商，你可以在 Cloudflare 购买域名无缝对接其 DNS 服务。Cloudflare 的域名有个特点，不做活动，但是续费也不涨价，例如你在 Cloudflare 第一年花 $10购买了一个域名，后续每年续费都是这个价格，不用担心被域名服务商敲诈 : )

### CDN

Cloudflare的内容分发网络 (CDN) 服务是其核心服务之一，旨在加速网站和应用程序的交付，提升用户体验。个人认为，Cloudflare 相较于其他CDN服务提供商的优势如下：

*   Argo 智能路由：通过实时分析互联网流量和网络状况，选择最快、最可靠的路径传输数据。这种技术确保了数据传输的低延迟和高可靠性，即使在网络拥堵或出现故障的情况下也能保持性能稳定。
    
*   缓存个性化设置：你可以配置不同的缓存设置，例如缓存级别、到期时间、缓存关键资源等。
    
*   边缘证书：确保你的网站使用SSL/TLS加密。Cloudflare提供免费的边缘证书，自动管理SSL/TLS配置，确保数据传输的安全
    
*   可与任何对象存储方案集成
    

### DDoS 防护

DDoS 防护也是 Cloudflare 的核心服务卖点之一，凭借遍布全球的服务器，防护能力 max，优势如下：

*   无缝集成的生态：如果你用了 CDN 或者 DNS 服务，甚至不知道原来 Cloudlfare 自动就给你套了层 DDoS 防护
    
*   多层防护策略：Cloudflare 提供 L4 （传输层）和 L7 （应用层）防护
    
*   自适应速率限制：Cloudflare的速率限制功能可以根据流量特征动态调整，防止恶意流量洪水淹没服务器。用户可以根据具体需求设置速率限制策略，进一步提高防护效果。
    

### SSL/TLS 安全服务

相比其他厂商优势Cloudflare 如下：

*   自动化 SSL 证书管理。Cloudflare提供自动化的SSL证书管理，简化了证书申请、安装和更新的过程。用户无需手动处理复杂的证书管理任务，Cloudflare会自动处理所有细节，确保网站始终使用最新的证书。
    
*   **免费 SSL/TLS 证书** Cloudflare 为每位客户提供免费的最高 15 年的 SSL 证书！这使得任何网站都可以轻松实现HTTPS加密，这对小型网站和个人用户特别有利，他们无需支付额外费用即可享受SSL/TLS加密。
    
*   灵活的 SSL/TLS 配置。这点后续的文章会详细介绍
    
*   TLS 1.3 支持。Cloudflare是最早支持TLS 1.3协议的提供商之一。TLS 1.3相比之前的版本，提供了更高的安全性和更快的握手速度，提升了用户体验和数据保护水平。
    
*   自动 HTTPS 重写。Cloudflare提供自动HTTPS重写功能，可以将所有HTTP请求自动重定向到HTTPS。这帮助用户轻松迁移到HTTPS，确保所有流量都经过加密传输。
    

### WAF 应用防火墙

Cloudflare的应用防火墙可以防护常见的Web攻击,如SQL注入、跨站脚本、敏感信息泄露等。它支持以下功能：

*   自定义规则。**免费版能配置5条规则**，例如可以配置放行一些搜索引擎爬虫
    
*   速率限制。**免费版能配置1条规则**，可以限制某些api的访问速率。
    
*   安全事件分析。Cloudflare提供面板分析经过 WAF 的流量都是什么请求
    

### 页面规则

Cloudflare 作为客户端和服务器之间的中间层，还提供自定义的缓存和页面重定向规则

### 网站分析

Cloudflare 提供简单的 Http 流量、网站性能等内容的分析，还可以查看流量的来源地

📦 产品
-----

Email Routing

![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/8tGdhMKicl03K3CMgdaSvxD6YyKhN8b4bgse5WnXRqgUQb7j6oYAvvJZdw180BZGyuEyq1srFXfLa6r16OajHIQ/640?wx_fmt=webp&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

通过电子邮件路由，你可以将自定义的域名邮箱接收的邮件转发到你个人邮箱里

### Turnsile

Turnstile 可以嵌入到任何网站中，无需通过 Cloudflare 发送流量，并且无需向访问者显示验证码即可完成验证。一图胜千言，相信大家上网冲浪时都遇到过 Cloudflare 的这个标志

![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/8tGdhMKicl03K3CMgdaSvxD6YyKhN8b4bv4Xlf3vXTrzNXLiaDny1vqlIQibbTbCicVm3N0FX3kNejdu2Hia7bJH6mA/640?wx_fmt=webp&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

R2

![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/8tGdhMKicl03K3CMgdaSvxD6YyKhN8b4bey1pKNvq5Oc2YCDeWNNXO88xoCX59iaHUDjEGvKlWPA844AH306Lgvg/640?wx_fmt=webp&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

Cloudflare 提供的对象存储服务，免费计划的用户有 **10G 的对象存储空间**，**A 类操作 100 万次请求/月，B 类操作 1000 万次请求/月**。A 类操作通常指那些相对较少使用但计算成本较高的 API 调用（例如创建 bucker, 增加对象，删除对象等），B 类操作指读取对象。总之个人使用绰绰有余

Workers

![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/8tGdhMKicl03K3CMgdaSvxD6YyKhN8b4b1a2TnUT64XtUl4Db84EWeoOWAWeScliafMN1GkklGicJc6wqEDRZYq6g/640?wx_fmt=webp&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

Cloudflare Workers 是 Cloudflare 提供的一种 Serverless 计算平台。它允许开发者在 Cloudflare 的边缘网络上运行 JavaScript 代码，无需管理传统的服务器基础设施。你可以在 Workers 做包括但不限于如下事情：

*   代理转发
    
*   集成第三方服务
    
*   边缘计算
    
*   请求鉴权
    
*   批量重定向
    
*   缓存 Post 请求
    
*   ........
    

免费计划里包括：**10 万次请求/天，每个请求最大占用 10ms CPU 时间，最多 100 个 Worker 脚本**

Workers KV

![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/8tGdhMKicl03K3CMgdaSvxD6YyKhN8b4bBHXatO8ibjwapdBvXlyObrxhZR8B68jKBqTN7Cxnb5wSkYyzqqlPUIw/640?wx_fmt=webp&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)  

Cloudflare Workers KV（Key-Value）是一个全球分布式、低延迟的键值数据存储系统，专门设计用于与 Cloudflare Workers 配合使用。它提供了一种在边缘存储和访问数据的简单方法，你可以把它当缓存使用，免费计划里包括：**1GB 键值存储空间，10 万次读取/天，1000 次写入/每天，1000 次删除/天**

D1

![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/8tGdhMKicl03K3CMgdaSvxD6YyKhN8b4bZW3BGd0WttOvWFgZL6zqZ8b62whAzthReHS2Otvs33ib6tSRl6tfJ9Q/640?wx_fmt=webp&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

Cloudflare D1 是 Cloudflare 提供的分布式 SQLite 数据库服务，设计用于与 Cloudflare Workers 无缝集成。

免费计划里提供 **5G 存储空间，500 万次读取/天，10 万次写入/天**

Pages

![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/8tGdhMKicl03K3CMgdaSvxD6YyKhN8b4bPjADtFhgO5xHHAaV6l8aGnVEc5HulAWE63LlK11vhfVVfnSZKVkaKw/640?wx_fmt=webp&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

Cloudflare Pages 是 Cloudflare 提供的一个静态网站托管和持续部署平台，支持常见的前端框架编写的代码部署（如 Vue, React, Nuxt, Nextjs 等）。它允许开发者直接从 Git 仓库部署网站，并提供了与其他 Cloudflare 服务（如 Workers 和 D1）的集成能力。免费计划包括：**500 次构建/月，每次构建时间最长 20 分钟，无限网站数量，无限请求数量**

Tunnel

内网穿透神器，后续有详细文章介绍

### 

ZeroTrust

Cloudflare Zero Trust 是一套全面的安全服务，旨在保护组织的应用程序、数据和设备，无论它们位于何处。这种方法基于"永不信任，始终验证"的原则。Zero Trust 提供了非常多个性化的服务，有免费也有收费的

*   Access: 为应用提供身份感知的访问控制。例如你自部署了一些应用在互联网上，你希望满足制定规则的人才能访问你的应用
    
*   Gateway: 提供 DNS 过滤和完全 web 网关、内容过滤和恶意软件防护 、加密 DNS 查询(DOH)
    
*   WARP: 加密所有流量，可与 Access 和 Gateway 集成，实现端到端安全
    

免费计划包括：**最多 50 个用户，包括基本的 Access 和 Gateway 功能**

### 

Workers AI

### 

![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/8tGdhMKicl03K3CMgdaSvxD6YyKhN8b4bF0DVXzR2Toicm1JpQg71JQR4NKVQE8wQpdRYKIESse1gqIHQE2fH4fA/640?wx_fmt=webp&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

2024 年 Cloudflare 推出的无服务器的 GPU 驱动推理平台，允许开发者在 Cloudflare 的全球网络上调用各种类型的大语言模型，目前支持的模型如下：

*   文本生成：qwen1.5 系列大模型、llama 系列大模型
    
*   语音识别：whisper
    
*   图生成文：llava-1.5-7b
    
*   Text Embedding: bge 系列模型
    
*   文生成图：stable-diffusion 系列模型
    
*   .....
    

免费计划提供：**1 万 Neurons/天的使用额度**。Neurons 是 Workers AI api 调用单位，换算过来每天大概可以生成 100-200 个 LLM 响应、500 个翻译、500 秒的语音到文本音频、10,000 个文本分类或 1,500 - 15,000 个 embedding。

如果你是 worker 付费用户，从 2024 年 4 月 1 日起，非 beta 版模型的使用将开始计费，超出免费额度的部分将按照 $0.011/1000 Neurons 计费

### 

AI Gateway

### 

![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/8tGdhMKicl03K3CMgdaSvxD6YyKhN8b4bhDG4mkskpqMrTPiafdRPIiaticicwicac2nShiaLrhnn0tnfESRWODzAmxew/640?wx_fmt=webp&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

AI 模型的 API 网关，提供和普通网关类似的功能，你可以对用户调用 API 进行跟踪分析、限流、超时重试、模型回退等操作。相比普通网关，AI Gateway 还提供 input 和 output token 的统计。目前已对接主流厂商，但是暂不支持对代理 api 的转发和统计功能。

Zaraz

![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/8tGdhMKicl03K3CMgdaSvxD6YyKhN8b4bsIGUV5KOiaEvRMeAdw6lYoUpicfcCKqcDmuPyTLHDGeT5Sr8H74lTZaQ/640?wx_fmt=webp&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

Zaraz 让你允许通过 Cloudflare 云端服务器加载第三方工具显著提升了 Web 应用程序的性能、安全性和隐私性。第三方脚本可能成为攻击者的入口点，Zaraz 允许你控制这些脚本在网站上的行为，降低了潜在的安全风险。Zaraz 还提供了对第三方工具访问个人数据的控制，帮助你更好地保护用户隐私。最常见的应用是将网页的 header 脚本放到 zaraz 上（如：Google Analytics, Umami 脚本等）

🌟 优势与特点
--------

### 

安全

安全是Cloudflare的安家立命之本，一个网站用上Cloudflare 的DNS + DDoS防护 + SSL + WAF这套组合拳基本可以高枕无忧，想要击穿Cloudflare的防护那得多有钱多闲 : ）你不用担心哪天被 DDoS 攻击后隔日收到云服务发来的天价账单

### 

便宜量大

Cloudflare的服务和产品给我第一感觉就是优质、便宜 、 量大，且不说免费的基础防护套装，R2也提供10g免费对象存储空间和免费的https流量，D1和Pages等产品个人用也绰绰有余，即便收费5刀/月的worker套餐相比动不动 20 刀/月起步还要另收流量费的 PaaS 厂商相比实在太过良心，可以说是个人开发者和独立开发的不二之选

📝 总结
-----

上面介绍了这么多产品和服务，大家也清楚为什么那么推荐使用 Cloudflare ，用一个公式表示就是：

DNS + DDoS 防护+SSL/TSL+WAF+ Workers + Pages + ..... = 免费

Cloudflare就像一位“赛博佛祖”，静静地守护着互联网，为企业和开发者提供了许多优质的服务与产品。Cloudlfare还有很多产品和玩法无法一一囊括，本文章后续也会持续汇总更新 Cloudflare 的各种产品的玩法，欢迎读者分享和订阅本文章。

* * *

🧑‍💻 更新日志
----------

### 

2024.06.10

2024 年 6 月 7 日 Cloudlfare warp 和 warp+据说集体挂掉，又少了个科学上网的办法