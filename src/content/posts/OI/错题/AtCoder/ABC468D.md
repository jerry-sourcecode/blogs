---
title: ABC468D
published: 2026-07-31
updated: 2026-07-31
description: AtCoder BeginnerContest 468 D
tags:
  - OI
  - 题目
  - 回文
language: 中文
slug: problem/atcoder/abc468d
category: 题目
---

# 题目描述

>[!note]-
>一个由小写英文字母组成的字符串，如果它**最多通过修改一个字符就能变成一个回文串**，则称它为一个**好字符串**．
>
>例如，`a`、`iwai` 和 `abcdcza` 都是好字符串，而 `abcd` 和 `atcoder` 则不是．需要注意，**回文串本身也是好字符串**．
>
>给定一个由小写英文字母组成的字符串 `S`，请找出 `S` 中**非空子串（连续子序列）** 中，好字符串的数量．
>
>如果两个子串在 `S` 中的位置不同，即使它们的内容相同，也视为不同的子串，需要分别计数．
>### 约束条件
>- 字符串 `S` 的长度介于 `1` 到 `10^4` 之间．

# 思路

对于一个字符串，若想要判断该字符串的子串是否回文，则需要明确一个**中心**和一个**半径**．

因此，对于此题，我们枚举中心，并向两边扩展（加长半径），对于扩展的一对字符，若这两个字符不相同，则将 `diff` 增加 1，若 `diff` 增加到 2，则剪枝，并更换中点．

标程：

```cpp
#include <bits/stdc++.h>
using namespace std;
string s;
int ans;
signed main() {
    cin >> s;
    for (int i = 0; i < s.size(); i++) {
        // 奇数：中点为 i
        int diff = 0;
        for (int r = 0; i - r >= 0 && i + r < s.size(); r++) {
            // 半径为 r（仅存在i则半径为0）
            if (s[i - r] != s[i + r])
                diff++;
            if (diff >= 2) {
                break;
            }
            ans++;
        }
    }
    for (int i = 0; i < s.size() - 1; i++) {
        // 偶数：中点为 i 和 i+1 之间
        int diff = 0;
        for (int r = 0; i - r >= 0 && i + r + 1 < s.size(); r++) {
            // 半径为 r（仅存在i和i+1则半径为0）
            if (s[i - r] != s[i + r + 1])
                diff++;
            if (diff >= 2) {
                break;
            }
            ans++;
        }
    }
    cout << ans;
}
```

这样的算法时间复杂度 $O(n^2)$．勉强可以通过．

这道题还可以使用哈希 + 二分的方法，在这里不再赘述．
