---
id: 172
title: MySQL分表——杂七杂八的事
date: "2022/11/11 21:48"
category:
    - Coding
tag: []
---

这是一篇水文，不要借鉴这篇文章。

<!-- wp:heading &#123"level":3} -->
<h3>Hash 分表</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>简单来说，Hash分表就是基于哈希值的分表操作。比如，用户账密信息分表：（保证username唯一）</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>userTable = md5(username)[0] # 获取用户名哈希值第一位
user = query(table = f"user_&#123userTable}", &#123"where": [&#123 # 查询用户
             "username": username,
             "password": saltedPassword
}]})</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>如果要实现邮箱/手机号登陆，应增加索引表。<br>重置密码可以要求再输入一次用户名，<s>以保证安全</s>节约数据库资源。</p>
<!-- /wp:paragraph -->

<!-- wp:heading &#123"level":3} -->
<h3>ID前缀/后缀分表</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>假如你有一亿条信息需要存储。首先，你需要为此单独新建一个数据库；然后，为每一条记录分配一个整数ID。</p>
<!-- /wp:paragraph -->

<!-- wp:table &#123"className":"is-style-regular"} -->
<figure class="wp-block-table is-style-regular"><table><tbody><tr><td class="has-text-align-left" data-align="left">记录id</td><td>记录值</td></tr><tr><td class="has-text-align-left" data-align="left">1990058</td><td>[[Object]]</td></tr><tr><td class="has-text-align-left" data-align="left">1919810</td><td>[[Object]]</td></tr><tr><td class="has-text-align-left" data-align="left">11451419</td><td>[[Object]]</td></tr><tr><td class="has-text-align-left" data-align="left">52056198</td><td>[[Object]]</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p>由于众所周知的原因，数据一多，MySQL的性能就会出问题。因此，我们可以将一百万条数据分一张表存。</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table><tbody><tr><td>记录id</td><td>表名</td><td>表内数据id</td><td>记录值</td></tr><tr><td>01990058</td><td>data_1</td><td>990058</td><td>[[Object]]</td></tr><tr><td>01919810</td><td>data_1</td><td>919810</td><td>[[Object]]</td></tr><tr><td>11451419</td><td>data_11</td><td>451419</td><td>[[Object]]</td></tr><tr><td>52056198</td><td>data_52</td><td>56198</td><td>[[Object]]</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph &#123"fontSize":"medium"} -->
<p class="has-medium-font-size">就先这样吧。</p>
<!-- /wp:paragraph -->
