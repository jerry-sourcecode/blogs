---
title: 安吉D8-E
published: 2026-08-10
updated: 2026-08-10
description: Day8-E
tags:
  - OI
  - 题目
  - dp
  - 线段树
language: 中文
slug: problem/anji2026/D8/E
series: 2026安吉
category: 题目
---

> [!NOTE]- 原题呈现
> # P10381 杂赛选比
>
> ## 题目背景
>
> 你说得对，但是小 $\iiint$ 在打 CF 时将 Earn or Unlock 错看成了下面的鬼畜样子，痛失 2h 遗憾离场，希望大家引以为戒．
>
> ## 题目描述
>
> 给定一个长度为 $n$ 的数组 $a$，初始只有 $a_1$ 是已被解锁的．现在有一个整数 $i$，初始值为 $1$．现在小 $\iiint$ 在对这个数组进行一个游戏：
>
> - 如果 $a_i$ 未被解锁，游戏结束．
> - 否则他可以将 $a_{i+1\sim i+a_i}$ 设置成已被解锁的，或是获得 $a_i$ 个金币（如果 $a_i=0$ 则无法解锁任何元素），然后将 $i$ 加 $1$．
> 
> 请你求出游戏结束后你能获得的最大金币数量．
>
> ## 输入格式
>
> **本题有多组测试数据．**
>
> 第一行一个整数 $T$，表示测试数据组数．
>
> 对于每一组数据，第一行一个正整数 $n$．
>
> 接下来一行 $n$ 个非负整数 $a_{1\sim n}$．
>
> ## 输出格式
>
> 对于每一组数据，一行一个数，表示答案．
>
> ## 输入输出样例 #1
>
> ### 输入 #1
>
> ```
> 3
> 2
> 1 2
> 5
> 2 4 5 0 1
> 4
> 0 4 4 4
> ```
>
> ### 输出 #1
>
> ```
> 2
> 9
> 0
> ```
>
> ## 输入输出样例 #2
>
> ### 输入 #2
>
> ```
> 1
> 10
> 1 1 4 5 1 4 1 9 1 9
> ```
>
> ### 输出 #2
>
> ```
> 26
> ```
>
> ## 说明/提示
>
> #### 【样例 1 解释】
>
> 对于第一组数据，你可以解锁 $a_2$，再获得 $a_2$ 个金币．而对于第三组数据，你无法解锁 $a_2$，因此只能获得 $0$ 个金币．
>
> 对于第二组数据，你可以解锁 $a_2,a_3$，并获得 $9$ 个金币．
>
> #### 【样例 2 解释】
>
> 将第 $1,2,3,6$ 个位置用于解锁为最优方案．
>
> #### 【数据范围】
>
> 对于 $100\%$ 的数据，$1\le n\le10^5$，$0\le a_i\le10^5$，$T\le 5$．
>
> |测试点编号|$n\leq$|$a_i\leq$|$T=$|特殊性质|
> |:-:|:-:|:-:|:-:|:-:|
> |$1$|$10$|$0$|$1$|/|
> |$2\sim3$|$10$|$5$|$1$|/|
> |$4\sim5$|$600$|$600$|$1$|/|
> |$6\sim8$|$5000$|$5000$|$1$|/|
> |$9\sim10$|$10^5$|$5$|$5$|/|
> |$11\sim12$|$5\times10^4$|$10^5$|$5$|$a_i>n$|
> |$13\sim20$|$10^5$|$10^5$|$5$|/|

此题考虑使用动态规划．

不妨定义 `dp[i][0/1]` 表示考虑到第 $i$ 张卡牌（即 $a$ 数组中第 $i$ 个数），且第 $i$ 张选择用于解锁（第二维为 0）或选择用于获得金币（第二维为 1），所能够获得的最大金币．

显然，当想要去考虑第 $i$ 张卡牌，前提是第 $i$ 张卡牌被解锁．因此，`dp[i][0/1]` 可以由所有能够解锁第 $i$ 张卡牌的状态转移而来．具体来说，是由所有满足以下条件的卡牌 $j$ 从 `dp[k][0]` 转移而来：

- 卡牌 $j$ 的解锁范围能够触及到卡牌 $i$，即 $j + a[j] \geq i$．

由于使用卡牌 $j$ 之后，卡牌 $i$ 立即被解锁了，因此处于卡牌 $j$ 到 $i$ 之间的卡牌选择什么都可以，显然，这里选择得分比选择解锁更优，因此，可以在转移时加上卡牌 $[k+1, i-1]$ 的权值之和，因此，有如下的转移方程：

$$
dp[i][0] = \max_j^{j + a[j] \geq i}(dp[j][0] + \sum^{i-1}_{k=j+1}a_k)
$$

$$
dp[i][1] = dp[i][0] + a_i
$$

其中的 $j$ 满足上述条件．最终状态即为 $dp[n][1]$．

不难发现，`dp[x][1]`（$x\in\mathbb{N}^+$）没有在转移中提供过数据，因此可以舍去第二维，且默认第二维是 0．这样状态就变为了：考虑前 $i$ 张卡牌，且钦定第 $i$ 张需要用于解锁，所能够获取的最多金币．这样最终状态需要补上最后一张卡牌用于解锁相对用来获取金币所带来的损失（即 $a_n$），故最终状态为 $dp[n] + a_n$．

求区间和可以使用前缀和优化，总算法时间复杂度为 $O(n^2)$．

**40pts 标程**

```cpp
#include <bits/stdc++.h>
#define sum(l, r) (s[(r)] - s[(l) - 1])
using namespace std;
const int N = 1e5 + 100;
int n, a[N], f[N], s[N];

void solve() {
    cin >> n;
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
        s[i] = s[i - 1] + a[i];
        f[i] = -1e8;
    }
    f[1] = 0;
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j < i; j++) {
            if (j + a[j] >= i) {
                f[i] = max(f[i], f[j] + sum(j + 1, i - 1));
            }
        }
    }
    int ans = a[1] + f[1];
    for (int i = 2; i <= n; i++) {
        ans = max(ans, a[i] + f[i]);
    }
    cout << ans << endl;
}

signed main() {
    int T;
    cin >> T;
    while (T--)
        solve();
    return 0;
}
```

考虑如何优化掉枚举 $j$ 的那一层循环．

上面的转移方程可以变形为下面的形式：

$$
dp[i] = \max_j^{j + a[j] \geq i}((dp[j] - s[j]) + s[i-1])
$$

若设 $v[j] = dp[j] - s[j],u[j]=j+a[j]$，则可以表示为：

$$
dp[i]=s[i-1]+\max_j^{u[j] \geq i} v[j]
$$

可以使用线段树动态维护最大值．每计算完一个点 $k$ 之后，将 $k$ 对应的 $v[k]$ 当作值，$u[j]$ 当作键加入线段树．在每一次计算时，查询在 $[1,i]$ 之间的元素的最大值，就可以使用 $O(\log n)$ 的时间复杂度计算 $\max_j^{u[j] \geq i} v[j]$．

 **标程**

```cpp
#include <bits/stdc++.h>
#define int long long
using namespace std;
const int N = 1e5 + 100;
const int INF = 1e18;
int n, a[N], f[N], s[N];

struct SegmentTree {
    struct Node {
        int l, r, max;
        friend Node operator+(const Node a, const Node b) {
            if (a.max == -INF)
                return b;
            if (b.max == -INF)
                return a;
            Node nNode;
            nNode.max = std::max(a.max, b.max);
            nNode.l = a.l;
            nNode.r = b.r;
            return nNode;
        }
        static Node null() { return {0, 0, (int)-1e18}; }
    } f[N * 8];
    void build(int t, int l, int r) {
        if (l > r) {
            return;
        }
        f[t].l = l;
        f[t].r = r;
        if (l == r) {
            f[t].max = -INF;
            return;
        }

        int mid = (l + r) / 2;
        build(t * 2, l, mid);
        build(t * 2 + 1, mid + 1, r);
        pushup(t);
    }
    void pushup(int t) {
        int l = f[t].l, r = f[t].r;
        f[t] = f[t * 2] + f[t * 2 + 1];
        f[t].l = l;
        f[t].r = r;
    }
    Node query(int t, int x, int y) {
        if (x > y) {
            return Node::null();
        }
        if (x <= f[t].l && f[t].r <= y) {
            return f[t];
        }
        if (x > f[t].r || y < f[t].l) {
            return Node::null();
        }
        return query(t * 2, x, y) + query(t * 2 + 1, x, y);
    }
    void Modify(int t, int pos, int v) {
        if (f[t].l == pos && f[t].r == pos) {
            f[t].max = max(f[t].max, v);
            return;
        }
        if (f[t].l > pos || f[t].r < pos) {
            return;
        }
        Modify(t * 2, pos, v);
        Modify(t * 2 + 1, pos, v);
        pushup(t);
    }
} sgTree;

void solve() {
    cin >> n;
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
        s[i] = s[i - 1] + a[i];
        f[i] = -INF;
    }
    sgTree.build(1, 1, n * 2);

    f[1] = 0;
    sgTree.Modify(1, 1 + a[1], f[1] - s[1]);
    int maxidx = 1 + a[1];

    for (int i = 2; i <= n; i++) {
        int qry = sgTree.query(1, i, maxidx).max;
        if (qry == -INF) {
            continue;
        }
        f[i] = qry + s[i - 1];
        sgTree.Modify(1, i + a[i], f[i] - s[i]);
        maxidx = max(maxidx, i + a[i]);
    }
    int ans = a[1] + f[1];
    for (int i = 2; i <= n; i++) {
        ans = max(ans, a[i] + f[i]);
    }
    cout << ans << endl;
}

signed main() {
    int T;
    cin >> T;
    while (T--)
        solve();
    return 0;
}
```
