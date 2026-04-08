---
title: CF-Round-Div2-1091
published: 2026-04-08
description: 'CF Round div2 1091的各题题解'
image: ''
tags: [CF题解, 算法, CFRound]
category: '算法'
draft: false 
lang: ''
---
## A题 The Equalizer
[原题](https://codeforces.com/contest/2217/problem/A)
### 思路
读题，发现输赢是由下棋的顺序决定的

而轮到某位棋手的时候，他会选择一个元素并 $-1$，实质上就是对整个数组总和 $S$ 减一，所以我们可以把这个问题转换成对数组总和 $S$ 的奇偶性的讨论

当 $S$ 为奇数的时候，自己赢

当然，这还是在不涉及到题目所说的 “**特殊操作**” 的情况下

我们现在就需要对这个特殊操作进行分析，从结果来看，特殊操作会将整个数组元素全部变成 $k$，那么这个时候的数组总和其实就是 $S' = n \cdot k$

而使用特殊操作也是算作一步的，因此在这之后，我们就需要看新的 $S'$ 的奇偶性，此时，如果 $S'$ 为偶数，则自己赢

而由于玩家可以自己决定是否使用特殊操作，因此我们只需要检查两个地方即可：

- $S$ 是否为奇数
- $S'$ 是否为偶数

如果以上条件满足一个，则判定为 “YES”
### 代码
```c++
#include <bits/stdc++.h>

using namespace std;

int main() {
    int t;
    cin >> t;
    while (t --) {
        int n, k;
        cin >> n >> k;
        vector<int> a(n);
        for (int i = 0; i < n; i ++) cin >> a[i];

        int sum = 0;
        for (auto n: a) sum += n;

        if ((n * k) % 2 == 0 || sum % 2 != 0) {
            cout << "YES" << endl;
        }
        else cout << "NO" << endl;
    }
}
```

## B题 Flip the Bit (Easy Version)
[原题](https://codeforces.com/contest/2217/problem/B)
### 思路
先弄清题目所说的 “**特殊位置**” 是什么：

这题因为是 **Easy** 版，固定有 $k = 1$，所以其实很简单就能得到这个固定位置：
$$
x = a_{p_i} \space 1 \leq i \leq k
$$
在这里，为了方便，我们取 $x = a_{p_1}$

而题目的要求是选定区间进行翻转，使得所有位置上的数都等于特殊位置上的数

很明显，对于已经等于特殊位置上的数，我们只需要让他反转偶数次就行

而对于不等于特殊位置的数的数字，我们需要让他反转奇数次才能达到墓目的

因此，我们定义一个差异数组
$$
d_i = a_i \oplus x
$$
其中：

- $d_i = 0$ 表示当前位置已经等于目标值 $x$
- $d_i = 1$ 表示当前位置和目标值 $x$ 不同

现在问题就从 “把整个数组变成 $x$” 变成了 “让所有差异位被合法操作消掉”

一次操作会选择区间 $[l, r]$，并且必须满足 $l \leq p_1 \leq r$
也就是说每次操作都会同时带一个左端点和一个右端点

因此我们分别统计左右两边的需求

左边统计 $u$

- 从 $1$ 扫到 $p_1$
- 维护一个 $prev$，初始为 $0$
- 如果 $d_i \ne prev$，说明出现一次状态切换，$u$ 加一
- 然后更新 $prev = d_i$

右边统计 $v$

- 从 $p_1$ 扫到 $n$
- 统计 $d_i \ne d_{i+1}$ 的次数
- 这里可以令 $d_{n+1} = 0$ 当作边界哨兵

最终答案就是
$$
\max(u, v)
$$

原因是一次操作会同时消耗一个左需求和一个右需求
所以最少操作次数至少是两者中的较大值
同时这个值可以通过配对构造达到

复杂度

- 时间复杂度 $O(n)$
- 空间复杂度 $O(n)$

### 代码
```c++
#include <bits/stdc++.h>

using namespace std;

int main() {
    int t;
    cin >> t;
    while (t --) {
        int n, k;
        cin >> n >> k;
        vector<int> a(n + 1), p(k + 1);
        for (int i = 1; i <= n; i ++) cin >> a[i];
        for (int i = 1; i <= k; i ++) cin >> p[i];
        int x = a[p[1]];

        vector<int> d(n + 2, 0);
        for (int i = 1; i <= n; i ++) {
            if (a[i] != x) d[i] = 1;
            else d[i] = 0;
        }

        int u = 0, v = 0, prev = 0;
        for (int i = 1; i <= p[1]; i ++) {
            if (d[i] != prev) u ++;
            prev = d[i];
        }

        for (int i = p[1]; i <= n; i ++) {
            if (d[i] != d[i + 1]) v ++;
        }

        cout << max(u, v) << endl;
    }
}
```

**To be continued**
