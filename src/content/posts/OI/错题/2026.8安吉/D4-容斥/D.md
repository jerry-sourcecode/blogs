---
title: 安吉Day4-D
published: 2026-08-04
updated: 2026-08-04
description: Day4-D
tags:
  - OI
  - 题目
  - 容斥
language: 中文
slug: problem/anji2026/D
series: 2026安吉
category: 题目
---

> [!NOTE]- 原题呈现
> # P10390 因数计数
>
> ## 题目描述
>
> 小蓝随手写出了含有 $n$ 个正整数的数组 $\{a_1, a_2,\cdots, a_n\}$，他发现可以轻松地算出有多少个有序二元组 $(i, j)$ 满足 $a_j$ 是 $a_i$ 的一个因数．因此他定义一个整数对 $(x_1, y_1)$ 是一个整数对 $(x_2, y_2)$ 的“因数”当且仅当 $x_1$ 和 $y_1$ 分别是 $x_2$ 和 $y_2$ 的因数．他想知道有多少个有序四元组 $(i, j, k, l)$ 满足 $(a_i
> , a_j)$ 是 $(a_k, a_l)$ 的因数，其中 $i, j, k, l$ 互不相等．
>
> ## 输入格式
>
> 输入的第一行包含一个正整数 $n$．
> 第二行包含 $n$ 个正整数 $a_1, a_2,\cdots, a_n $，相邻整数之间使用一个空格分隔．
>
> ## 输出格式
>
> 输出一行包含一个整数表示答案．
>
> ## 输入输出样例 #1
>
> ### 输入 #1
>
> ```
> 5
> 3 6 2 2 7
> ```
>
> ### 输出 #1
>
> ```
> 4
> ```
>
> ## 说明/提示
>
> 四元组 $(1, 4, 2, 3)$：$(3, 2)$ 为 $(6, 2)$ 的因子；
> 四元组 $(1, 3, 2, 4)$：$(3, 2)$ 为 $(6, 2)$ 的因子；
> 四元组 $(4, 1, 3, 2)$：$(2, 3)$ 为 $(2, 6)$ 的因子；
> 四元组 $(3, 1, 4, 2)$：$(2, 3)$ 为 $(2, 6)$ 的因子．
>
> 对于 $20\%$ 的评测用例，$n ≤ 50$；
> 对于 $40\%$ 的评测用例，$n ≤ 10^4$；
> 对于所有评测用例，$1 ≤ n ≤ 10^5 ，1 ≤ a_i ≤ 10^5$．

在题面中，出现了一个子问题：

>求出长度为 $n$ 的序列 $a$ 中有多少个有序二元组 $(i, j)$ 满足 $a_j \mid a_i$．

接下来，我们来解决这个子问题．

考虑到值域只到 $10^5$，我们考虑将值域放入数组下标，定义 $cnt[x]$ 表示序列 $a$ 中元素 $x$ 的数量，并预处理下列数据：

- $mult[x]=\sum_{y\in \{i :x\mid i \}}cnt[y]$，记录序列中是 $x$ 的倍数的数有多少个（包含 $x$）
- $divs[x]=\sum_{y\in \{i :i\mid x \}}cnt[y]$，记录序列中是 $x$ 的因数的数有多少个（包含 $x$）
这些数据可以用调和级数 $O(n\log n)$ 时间复杂度解决．

接下来，我们就可以使用 $F = \sum_{x\in\mathbb{N}} cnt[x] \times mult[x]$，来计算所有满足 $a_j \mid a_i$ 的有序二元组 $(i, j)$ 数量（包括 $i=j$）．去掉 $i=j$ 情况的方法也很简单，显然，$i=j$ 的方法有 $n$ 种，因此使用 $T = F -n$ 就可以解决上面的子问题．

那么，有序四元对的数量就是 $T^2$ 吗？显然不是．我们需要先确定一组满足子问题的有序二元对 $(i, k)$，设 $a[i] = x, a[k] = y$，由于值域小，我们可以使用枚举 $x$ 和 $y$ 的方式来确定一对**数值对**（此处不需要求出 $i$ 和 $j$ 具体是多少，因为后续只会用到 $a[i]$ 和 $a[j]$，因此所有 $a[i]$ 和 $a[j]$ 分别相等，但 $i,j$ 不分别相等的有序二元对是等价的），时间复杂度 $O(n\log n)$．显然，这一对数值对对应着 $cnt[x] \times cnt[y]$ 个有序二元对（如果 $x=y$ 则对应 $cnt[x]\times (cnt[x] - 1)$ 个）这些有序二元对是完全相同的，可以一起处理．

接下来，对于每一对合法的有序二元对 $(i,k)$，我们需要枚举另外一对合法的有序二元对 $(j,l)$，当然，需要满足下面这些条件：

- $a_j \mid a_l$ 且 $j\neq l$
- $j,l\notin \{i,k\}$

根据容斥原理，总方案数 $tot$ 有如下表达式：

$$
tot = T - A_i -A_k+B_{ik}
$$

其中：

- $T$ 为上述子问题的答案
- $A_i$ 为 $j$ 和 $l$ 中存在一个与 $i$ 相等的情况，即 $j=i$ 或 $l=i$（$j\neq l$），计算方法如下：
	- 使用 $mult[a[i]]$ 计算 $a[i]$ 的倍数有多少个，当 $a[j]$ 取到 $a[i]$ 时，$a[l]$ 的取值就一定是 $a[i]$ 的倍数，一共会有 $mult[a[i]] - 1$ 个，由于 $a[j] \neq a[l]$，所以要 -1．
	- 使用 $divs[a[i]]$ 计算 $a[i]$ 的因数有多少个，当 $a[l]$ 取到 $a[i]$ 时，$a[j]$ 的取值就一定是 $a[i]$ 的因数，一共会有 $divs[a[i]] - 1$ 个，由于 $a[j] \neq a[l]$，所以要 -1．
	- 综合一下，$A_i = mult[a[i]] + divs[a[i]] - 2$．
- $A_k$ 为 $j$ 和 $l$ 中存在一个与 $k$ 相等的情况，即 $j=k$ 或 $l=k$（$j\neq l$），计算方法同上．
- $B_{ik}$ $j$ 和 $l$ 中一个与 $i$ 相等，另一个与 $k$ 相等的情况，即 $j=i$ 且 $l=k$；或 $j=k$ 且 $l=i$
	- 对于 $j=i$ 且 $l=k$ 的情况，由于 $a_i \mid a_k$，故 $a_j \mid a_l$ 恒成立，因此，这种情况一定会有一个贡献为 1．
	- 对于 $j=k$ 且 $l=i$ 的情况，由于 $a_i \mid a_k$，当且仅当 $a_j=a_l$ 时， $a_j \mid a_l$ 成立，故当 $a_j=a_l$ 时贡献为 1，否则贡献为 0．

得出 $tot$ 之后，数值对 $(x,y)$ 对答案的贡献就是这个数值对对应的有序二元组数量和 $tot$ 之积，具体来说，

- 若 $x=y$，贡献为 $cnt[x] * (cnt[x] - 1) * tot$．
- 否则，贡献为 $cnt[x] * cnt[y] * tot$．

注意，这个题需要开 `__int128`．

**标程**

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef __int128 lll;
const int N = 1e5 + 100;
int n, a[N];
lll cnt[N], mult[N], divs[N];

void print(lll x) {
    if (x >= 10)
        print(x / 10);
    putchar('0' + x % 10);
}

signed main() {
    cin >> n;
    int maxa = -1;
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
        cnt[a[i]]++;
        maxa = max(maxa, a[i]);
    }
    // 预处理
    for (int i = 1; i <= maxa; i++) {
        for (int j = i; j <= maxa; j += i) {
            mult[i] += cnt[j];
        }
        for (int j = 1; j <= sqrt(i); j++) {
            if (i % j)
                continue;
            divs[i] += cnt[j];
            if (j * j != i) {
                divs[i] += cnt[i / j];
            }
        }
    }
    lll T = -n;
    for (int i = 1; i <= maxa; i++) {
        T += cnt[i] * mult[i];
    }
    // 计算贡献
    lll ans = 0;
    for (int x = 1; x <= maxa; x++) {
        for (int y = x; y <= maxa; y += x) {
            lll Ai = mult[x] + divs[x] - 2;
            lll Ak = mult[y] + divs[y] - 2;
            lll Bik = 1 + (x == y);
            lll tot = T - Ai - Ak + Bik;
            if (x == y) {
                ans += (lll)cnt[x] * (cnt[y] - 1) * tot;
            } else {
                ans += (lll)cnt[x] * cnt[y] * tot;
            }
        }
    }
    print(ans);
}
```
