---
id: 90
title: 信息学奥赛一本通1358：中缀表达式值Python题解
publish_time: "2022/10/27 20:28"
category:
    - Coding
tag: []
---

<!-- wp:heading {"level":1} -->
<h1>题面</h1>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>输入一个中缀表达式（由0-9组成的运算数、加+减-乘*除/四种运算符、左右小括号组成。注意“-”也可作为负数的标志，表达式以“@”作为结束符），判断表达式是否合法，如果不合法，请输出“NO”；否则请把表达式转换成后缀形式，再求出后缀表达式的值并输出。</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":1} -->
<h1>题解</h1>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>注意：必须用栈操作，不能直接输出表达式的值。<br>众所周知，Python中有一个<code>exec(str)</code>的函数。它可以把<code>str</code>作为Python语句执行。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>那么。。。</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>a = "b = " + input().split("@")[0]
try:
    exec(a)
    print(b)
except:
    print("NO")</code></pre>
<!-- /wp:code -->

<!-- wp:heading -->
<h2><del>还能再短一点吗？</del></h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>经过一段时间的摸索，我发现exec似乎可以执行多行语句。</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>exec("try:\n exec("b="+input().split("@")[0]+"")\n print(b)\nexcept:\n print("NO")")</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p><strong>（仅供为了专门减少代码可读性时使用，不要在任何正式/非正式场合使用此类代码，因为这样只会减少代码的可读性，令人迷惑）</strong></p>
<!-- /wp:paragraph -->
