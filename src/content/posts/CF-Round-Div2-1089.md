---
title: CF-Round-Div2-1089
published: 2026-03-30
description: 'CF Round div2 1089的各题题解'
image: ''
tags: [CF题解, 算法, CFRound]
category: '算法'
draft: false 
lang: ''
---

## A题 A Simple Sequence
[原题](https://codeforces.com/contest/2210/problem/A)
### 思路
题目中提到了要构造一个排列，满足：
$$
a_1 \bmod a_2 \ge a_2 \bmod a_3 \geq \ldots \ge a_{n-1} \bmod a_{n}, 
$$
很容易想到，对于一个排列来说，它的倒序排列必定符合这个要求

### 代码
```c++ title="A题题解 C++" {"注意格式化":20}
#include <bits/stdc++.h>

using namespace std;

int main() {
    int t;
    cin >> t;
    while (t --) {
        int n;
        cin >> n;
        vector<int> r(n);
        int j = 0;
        for (int i = n; i > 0; i --) {
            r[j] = i;
            j ++;
        }
        
        for (int i = 0; i < n; i ++) {
            cout << r[i];
            if (i != n - 1) cout << " ";
        }
        if (t != 0) {
            cout << endl;
        }
    }
}
```

## B题 Simply Sitting on Chairs
[原题](https://codeforces.com/contest/2210/problem/B)
### 思路
先理清题目涉及到的操作：
- 给出一个长度为 $n$ 的排列 $p$ 
- **从第1把椅子出发，到第i把椅子**

***移动到第 $i$ 把椅子时：***
- 如果第 $i$ 把椅子被标记了：游戏立即结束
- 如果第 $i$ 把椅子未被标记：玩家可以坐在第 $i$ 把椅子上，并标记第 $p[i]$ 把椅子并移动到下一把椅子上；**或者**：跳过当前的椅子，直接移动到下一把椅子上
  
> [!WARNING]
> 需要注意的是，标记的是第 $p[i]$ 把椅子，不是当前坐上去的椅子
> 
> **e.g.** 当 $p$ 为：$[4, 3, 2, 1]$ 时，如果坐在了第 $2$ 把椅子上，标记的是第 $p[2] = 3$ 把椅子（按照题意，这里的索引以 $1$ 为起始）

理清了上述点之后，按照题目的要求：“最大化坐下次数”，我们不难发现，对于满足条件：
$$
p[i] \le i
$$
的椅子来说，是可以计入答案中的，因为坐上第 $i$ 把椅子后，标记的 $p[i]$ 是已经坐过的，在后续不会出现，不会影响到我们后续结果的增加

所以我们只需要统计，有多少个椅子满足上述的条件即可

### 代码
```c++ title="B题题解 C++"
#include <bits/stdc++.h>

using namespace std;

int main() {
    int t;
    cin >> t;
    while (t--) {
        int n;
        cin >> n;

        int ans = 0;
        for (int i = 1; i <= n; i++) {
            int x;
            cin >> x;
            if (x <= i) ans++;
        }

        cout << ans << '\n';
    }
    return 0;
}
```
**To be continued**