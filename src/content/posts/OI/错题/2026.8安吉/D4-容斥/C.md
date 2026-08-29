---
title: 安吉D4-C
published: 2026-08-04
updated: 2026-08-04
description: Day4-C
tags:
  - OI
  - 题目
  - 容斥
language: 中文
slug: problem/anji2026/D4/C
series: 2026安吉
category: 题目
---

> [!note]- 原题呈现
> # P5123 Cowpatibility G
> ## 题目描述
>
> 研究证明，有一个因素在两头奶牛能否作为朋友和谐共处这方面比其他任何因素都来得重要——她们是不是喜欢同一种口味的冰激凌！
>
> Farmer John 的 $N$ 头奶牛（$2\le N\le 5\times 10^4$）各自列举了她们最喜欢的五种冰激凌口味的清单．为使这个清单更加精炼，每种可能的口味用一个不超过 $10^6$ 的正整数 $\texttt{ID}$ 表示．如果两头奶牛的清单上有至少一种共同的冰激凌口味，那么她们可以和谐共处．
>
> 请求出不能和谐共处的奶牛的对数．
>
> ## 输入格式
>
> 输入的第一行包含 $N$．以下 $N$ 行每行包含 $5$ 个整数（各不相同），表示一头奶牛最喜欢的冰激凌口味．
>
> ## 输出格式
>
> 输出不能和谐共处的奶牛的对数．
>
> ## 输入输出样例 #1
>
> ### 输入 #1
>
> ```
> 4
> 1 2 3 4 5
> 1 2 3 10 8
> 10 9 8 7 6
> 50 60 70 80 90
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
> 在这里，奶牛 $4$ 不能和奶牛 $1$、$2$、$3$ 中的任一头和谐共处，奶牛 $1$ 和奶牛 $3$ 也不能和谐共处．

或许直接求不能和谐共处的奶牛数量不好求，但是正难则反，我们可以先求能和谐共处的奶牛数量．

能和谐共处的奶牛数量 $ans$，就是至少有一种共同的冰淇淋的奶牛对数，通过容斥原理，我们在所有的冰淇凌口味，一共 $tot$ 种，钦定 $i$ 种，一共有 $\binom{tot}{i}$ 种钦定的方法．对于每一种钦定方案，我们统计所有的奶牛对，满足每一头奶牛的列表上都有钦定的 $i$ 种冰淇凌，这样的奶牛对数量，记为 $f$，将 $\binom{tot}{i}$ 种钦定的方法所得的 $f$ 取和，将其记为 $g_i$，则 $ans$ 和 $g_i$ 满足：

$$
ans=g_1-g_2+g_3-g_4+g_5
$$

在程序中，我们定义 $cnt_T$ 用于记录子集 $T$ 出现的数量，一开始，$cnt$ 为空，遍历每一头奶牛，让这头奶牛尝试和前面的奶牛在不同钦定方案下组成奶牛对，具体来说，需要做以下事情：

- 遍历当前奶牛清单 $s_i$ 的所有非空子集 $T$（即枚举钦定方案为 $T$）
	- 将 `ans` 增加 $​​(-1)^{|T-1|} * cnt_T$
	- 将 $cnt_T$ 增加 1

最终答案就是 $(n - 1) \times n / 2 - ans$．

**标程**

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N = 5e4 + 100;
ll ans;
int n;
vector<int> a[N];
map<vector<int>, int> cnt;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    cout.tie(0);
    cin >> n;
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= 5; j++) {
            int x;
            cin >> x;
            a[i].push_back(x);
        }
        sort(a[i].begin(), a[i].end());
    }
    ans = 1ll * (n - 1) * n / 2;
    for (int i = 1; i <= n; i++) {
        vector<int> vec;
        for (int k : a[i])
            vec.push_back(k);
        for (int st = 1; st <= (1 << 5) - 1; st++) {
            vector<int> now;
            for (int j = 0; j <= 4; j++) {
                if (st & (1 << j))
                    now.push_back(vec[j]);
            }
            ans += (now.size() % 2 ? -1 : 1) * cnt[now];
            cnt[now]++;
        }
    }
    cout << ans;
}
```
