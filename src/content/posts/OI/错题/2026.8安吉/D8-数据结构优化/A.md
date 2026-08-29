---
title: 安吉D8-A
published: 2026-08-09
updated: 2026-08-09
description: Day8-A
tags:
  - OI
  - 题目
  - 倍增
  - 滚动数组优化
language: 中文
slug: problem/anji2026/D8/A
series: 2026安吉
category: 题目
---

> [!NOTE]- 原题呈现
> # P3509 ZAB-Frog
>
> ## 题目描述
>
> 在一个特别长且笔直的 Byteotian 小溪的河床上，有 $n$ 块石头露出水面．它们距离小溪源头的距离分别为 $p_1 < p_2 < \cdots < p_n$．一只小青蛙正坐在其中一块石头上，准备开始它的跳跃训练．每次青蛙跳跃到距离它所在石头第 $k$ 近的石头上．具体来说，如果青蛙坐在位置 $p_i$ 的石头上，那么它将跳到这样的 $p_j$ 上，使得：
>
> $$
> |\{ p_a : |p _ a - p _ i| < |p_j - p_i| \}| \le k \text{ and } |\{ p_a : |p _ a - p _ i| \le |p_j - p_i| \}| > k
> $$
>
> 如果 $p_j$ 不是唯一的，那么青蛙在其中选择距离源头最近的石头．对于每一块石头分别计算，若青蛙从这块石头开始跳跃，经过 $m$ 次跳跃后最终会停留在哪一块石头上？
>
> ## 输入格式
>
> 标准输入的第一行包含三个整数 $n$、$k$ 和 $m$（$1 \le k < n \le 10^6, 1 \le m \le 10^{18}$），用空格分隔，分别表示石头的数量、参数 $k$ 和计划跳跃的次数．第二行包含 $n$ 个整数 $p_j$（$1 \le p_1 < p_2 < \cdots < p_n \le 10^{18}$），用空格分隔，表示小溪河床上连续石头的位置．
>
> ## 输出格式
>
> 你的程序应在标准输出上打印一行，包含 $n$ 个整数 $r_1, r_2, \cdots, r_n$，用空格分隔．数字 $r_i$ 表示从输入顺序中的第 $i$ 块石头开始跳跃 $m$ 次后，青蛙最终停留的石头编号．
>
> ## 输入输出样例 #1
>
> ### 输入 #1
>
> ```
> 5 2 4
> 1 2 4 7 10
> ```
>
> ### 输出 #1
>
> ```
> 1 1 3 1 1
> ```
>
> ## 说明/提示
>
> ### 样例 #1 解释：
>
> ![](https://cdn.luogu.com.cn/upload/image_hosting/yyilx2mp.png)
>
> 图中展示了青蛙从每块石头跳跃（单次跳跃）到的位置．

我们发现此题中，$m$ 的范围是 $1e18$，这显然不可能模拟跳那么多步．

通过模拟样例，我们可以发现：从一个格子开始，跳到的下一个格子是固定的．因此，考虑使用倍增．

具体来说，分别预处理出每一个点跳 1 次、跳 2 次、跳 4 次……，设点 $i$ 跳了 $2^j$ 次之后落在了 $f[j][i]$，则转移方程为：

```cpp
f[i][j] = f[i-1][f[i-1][j]];
```

可以枚举 $m$ 中所有二进制为 1 的位数 $x$（如 $6$ 的二进制表示为 $110$，由于二进制表示中为 1 的数位是右起第二位和右起第三位，因此此时 $x\in\{2, 3\}$），初始时，设每一个格子 $i$ 上都有一只青蛙，这只青蛙实时位置为 $a_i$，对于每一个 $x$，将每一个格子 $a_i$ 跳 $2^{x-1}$ 次，即 `a[i] = f[x-1][i]`．

最终 $a_i$ 中记录的值就是答案了．

那么，怎么求出最初始的 $f[0][i]$，即每一个点跳一步会落在哪里呢？

使用滑动窗口．维护一个大小始终为 $k+1$ 的滑动窗口 $[l, r]$．初始时，将前 $k+1$ 个数加入滑动窗口，之后，对于每一个点 $i$，维护这个滑动窗口，使得其恰好包含 $i$ 和离 $i$ 最近的前 $k$ 个数：

- 尝试将此窗口整体右移 1 格．具体的：若滑动窗口外右边第一个元素 $r+1$ 离 $i$ 的距离比滑动窗口中最左边元素 $l$ 小，即第 $r+1$ 块石头离 $i$ 比第 $l$ 块石头近，此时，将滑动窗口右移一格．重复这个操作，直到上述条件不满足．
- 接着，比较滑动窗口中最左端的 $l$ 和最右端的 $r$，这两块石头是滑动窗口中离 $i$ 最远的两块，比较谁离 $i$ 更远，更远的一块就是 $i$ 下一步会跳到的石头．

可以写出代码：

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 1e6 + 1;
typedef long long ll;
const int INF = 0x3f3f3f3f;
ll n, k, m, a[N];
int f[60][N];
signed main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    cout.tie(0);
    cin >> n >> k >> m;
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
    }
    a[n + 1] = INF; // 哨兵
    int l = 1, r = k + 1;
    for (int i = 1; i <= n; i++) {
        while (r < n && a[r + 1] - a[i] < a[i] - a[l]) {
            l++;
            r++;
        }
        if (a[i] - a[l] >= a[r] - a[i])
            f[0][i] = l;
        else
            f[0][i] = r;
    }

    for (int i = 1; i <= 59; i++) {
        for (int j = 1; j <= n; j++) {
            f[i][j] = f[i - 1][f[i - 1][j]];
        }
    }

    for (int i = 1; i <= n; i++) {
        ll x = i, st = m, wt = 0;
        while (st >= 1) {
            if (st % 2) {
                x = f[wt][x];
            }
            st /= 2;
            wt++;
        }
        cout << x << ' ';
    }
}
```

如果这样写，会导致 MLE．具体来说，由于此题的 125MB 内存限制，我们只能存 $3\times 10^7$ 个 int 类型变量．而 f 数组中有 $60\times 10^6 = 6\times 10^7$ 个 int 变量．

此题应该选用滚动倍增数组进行优化．具体来说，省去 f 数组，在计算 $a_i$ 时，动态计算当前需要的倍增数组．

**标程**

```cpp
#include <bits/stdc++.h>
#define int long long
using namespace std;
const int N = 1e6 + 100;
typedef long long ll;
const ll INF = 2e18;
int n, k;
ll m, a[N];
int nxt[N], cur[N], tmp[N];
signed main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    cout.tie(0);
    cin >> n >> k >> m;
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
    }
    a[n + 1] = INF;
    int l = 1, r = k + 1;
    for (int i = 1; i <= n; i++) {
        while (r < n && a[r + 1] - a[i] < a[i] - a[l]) {
            l++;
            r++;
        }
        if (a[i] - a[l] >= a[r] - a[i])
            nxt[i] = l;
        else
            nxt[i] = r;
    }

    for (int i = 1; i <= n; i++) {
        cur[i] = i;
    }
    while (m >= 1) {
        if (m & 1) {
            for (int i = 1; i <= n; i++) {
                tmp[i] = cur[i];
            }
            for (int i = 1; i <= n; i++) {
                cur[i] = nxt[tmp[i]];
            }
        }
        m /= 2;
        for (int i = 1; i <= n; i++)
            tmp[i] = nxt[i];
        for (int i = 1; i <= n; i++)
            nxt[i] = tmp[tmp[i]];
    }
    for (int i = 1; i <= n; i++) {
        cout << cur[i] << ' ';
    }
}
```