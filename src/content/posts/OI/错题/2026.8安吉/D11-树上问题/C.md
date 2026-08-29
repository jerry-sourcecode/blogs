---
title: 安吉D11-C
published: 2026-08-12
updated: 2026-08-12
description: Day11-C
tags:
  - OI
  - 题目
  - 树论
language: 中文
slug: problem/anji2026/D11/C
series: 2026安吉
category: 题目
---

> [!NOTE]- 原题呈现
> # P3605 Promotion Counting P
>
> ## 题目描述
>
> 奶牛们又一次试图创建一家创业公司，还是没有从过去的经验中吸取教训——牛是可怕的管理者！
>
> 为了方便，把奶牛从 $1\sim n$ 编号，把公司组织成一棵树，1 号奶牛作为总裁（这棵树的根节点）．除了总裁以外的每头奶牛都有一个单独的上司（它在树上的 “双亲结点”）．
>
> 所有的第 $i$ 头牛都有一个不同的能力指数 $p_i$，描述了她对其工作的擅长程度．如果奶牛 $i$ 是奶牛 $j$ 的祖先节点，那么我们把奶牛 $j$ 叫做 $i$ 的下属．
>
> 不幸地是，奶牛们发现经常发生一个上司比她的一些下属能力低的情况，在这种情况下，上司应当考虑晋升她的一些下属．你的任务是帮助奶牛弄清楚这是什么时候发生的．简而言之，对于公司的中的每一头奶牛 $i$，请计算其下属满足 $p_j > p_i$ 的 $j$ 的数量．
>
> ## 输入格式
>
> 输入的第一行包括一个整数 $n$．
>
> 接下来的 $n$ 行包括奶牛们的能力指数 $p_1,p_2 \dots p_n$．保证所有数互不相同．
>
> 接下来的 $n-1$ 行描述了奶牛 $2 \sim n$ 的上司的编号．再次提醒，1 号奶牛作为总裁，没有上司．
>
> ## 输出格式
>
> 输出包括 $n$ 行．输出的第 $i$ 行应当给出有多少奶牛 $i$ 的下属比奶牛 $i$ 能力高．
>
> ## 输入输出样例 #1
>
> ### 输入 #1
>
> ```
> 5
> 804289384
> 846930887
> 681692778
> 714636916
> 957747794
> 1
> 1
> 2
> 3
> ```
>
> ### 输出 #1
>
> ```
> 2
> 0
> 1
> 0
> 0
> ```
>
> ## 说明/提示
>
> 对于 $100\%$ 的数据，$1\le n \le 10^5$，$1 \le p_i \le 10^9$．

既然是一棵树，不知道怎么做的时候就搜索．尝试 dfs，并在搜索时维护一些数据．

之后，我们观察到问题问的是：满足 $p_j > p_i$ 的 $j$ 的数量，要求有多少数大于（或小于）目标值，可以使用离散化 + 树状数组．

具体来说，我们遍历到一个节点 $u$ 时：

- 查询当前树状数组中有多少数比 $p_u$ 大．注意：现在树状数组中还没有加入过 $u$ 的任何子孙节点．
- 遍历 $u$ 子节点．
- 再次回到 $u$ 时，树状数组已经完全收录的 $u$ 的所有子孙节点．再次查询当前树状数组中有多少数比 $p_u$ 大，与第一次查询的差值就是答案．

> [!NOTE] 小技巧：树的 dfs 中两次查询时机
> 当我们在树上进行 dfs 搜索时，遍历到一个节点 $u$，在遍历其子节点前，$u$ 的子节点任意一个都没有被遍历，在遍历其子节点之后，$u$ 的所有子节点都会被遍历过．在两者之间，不会有不是 $u$ 子节点的节点被遍历，因此，若是想要查询子节点的一些情况，可以在这两次查询时机各记录一次结果，然后考虑差量．

**标程**

```cpp
#include <bits/stdc++.h>
using namespace std;

const int N = 1e5 + 100;

int n, p[N], q[N], f[N], ans[N];
vector<int> e[N];

struct BIT {
    int f[N];
    int lowbit(int i) { return i & (-i); }
    void Modify(int pos, int v) {
        for (int i = pos; i <= n; i += lowbit(i)) {
            f[i] += v;
        }
    }
    int Query(int pos) {
        if (pos == 0)
            return 0;
        int ans = 0;
        for (int i = pos; i >= 1; i -= lowbit(i)) {
            ans += f[i];
        }
        return ans;
    }
    int Query(int l, int r) { return Query(r) - Query(l - 1); }
} bit;

void dfs(int u) {
    int tot = bit.Query(p[u] + 1, n);
    bit.Modify(p[u], 1);
    for (auto v : e[u]) {
        dfs(v);
    }
    ans[u] = bit.Query(p[u] + 1, n) - tot;
}

signed main() {
    cin >> n;
    for (int i = 1; i <= n; i++) {
        cin >> p[i];
        q[i] = p[i];
    }
    sort(q + 1, q + n + 1);
    for (int i = 1; i <= n; i++) {
        p[i] = lower_bound(q + 1, q + n + 1, p[i]) - q;
    }
    for (int i = 2; i <= n; i++) {
        cin >> f[i];
        e[f[i]].push_back(i);
    }
    dfs(1);
    for (int i = 1; i <= n; i++) {
        cout << ans[i] << endl;
    }
    return 0;
}
```
