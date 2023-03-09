---
id: 264
title: POJ3481 Double Queue 题解 （aka 双端优先队列）
date: "2022/12/13 22:03"
category:
    - Coding
    - OI
    - 题解
tag:
    - C++
    - OI
    - POJ
    - set
    - STL
---

题目链接：<a class="wp-editor-md-post-content-link" href="http://poj.org/problem?id=3481">POJ3481</a>

## 思路

umm
要维护一个内部有序的双端队列？

想到平衡树。

但是我几百年前写的板子早忘了。

## C++内部有序的容器——set

嗨害嗨！set是内部有序的！

如以下<del>恶臭</del>代码：

```cpp
#include <iostream>
#include <set>
using namespace std;
set<int> homo;
int main()
&#123
    homo.insert(114);
    homo.insert(1919810);
    homo.insert(114514);
    homo.insert(24);
    homo.insert(514);
    // 注意！这里是“迭代器”——STL中类对象的指针
    for (set<int>::iterator it = homo.begin(); it != homo.end(); it++) &#123
        cout << *it << endl;
    }
}
```

输出：

<pre><code class="">24
114
514
114514
1919810
</code></pre>

![](https://blog.immccn123.xyz/wp-content/uploads/2022/12/doube_queue_1-300x147.jpg)

所以说，set是内部有序的。（据说set是用平衡树实现的）

## set的常用函数（方法）

<table>
<thead>
<tr>
  <th>函数</th>
  <th>用途</th>
</tr>
</thead>
<tbody>
<tr>
  <td><code>s.insert()</code></td>
  <td>插入元素</td>
</tr>
<tr>
  <td><code>s.count()</code></td>
  <td>统计元素个数（其实只有<code>1</code>和<code>0</code>）</td>
</tr>
<tr>
  <td><code>s.empty()</code></td>
  <td>判空</td>
</tr>
<tr>
  <td><code>s.begin()</code></td>
  <td>这个set头元素的迭代器</td>
</tr>
<tr>
  <td><code>s.end()</code></td>
  <td>这个set尾元素的迭代器的后一个迭代器</td>
</tr>
</tbody>
</table>

## 优先双端队列（set实现）（雾

只有大致思路。
插入元素（<code>push</code>）：<code>s.insert(<element>);</code>
弹出最大的元素（<code>pop_max()</code>）：

```cpp
if (!s.empty()) &#123
    // 这里的--很重要，因为s.end()返回的是结尾的下一个元素的迭代器
    s.erase(--s.end());
}
```

弹出最小的元素（`pop_min()`）：

```cpp
if (!s.empty()) &#123
    s.erase(s.begin());
}
```

读最大的元素（`get_max()`）：

```cpp
// 先判空
// rbegin()是指最后一个元素的迭代器
return *(s.rbegin());
```

读最小的元素（`get_min()`）：

```cpp
// 先判空
return *(s.begin());
```

## 题解代码

```cpp
#include <iostream>
#include <set>
using namespace std;
int max_p, min_p;
// 这里是个索引，因为p是唯一的
// 相当于p->k是多对一，k->p是一对多
int p_to_k[10000007];
int opt, p, k, ed;
set<int> waiting_p;

int main()
&#123
    while (scanf("%d", &opt)) &#123
        if (opt == 0) &#123
            // 不要学我
            goto end;
        } else if (opt == 1) &#123
            scanf("%d%d", &k, &p);
            waiting_p.insert(p);
            p_to_k[p] = k;
        } else if (opt == 2) &#123
            if (!waiting_p.empty()) &#123
                ed = *(waiting_p.rbegin());
                waiting_p.erase(--waiting_p.end());
                printf("%d\n", p_to_k[ed]);
                p_to_k[ed] = 0;
            } else &#123
                printf("0\n");
            }
        } else if (opt == 3) &#123
            if (!waiting_p.empty()) &#123
                ed = *(waiting_p.begin());
                waiting_p.erase(waiting_p.begin());
                printf("%d\n", p_to_k[ed]);
                p_to_k[ed] = 0;
            } else &#123
                printf("0\n");
            }
        }
    }

end:
    return 0;
}
```

附赠题目数据构建代码（Python）：

```python
# 请搭配<https://blog.immccn123.xyz/archives/232>食用。
from random import randint
from data_io import *

data = DataIO('dbq', None)

for i in range(1, 11):
    # 用例开始
    # p -> k
    customers_p = &#123}
    customers = set()
    data.processing()
    for j in range(int(1e4 * i)):
        opt = randint(1, 3)
        if opt == 1:
            k = randint(1, int(1e6))
            p = randint(1, int(1e7))
            while p in customers_p:
                p = randint(1, int(1e7))
            customers_p[p] = k
            customers.add(p)
            data.input(f'&#123opt} &#123k} &#123p}')
        elif opt == 2:
            data.input('2')
            if len(customers) != 0:
                customers = list(customers)
                customers.sort()
                data.output(f'&#123customers_p[customers[len(customers) - 1]]}')
                del customers_p[customers[len(customers) - 1]]
                del customers[len(customers) - 1]
                customers = set(customers)
            else:
                data.output('0')
        elif opt == 3:
            data.input('3')
            if len(customers) != 0:
                customers = list(customers)
                customers.sort()
                data.output(f'&#123customers_p[customers[0]]}')
                del customers_p[customers[0]]
                del customers[0]
                customers = set(customers)
            else:
                data.output('0')
    data.input('0')
    data.next()
```