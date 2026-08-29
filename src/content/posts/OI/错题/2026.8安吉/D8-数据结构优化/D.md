---
title: 安吉D8-D
published: 2026-08-09
updated: 2026-08-09
description: Day8-D
tags:
  - OI
  - 题目
  - 分治
  - 未完成
language: 中文
slug: problem/anji2026/D8/D
series: 2026安吉
category: 题目
---

> [!NOTE]- 原题呈现
> # P13037 Hidden Pancakes
>
> ## 题目描述
>
> 我们总共要烹饪 $\mathbf{N}$ 张煎饼．这些煎饼的半径分别为 $1$ 厘米（cm）、$2 \mathrm{~cm}$、$3 \mathrm{~cm}$，……，以及 $\mathbf{N} \mathrm{cm}$，但烹饪顺序不一定按半径从小到大排列．烹饪完第一张煎饼后，我们直接将其放在盘子上．之后每烹饪完一张煎饼，就将其叠放在之前所有煎饼的最上方，且所有煎饼的中心对齐．这样，每张煎饼在刚被加入时都能从顶部被看到．只有当之后烹饪了比它半径更大的煎饼时，这张煎饼才会被隐藏．
>
> 例如，假设我们烹饪 4 张煎饼．首先烹饪半径为 $3 \mathrm{~cm}$ 的煎饼，此时它可见．接着烹饪半径为 $1 \mathrm{~cm}$ 的煎饼，叠放在第一张煎饼上，此时两张煎饼都可见．然后烹饪半径为 $2 \mathrm{~cm}$ 的煎饼，它会覆盖前一张煎饼（半径为 $1 \mathrm{~cm}$ 的煎饼），但不会覆盖第一张煎饼，因此此时共有 2 张煎饼可见．最后，烹饪半径为 $4 \mathrm{~cm}$ 的煎饼，它会覆盖所有其他煎饼，此时只有 1 张煎饼可见．下图展示了每张煎饼被烹饪后叠放的状态，其中完全不透明的煎饼表示可见，半透明的煎饼表示不可见．
>
> ![](https://cdn.luogu.com.cn/upload/image_hosting/s69k9evw.png)
>
> 设 $\mathbf{V}_{\mathbf{i}}$ 表示叠放了恰好 $i$ 张煎饼时可见的煎饼数量．在上面的例子中，$\mathbf{V}_{1}=1$、$\mathbf{V}_{2}=2$、$\mathbf{V}_{3}=2$、$\mathbf{V}_{4}=1$．
>
> 给定列表 $\mathbf{V}_{1}, \mathbf{V}_{2}, \ldots, \mathbf{V}_{\mathbf{N}}$，问在所有 $\mathbf{N} !$ 种可能的烹饪顺序中，有多少种顺序能恰好得到给定的 $\mathbf{V}_{\mathbf{i}}$ 序列？由于结果可能非常大，只需输出结果对质数 $10^{9}+7$（即 $1000000007$）取模后的值．
>
> ## 输入格式
>
> 输入的第一行包含测试用例数量 $\mathbf{T}$．每个测试用例包含两行：第一行是一个整数 $\mathbf{N}$，表示烹饪的煎饼数量；第二行包含 $\mathbf{N}$ 个整数 $\mathbf{V}_{1}, \mathbf{V}_{2}, \ldots, \mathbf{V}_{\mathbf{N}}$，分别表示叠放了 $1, 2, \ldots, \mathbf{N}$ 张煎饼时的可见煎饼数量．
>
> ## 输出格式
>
> 对于每个测试用例，输出一行 `Case #x: y`，其中 $x$ 是测试用例编号（从 1 开始），$y$ 是满足条件的烹饪顺序数量对 $10^{9}+7$ 取模后的结果．
>
> ## 输入输出样例 #1
>
> ### 输入 #1
>
> ```
> 3
> 4
> 1 2 2 1
> 3
> 1 1 2
> 3
> 1 1 3
> ```
>
> ### 输出 #1
>
> ```
> Case #1: 1
> Case #2: 2
> Case #3: 0
> ```
>
> ## 输入输出样例 #2
>
> ### 输入 #2
>
> ```
> 1
> 24
> 1 2 1 2 1 2 1 2 1 2 1 2 1 2 1 2 1 2 1 2 1 2 1 2
> ```
>
> ### 输出 #2
>
> ```
> Case #1: 234141013
> ```
>
> ## 说明/提示
>
> **样例解释**
>
> 样例 #1 已在题目描述中说明，唯一的满足条件的烹饪顺序是 $3,1,2,4$．
>
> 在样例 #2 中，顺序 $1,3,2$ 和 $2,3,1$ 均能满足给定的 $\mathbf{V}_{\mathbf{i}}$ 序列．下图展示了这两种情况：
>
> ![](https://cdn.luogu.com.cn/upload/image_hosting/o981r60x.png)
>
> ![](https://cdn.luogu.com.cn/upload/image_hosting/3vhqt53k.png)
>
> 在样例 #3 中，叠加第二张煎饼后只有 1 张煎饼可见，因此无法通过叠加第三张煎饼使可见煎饼数量超过 2．
>
> 样例测试集 2 符合测试集 2 的限制条件，但提交的解法不会实际运行该测试集．
>
> 在测试集 2 的样例中，共有 $316234143225$ 种烹饪顺序满足给定的 $\mathbf{V}_{\mathbf{i}}$ 序列，对 $10^{9}+7$ 取模后的结果是 $234141013$．
>
> **限制条件**
>
> - $1 \leq \mathbf{T} \leq 100$．
> - 对于所有 $i$，$1 \leq \mathbf{V}_{\mathbf{i}} \leq i$．
> 
> **测试集 1（可见判定）**
>
> - 时间限制：30 秒．
> - $2 \leq \mathbf{N} \leq 13$．
> 
> **测试集 2（隐藏判定）**
>
> - 时间限制：40 秒．
> - $2 \leq \mathbf{N} \leq 10^{5}$．

直接解决问题有些难度，我们先不要想着如何确定所有煎饼的位置，先想想如何确定最大煎饼（即半径为 N 的煎饼）的位置．

显然，当最大煎饼铺在盘子上时，可见的煎饼数量一定为 1，只能看见最大煎饼．而且之后的煎饼无法覆盖这张煎饼，故后面的可见煎饼数量必然大于 1．因此，我们得知：最大的煎饼就处于 $v$ 序列中最后一个 1 的位置．

得知了最大煎饼，又有什么用呢？我们知道，最大煎饼会把所有的煎饼分成上面和下面两个部分，由于有最大煎饼的阻隔，上下两个部分互不干涉，分成了两个子问题．在子问题中，我们也可以获取子区间内最后一个 1 的位置，这是子区间内的最大煎饼（当然，如果该子区间在父问题中在上面，则该区间内的每一个数字被增加了 1，即父问题中的最大煎饼是可见的，因此，此时需要查询的是子区间内最后一个 1 的位置）

一般的，对于一个子问题，它需要得到 $[l,r]$ 之间煎饼排列的方案数，其会存在一个权值 $w$．解决子问题的操作如下（设该子问题中煎饼数量为 $x=r-l+1$）：

1. 找出当前区间中最小的元素 $v[k]$，如果 $v[k]\neq w$，说明没有方案，返回 0．
2. 依照这个最小的元素进行分割，可以分成上子问题和下子问题，上子问题的权值为 $w+1$（因为有煎饼 $k$ 的存在），下子问题的权值为 $w$．若上子问题和下子问题的答案分别为 $a$ 和 $b$，且上子问题的煎饼数量为 $y$，那么答案相当于在一共的 $x$ 个煎饼中选择 $y$ 个放在上子区间，为 $\binom{x}{y}\times a\times b$．
3. 边界情况，若该子问题中煎饼数量为 1，则表明只有 1 种情况．

一般的，根问题（最大的问题）的权值为 1．

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long
const int N = 1e5 + 100;
const int MOD = 1e9 + 7;
int n, fac[N];

struct Node {
    int v, id;
    friend bool operator<(const Node a, const Node b) {
        if (a.v == b.v)
            return a.id > b.id;
        return a.v < b.v;
    }
};

Node v[N];
struct ST {
    Node mn[18][N];
    Node *a;
    int size;
    void init(Node *a, int size) {
        this->a = a;
        memset(mn, 0x3f, sizeof(mn));
        for (int i = 1; i <= size; i++) {
            mn[0][i] = a[i];
        }
        for (int j = 1; j <= 16; j++) {
            for (int i = 1; i <= size; i++) {
                mn[j][i] = min(mn[j - 1][i], mn[j - 1][i + (1 << (j - 1))]);
            }
        }
    }
    Node qry(int l, int r) {
        int len = log2(r - l + 1);
        return min(mn[len][l], mn[len][r - (1 << len) + 1]);
    }
} st;

int fastPow(int a, int p) {
    int ans = 1, cur = a;
    while (p >= 1) {
        if (p & 1) {
            (ans *= cur) %= MOD;
        }
        (cur *= cur) %= MOD;
        p /= 2;
    }
    return ans;
}

int inv(int x) { return fastPow(x, MOD - 2); }

int calFac(int x) {
    if (!fac[x]) {
        fac[x] = calFac(x - 1) * x % MOD;
    }
    return fac[x];
}

int calC(int from, int select) {
    if (select == 0 || from == select)
        return 1;
    return calFac(from) * inv(calFac(select)) % MOD *
           inv(calFac(from - select)) % MOD;
}

int solve_sub(int l, int r, int lower) {
    // cout << "DEBA " << l << "~" << r << endl;
    if (l > r)
        return 1;
    Node nd = st.qry(l, r);
    if (nd.v != lower) {
        return 0;
    }
    if (l == r) {
        return 1;
    }
    int mid = nd.id;
    int upper = r - mid;
    int ways = calC(r - l, upper);
    // cout << "DEBB " << l << "~" << r << ": " << mid << " " << ways << " "
    // << lower << " " << nd.v << endl;
    int left = solve_sub(l, mid - 1, lower);
    int right = solve_sub(mid + 1, r, lower + 1);
    return left * right % MOD * ways % MOD;
}

void solve(int id) {
    cin >> n;
    for (int i = 1; i <= n; i++) {
        cin >> v[i].v;
        v[i].id = i;
    }
    st.init(v, n);
    cout << "Case #" << id << ": " << solve_sub(1, n, 1) << endl;
}

signed main() {
    int T;
    cin >> T;
    fac[0] = 1;
    for (int i = 1; i <= T; i++)
        solve(i);
    return 0;
}
```