---
id: 222
title: 如何使用.htaccess文件阻止恶意解析的域名访问
date: "2022/11/19 09:12"
category:
    - 网安（雾
tag: []
---

<!-- wp:paragraph -->
<p>前两天，我在闲着无聊的时候给网站上了个Clarity.ms。第二天，我发现了一个奇怪的域名：</p>
<!-- /wp:paragraph -->

![](img/222-htacess-1.png)

<!-- wp:paragraph -->
<p>点进去一看，好家伙，跟我的网站一模一样。看来是被恶意解析了啊。</p>
<!-- /wp:paragraph -->
<!-- wp:paragraph -->
<p>因为前两天折腾nginx反代php老是出问题，于是经过几番思索，我到容器里面修改了.htaccess。</p>
<!-- /wp:paragraph -->
<!-- wp:paragraph -->
<p>在.htaccess文件加上后面一段：</p>
<!-- /wp:paragraph -->
<!-- wp:code -->

```
RewriteEngine On
RewriteBase /

rewritecond %&#123http_host} !^blog.immccn123.xyz$ [nc]
rewritecond %&#123http_host} !^img.immccn123.xyz$ [nc]
# Block All
rewriterule ^.* - [F,L]
```

<!-- /wp:code -->

![](img/222-htacess-2.png)

<!-- wp:paragraph -->
<p>目标达成。</p>
<!-- /wp:paragraph -->
