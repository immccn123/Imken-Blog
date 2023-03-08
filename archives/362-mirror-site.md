---
id: 362
title: 如何科学地制作一个镜像反代站点
publish_time: "2023/2/27 21:28"
category:
    - 网络相关
tag:
    - Caddy
    - Nginx
    - 反代
    - 网络
---

近期情况特殊，本文已屏蔽来自中国大陆的访问。之后会解除。

请注意，本文仅供交流学习使用，任何利用此方式违反法律的行为将会收到制裁。云服务厂商知道你在干违法的事。

今天因为某些原因，vjudge.net 的服务器在大陆（尤其是我们学校）无法访问。但是我们要在那个平台上刷题。

正巧手上有台双程 CN2 GIA 的服务器（腾讯云新加坡轻量），计划着做个镜像。服务器在学校的访问速度（四川移动）还行。

## 0x00. 反代镜像的要点

1. 服务器对外对内速率都很好，要不然可能会负优化。
2. 被镜像的网站在国内的访问速度必须要慢（雾
3. 不要反代违法违规网站，要不然会被叫去喝茶。
4. 可以适当通过重写内容来阻止谷歌广告。
5. 可以通过 Caddy + Nginx 反代，Nginx 重写规则，Caddy 用来 SSL（因为 Nginx + Acme.sh 我懒得搞）

接下来上教程。

## 0x01. Caddy 的碰壁

Caddy 是一款非常方便的 Web 服务器，虽然之前测试出来效率比 Nginx 慢至少 50%，但是个人网站 Caddy 的性能已经足够客观。并且 Caddy 配置方便，例如：

```
imken.moe {
	reverse_proxy https://example.com
}
```

于是，依葫芦画瓢，镜像站上线了。

```
private.vjmirror.imken.moe {
	reverse_proxy https://vjudge.net
}
```

但是，上线测试无法登录，因为 Cookie 的 Domain 不对。
```
set-cookie: ...xxxxx=xxxxx; Domain=vjudge.net;
```

查阅资料，Caddy 无法有效删除 Cookie。所以，只能上 Nginx。

## 0x02. Nginx 反代+屏蔽部分 Cookie

先放配置文件为敬。

```nginx
server {
    # 监听端口
    listen 5663;
    # 域名
    server_name private.vjmirror.imken.moe;
	# ssl 配置，由于要过一层 Caddy，注释掉了
    # ssl on;
    # ssl_certificate /path/to/ssl/cert.pem;
    # ssl_certificate_key /path/to/ssl/private.key;
    # 反代配置
    location / {
		# 不用我说了吧
        proxy_pass https://vjudge.net;
		# 设置 Referer 标头防止被某些 WAF 察觉
        proxy_set_header   Referer https://vjudge.net;
		# 防止 Content Security Policy 阻止内容加载
        sub_filter 'http-equiv="Content-Security-Policy"' '';
		# 修改内部链接
		sub_filter 'vjudge.net' 'private.vjmirror.imken.moe';
        # 允许修改响应内容
        sub_filter_last_modified on;
        proxy_set_header Accept-Encoding deflate;
        sub_filter_types *;
        sub_filter_once on;
		# 修改响应 Cookie 的 Domain
        proxy_cookie_domain vjudge.net private.vjmirror.imken.moe;
    }
}
```

接下来是 Caddy：
```conf
private.vjmirror.imken.moe {
	# 这里添加 http:// 会导致 Caddy 不会将请求的 Host 添加到反向代理的 Host 里
	reverse_proxy localhost:5663
}
```

## 0x03. 广告屏蔽

上线之后，网站快了许多，但还是有点慢。经检查，是谷歌广告的加载很慢。

在配置文件里加两行：

```nginx
# 把规定文本改为空字符串
sub_filter 'adservice.google.com.hk' '';
sub_filter 'https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-9098591903020457' '';
```

成功。镜像站上线，速度还可以。

总的 Nginx 配置：

```nginx
server {
    # 监听端口
    listen 5663;
    # 域名
    server_name private.vjmirror.imken.moe;
	# ssl 配置，由于要过一层 Caddy，注释掉了
    # ssl on;
    # ssl_certificate /path/to/ssl/cert.pem;
    # ssl_certificate_key /path/to/ssl/private.key;
    # 反代配置
    location / {
		# 不用我说了吧
        proxy_pass https://vjudge.net;
		# 设置 Referer 标头防止被某些 WAF 察觉
        proxy_set_header   Referer https://vjudge.net;
		# 防止 Content Security Policy 阻止内容加载
        sub_filter 'http-equiv="Content-Security-Policy"' '';
		# 修改内部链接
		sub_filter 'vjudge.net' 'private.vjmirror.imken.moe';
		sub_filter 'adservice.google.com.hk' '';
		sub_filter 'https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-9098591903020457' '';
        # 允许修改响应内容
        sub_filter_last_modified on;
        proxy_set_header Accept-Encoding deflate;
        sub_filter_types *;
        sub_filter_once on;
		# 修改响应 Cookie 的 Domain
        proxy_cookie_domain vjudge.net private.vjmirror.imken.moe;
    }
}
```

