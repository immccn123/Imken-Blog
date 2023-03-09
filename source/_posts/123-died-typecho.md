---
id: 123
title: 记一次寄掉的Typecho
date: "2022/10/27 22:03"
category:
    - 未分类
tag: []
---

<!-- wp:paragraph -->
<p>如你所见，我正在使用WordPress作为我的博客软件。但是，一开始我相中了Typecho。为什么呢？因为它十分轻量、扩展性极强，并且支持SQLite。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>但！是！在我兴冲冲配置完LNMP环境之后，走完了Typecho的安装流程，调整了一大堆权限，做完了个性化操作，新建文章之后。。。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>怎么没反应！？？</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>重装系统试了几次，都不行。</p>
<!-- /wp:paragraph -->

<!-- wp:heading &#123"level":1} -->
<h1>寄</h1>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>根据日志的相关记录，出问题的是数据库。由于SQLite的特性，它存在一个叫做“锁（Lock）”的东西。简而言之，数据库被锁了，无法写入数据。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>但是tnnd Typecho不给我报错是几个意思啊喂！！！</p>
<!-- /wp:paragraph -->

<!-- wp:heading &#123"level":1} -->
<h1>WordPress</h1>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>于是我就用Docker部署了WordPress。</p>
<!-- /wp:paragraph -->

<!-- wp:quote &#123"className":"is-style-default"} -->
<blockquote class="wp-block-quote is-style-default"><p><s>当您看到这篇文章时，您正在运行Typecho软件。</s></p><p>世界，您好！欢迎使用WordPress。这是您的第一篇文章。编辑或删除它，然后开始写作吧！</p></blockquote>
<!-- /wp:quote -->
