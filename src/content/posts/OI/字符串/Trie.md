trie，即字典树，可以快速处理字符串前后缀的相关问题．

# 结构

字典树，顾名思义，就是一棵树．只不过这棵树是用来存储一个个字符串的．我们会在树的边上存储一个字符．

下图所示的一棵树就是一棵 trie 树：（图源 oi-wiki）

![](https://static.cdn.menci.xyz/oi-wiki/string/images/trie1.png?h=02Eo7Q)

在 trie 中的每一个节点 $i$ 都可以代表一个字符串 $s_i$．其意义为从根节点一直向下走，走到节点 $i$，则所路过的所有边上字符组成的字符串 $s_i$．

在上图中，有：

- $s_1=$ `''`（空串）
- $s_2=$ `'a'`
- $s_5=$ `'aa'`
- $s_{15}=$ `'caaa'`
- ……

# 储存

trie 树节点的定义代码如下：

```cpp
struct Node {
    // 字符串池中有多少字符串与当前节点表示的字符串相等
    int cnt;
    // 子节点编号，如 son['a'] 表示一个子节点，其余该节点所连边权值为 a
    unordered_map<char, int> son;
} tr[N];
```

我们想要将一个字符串塞入 trie 中．需要从根节点开始遍历，依次查找字符串的每一个字符．一般的，其步骤如下：

- 设我们当前已经匹配到字符串 $str$ 的第 $idx$ 位（0-based），目前所在节点的编号为 $top$．
- 在 $top$ 下查找一个子节点 $son$，使得 $top$ 和 $son$ 之间的权值为 $str_{idx}$，若没有，创建之．
- 很好，第 $idx$ 位已经完成匹配了，接下来，只需要以 $son$ 为根节点，递归匹配第 $idx+1$ 位即可．
- 特别的，若匹配时 $idx$ 超出了 $str$ 的范围，即 `idx == str.size()`，那么这个字符串和 $top$ 所对应的字符串是完全相等的，将 $top$ 节点的权值增加一．

```cpp
void insert(int top, string& str, unsigned idx){
    if (idx == str.size()){
        // 匹配结束啦，到了字符串末尾了
        tr[top].cnt++;
        return;
    }
    char c = str[idx]; // 目标字符
    if (!tr[top].son.count(c)){
        // 没有这个节点，初始化一下
        tr[top].son[c] = ++cnt; // 全局 cnt 是节点数量
        tr[cnt].cnt = 0;
        tr[cnt].son.clear();
    }
    // 递归处理
    return insert(tr[top].son[c], str, idx+1);
}
```

# 查询

至于查询也很简单，和插入类似，只需要从顶端开始，每一次检查有没有目标字符的分支，如果没有，返回 `0`，如果有，则进入，并且尝试匹配下一个字符．直到所有字符都处理完，这样就返回该叶子结点所代表的数据．

```cpp
int query(int top, string& str, unsigned idx){
    if (idx == str.size()){
        return sz[top];
    }
    if (!tr[top].son.count(str[idx])){
        // 注意，这里使用 count 而不是 []，是因为 [] 会将 str[idx] 添加进 map
        return 0;
    }
    return query(tr[top].son[str[idx]], str, idx+1);
}
```

接下来看一道例题．

（洛谷 P8306）

这道题需要求有多少字符串是其余字符串的前缀．

很简单，只要：

- 将所有字符串加入到 trie．
- 对这棵 trie 树做一次 dfs，求出每一个节点 $i$ 为根的子树大小 $sz_i$．若节点 $i$ 代表的字符串为 $s_i$，则 $sz_i$ 表示**以 $s_i$ 为前缀的字符串有多少个**．
- 读入每一个查询字符串，并从根节点开始搜索，在找到相对应的节点（设这个节点为 $k$）之后，$sz_k$ 即为所求的答案．

标程：

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 3e6+100;
int n, q, cnt, sz[N];
struct Node {
    int cnt;
    unordered_map<char, int> son;
} tr[N];

void insert(int top, string& str, unsigned idx){
    if (idx == str.size()){
        tr[top].cnt++;
        return;
    }
	char c = str[idx];
    if (!tr[top].son.count(c)){
        tr[top].son[c] = ++cnt;
        tr[cnt].cnt = 0;
        tr[cnt].son.clear();
    }
    return insert(tr[top].son[c], str, idx+1);
}

int cal(int top){
    sz[top] = tr[top].cnt;
    for(auto it : tr[top].son){
        sz[top] += cal(it.second);
    }
    return sz[top];
}

int query(int top, string& str, unsigned idx){
    if (idx == str.size()){
        return sz[top];
    }
    if (!tr[top].son.count(str[idx])){
        return 0;
    }
    return query(tr[top].son[str[idx]], str, idx+1);
}

void solve(){
    tr[0].cnt = 0;
    tr[0].son.clear();
    cin >> n >> q;
    for(int i = 1; i <= n; i++){
        string str;
        cin >> str;
        insert(0, str, 0);
    }
    cal(0);
    for(int i = 1; i <= q; i++){
        string str;
        cin >> str;
        cout << query(0, str, 0) << endl;
    }
}

int main()
{
    int T;
    cin >> T;
    while(T--) solve();
}
```cpp
