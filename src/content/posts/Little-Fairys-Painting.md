---
title: Little-Fairy's-Painting
published: 2026-05-23
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法, 每日一题]
category: '算法'
draft: false 
lang: ''
---
## 题面
The little fairy has a ribbon with $10^{18}$ cells and an infinite variety of colors. The first $n$ cells of the ribbon have already been colored, where the $i$\-th cell is colored with color $a_i$.

Little fairy will color the remaining cells in order from $n+1$ to $10^{18}$. For the $i$\-th cell:

-   Little fairy first counts the number of distinct colors currently present on the ribbon, denoted as $c_i$.
-   Then she will color the $i$\-th cell with color $c_i$.

What color will the fairy use for the $10^{18}$\-th cell?

**Input**

Each test contains multiple test cases. The first line contains the number of test cases $t$ ($1 \le t \le 500$). The description of the test cases follows.

The first line of each test case contains a single integer $n$ ($1 \le n \le 100$) — the number of colored cells.

The second line of each test case contains $n$ integers $a_1,a_2,\ldots,a_n$ ($1 \le a_i \le 10^3$) — the colors of the cells.

**Output**

For each test case, print the color of the last cell.
## 思路
设初始出现过的颜色集合为 $S$ 且 $k=|S|$ \
对于任意一步，若当前不同颜色数为 $k$ 则新染色为 $k$ \
\
若 $k\in S$，则本次不引入新颜色且之后每步仍染 $k$ 因而答案为 $k$ \
若 $k\notin S$，则加入新颜色 $k$，使集合大小变为 $k+1$ 即 $k\leftarrow k+1$ \
\
因此，$k$ 单调递增直到首次满足 $k\in S$ 为止 \
实现上从 $k=|S|$ 开始循环检查，若不在则插入并 $k\leftarrow k+1$ 否则输出 $k$ \
## 代码
```c++
#include <bits/stdc++.h>

using namespace std;

int main() {
    int t;
    cin >> t;
    while (t --) {
        int n;
        cin >> n;

        unordered_set<int> c;
        c.reserve(n * 2 + 10);
        for (int i = 0; i < n; i ++) {
            int x;
            cin >> x;
            c.insert(x);
        }

        int k = (int)c.size();
        while (true) {
            if (c.count(k)) {
                cout << k << endl;
                break;
            }
            c.insert(k);
            k ++;
        }
    }
}
```