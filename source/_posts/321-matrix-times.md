---
id: 321
title: 矩阵乘法递推加速
date: "2023/1/16 11:51"
category:
    - OI
tag:
    - 矩阵乘法
    - OI
    - 算法
    - 递推
---

警告，本文KaTex排版较多，请耐心等待页面加载完成再浏览。

如果有不想看的章节，请查看左侧 TOC 跳过。

## 0x00 什么是矩阵乘法

这是一个 $\rm 2\times 3$ 的矩阵。

$\rm A =\begin{bmatrix}a_{1,1} & a_{1,2} & a_{1,3} \cr a_{2,1} & a_{2,2} & a_{2,3}\end{bmatrix}$

这是另一个 $\rm 3\times 4$ 的矩阵。

$\rm B =
\begin{bmatrix}
b_{1,1} & b_{1,2} & b_{1,3} & b_{1,4} \cr
b_{2,1} & b_{2,2} & b_{2,3} & b_{2,4} \cr
b_{3,1} & b_{3,2} & b_{3,3} & b_{3,4}
\end{bmatrix}$

矩阵乘法的定义是，对于两个矩阵 $\rm m \times p$ 的矩阵 $A$ 和 $\rm p \times n$ 的矩阵 $B$，令矩阵 $C = A\times B$，则矩阵 $C$ 中的第 $i$ 行 $j$ 列的元素可以表示为：

$
C_{ij} = \sum\limits_{k = 1}^{p} a_{ik}\times b_{kj}
$

最后乘出来的矩阵是一个 $\rm m \times n$ 的矩阵。

用稍微通俗一点的语言说，就是对于矩阵 $C$ 的第 $i$ 行 $j$ 列的元素值就是 $A$ 矩阵第 $i$ 行的元素和 $B$ 矩阵第 $j$ 列元素一一对应相乘的和。

如果你看不明白，那我就以矩阵 $C \rm(2 \times 4)$ 的一个元素（$c_{1,2}$）举个例子。
~~长公式不想打了啊啊啊~~

$$
c_{1,2} = \sum\limits_{k = 1}^{p} a_{1,k}\times b_{k,2} = a_{1,1}\times b_{1,2} + a_{1,2}\times b_{2,2} + a_{1,3}\times b_{3,2}
$$

![](img/321-matrix-times-1.jpg)

好，现在你知道什么是矩阵乘法了。尝试一下 [Hydro H1005.【模板】矩阵乘法](https://hydro.ac/p/H1005)。

[collapse title="注意" color="red"]
注意取模。
<code>tp = ((tp + (a[i][k] % mod) * (b[k][j] % mod) + mod) % mod) % mod;</code>
[/collapse]


## 0x01 矩阵乘法和递推式之间的关系

先来看一道题：[洛谷 P1962 斐波那契数列](https://www.luogu.com.cn/problem/P1962)

这个东西不就是暴力循环就能拿分的题吗？

如果你是这么想的，那么就去逝逝罢，$\sf\color{ #fadb14}60\space \color{white}\colorbox{ #e74c3c}{Unaccpted}$。

$1\le n < 2^{63}$ 的范围不卡死你才怪。

接下来的思路有点奇妙。

记斐波那契数列第 $n$ 项为 $F_n$。
我们假定一个矩阵：

$\begin{bmatrix}
F_n & F_{n - 1}
\end{bmatrix}$

根据斐波那契数列的递推式，$F_n = F_{n - 1} + F_{n - 2}$。

我们希望有一个矩阵 $\rm base$，达到如下效果：

$\begin{bmatrix}
F_n & F_{n - 1}
\end{bmatrix} =
\begin{bmatrix}
F_{n - 1} & F_{n - 2}
\end{bmatrix}
\times \rm base
$

此时 $\rm base =
\begin{bmatrix}
1 & 1 \cr
1 & 0
\end{bmatrix}$。

因此，只需要把 $\rm base$ 推出来就可以了。

![](img/321-matrix-times-2.jpg)

又由于矩阵乘法有结合律（在此不做证明），故可以将 $F_n$ 表示如下：

$\begin{aligned}
\begin{bmatrix}
F_n & F_{n - 1}
\end{bmatrix} &=
\begin{bmatrix}
F_{n - 1} & F_{n - 2}
\end{bmatrix}\times \rm base\cr
&= \begin{bmatrix}
F_{n - 2} & F_{n - 3}
\end{bmatrix}
\times\rm base \times\rm base\cr &=\cdots\cr&=
\begin{bmatrix}
F_1 & F_2
\end{bmatrix}
\times \rm base^{n - 1}\cr &=
\begin{bmatrix}
1 & 1
\end{bmatrix}
\times\rm base^{n - 1}
\end{aligned}$

### 矩阵/递推式练习

做几道练习题感受一下：（递推式转矩阵）

$F_n = 3F_{n-1} + 2F_{n-2}$

[collapse title="答案" color="green"]$\begin{bmatrix}
F_n & F_{n - 1}
\end{bmatrix} = \begin{bmatrix}
F_{n - 1} & F_{n - 2}
\end{bmatrix}
\times\begin{bmatrix}
3 & 1 \cr
2 & 0
\end{bmatrix}$[/collapse]

$F_n = 15F_{n-1} - 2F_{n-2}$

[collapse title="答案" color="green"]$\begin{bmatrix}
F_n & F_{n - 1}
\end{bmatrix} =\begin{bmatrix}
F_{n - 1} & F_{n - 2}
\end{bmatrix} \times
\begin{bmatrix}
15 & 1 \cr
-2 & 0
\end{bmatrix}$[/collapse]

$F_n = 3F_{n-1} + 2F_{n-2} + F_{n-3}$

[collapse title="答案" color="green"]$\begin{bmatrix}
F_n & F_{n - 1} & F_{n-2}
\end{bmatrix} = \begin{bmatrix}
F_{n - 1} & F_{n - 2} & F_{n-3}
\end{bmatrix} \times \begin{bmatrix}
3 & 1 & 0 \cr
2 & 0 & 1 \cr
1 & 0 & 0
\end{bmatrix}$[/collapse]

$F_n = 3F_{n-1} + 2F_{n-2} + F_{n-3}$

[collapse title="答案" color="green"]
$\begin{bmatrix}
F_n & F_{n - 1} & F_{n-2}
\end{bmatrix} =
\begin{bmatrix}
F_{n - 1} & F_{n - 2} & F_{n - 3}
\end{bmatrix} \times \begin{bmatrix}
3 & 1 & 0 \cr
2 & 0 & 1 \cr
1 & 0 & 0
\end{bmatrix}$
[/collapse]

$F_n = 3F_{n-1} + 2F_{n-2} + 114514$

[collapse title="答案" color="green"]$\begin{bmatrix}
F_n & F_{n - 1} & 114514
\end{bmatrix} = \begin{bmatrix}
F_{n - 1} & F_{n - 2} & 114514
\end{bmatrix} \times
\begin{bmatrix}
3 & 1 & 0 \cr
2 & 0 & 0 \cr
1 & 0 & 1
\end{bmatrix}$[/collapse]

还可以将多个递推式合成一个矩阵。

$F_n = F_{n-1} + F_{n-2}$

$S_n = S_{n-1} + F_n$

[collapse title="答案" color="green"]

$S_n = S_{n-1} + F_n = S_{n-1} + F_{n-1} + F_{n-2}$

$\begin{bmatrix}
F_n & F_{n - 1} & S_n
\end{bmatrix} = \begin{bmatrix}
F_{n - 1} & F_{n - 2} & S_{n-1}
\end{bmatrix} \times
\begin{bmatrix}
3 & 1 & 1 \cr
2 & 0 & 1 \cr
1 & 0 & 1
\end{bmatrix}$[/collapse]

<h2>0x03 矩阵快速幂</h2>

现在我们已经推出了 $\begin{bmatrix}
F_n & F_{n - 1}
\end{bmatrix} = \begin{bmatrix}
1 & 1
\end{bmatrix} \times base^{n - 1}
$。

这么做的时间复杂度还是 $\rm O(n)$（甚至更高），因此我们需要特殊手段进行优化。

这时候，我们就可以搬出矩阵快速幂了。矩阵快速幂与整数快速幂思路基本相同，唯一的不同就是将整数换成了矩阵。

模板题：<a class="wp-editor-md-post-content-link" href="https://www.luogu.com.cn/problem/P3390">洛谷 P3390 【模板】矩阵快速幂</a>

建议封装一下矩阵结构体/类，这样写起来方便一点。注意取模。

总体时间复杂度：$\rm O(\log n)$

## 0x04 例题

<table>
<thead>
<tr>
  <th align="center">题目编号</th>
  <th align="center">名称</th>
  <th align="center">难度</th>
</tr>
</thead>
<tbody>
<tr>
  <td align="center"><a class="wp-editor-md-post-content-link" href="https://www.luogu.com.cn/problem/P1962">P1962</a></td>
  <td align="center">斐波那契数列</td>
  <td align="center">$\colorbox{ #ffc116}{\color{white}普及/提高-}$</td>
</tr>
<tr>
  <td align="center"><a class="wp-editor-md-post-content-link" href="https://www.luogu.com.cn/problem/P1349">P1349</a></td>
  <td align="center">广义斐波那契数列</td>
  <td align="center">$\colorbox{ #52c41a}{\color{white}普及+/提高}$</td>
</tr>
<tr>
  <td align="center"><a class="wp-editor-md-post-content-link" href="https://www.luogu.com.cn/problem/P5789">P5789</a></td>
  <td align="center">[TJOI2017]可乐（数据加强版）</td>
  <td align="center">$\colorbox{ #52c41a}{\color{white}普及+/提高}$</td>
</tr>
<tr>
  <td align="center"><a class="wp-editor-md-post-content-link" href="https://www.luogu.com.cn/problem/P1707">P1707</a></td>
  <td align="center">刷题比赛</td>
  <td align="center">$\colorbox{ #3498db}{\color{white}提高+/省选-}$</td>
</tr>
<tr>
  <td align="center"><a class="wp-editor-md-post-content-link" href="https://www.luogu.com.cn/problem/P1397">P1397</a></td>
  <td align="center">[NOI2013] 矩阵游戏</td>
  <td align="center">$\colorbox{ #3498db}{\color{white}提高+/省选-}$</td>
</tr>
<tr>
  <td align="center"><a class="wp-editor-md-post-content-link" href="https://www.luogu.com.cn/problem/P1397">P2151</a></td>
  <td align="center">[SDOI2009] HH去散步</td>
  <td align="center">$\colorbox{ #3498db}{\color{white}提高+/省选-}$</td>
</tr>
<tr>
  <td align="center"><a class="wp-editor-md-post-content-link" href="https://www.luogu.com.cn/problem/P3216">P3216</a></td>
  <td align="center">[HNOI2011]数学作业</td>
  <td align="center">$\colorbox{ #3498db}{\color{white}提高+/省选-}$</td>
</tr>
<tr>
  <td align="center"><a class="wp-editor-md-post-content-link" href="https://www.luogu.com.cn/problem/P4159">P4159</a></td>
  <td align="center">[SCOI2009] 迷路</td>
  <td align="center">$\colorbox{ #9d3dcf}{\color{white}省选/NOI-}$</td>
</tr>
</tbody>
</table>

以及模板题。
