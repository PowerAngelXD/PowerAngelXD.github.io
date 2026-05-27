---
title: Maximum-Neighborhood
published: 2026-05-27
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法, 每日一题]
category: '算法'
draft: false 
lang: ''
---
## 题面
Consider an $n \times n$ grid filled with numbers as follows:

-   the first row contains integers from $1$ to $n$ from left to right;
-   the second row contains integers from $(n+1)$ to $2n$ from left to right;
-   this pattern continues until the $n$\-th row, which contains integers from $(n^2-n+1)$ to $n^2$ from left to right.

Let's define the cost of a cell as its value plus the sum of its neighboring cells' values. Two cells are considered neighboring if they share a side.

Your task is to calculate the maximum cost among all cells in the grid.

![](https://espresso.codeforces.com/a6ba7eedd87ca0c721dfc96741adf90a92e91d57.png) The grid for $n = 4$ and the optimal answer for it. The yellow cell has the maximum possible cost; the green cells are its neighbors. The cost of the cell is $15+11+14+16=56$.

**Input**

The first line contains a single integer $t$ ($1 \le t \le 100$) — the number of test cases.

The only line of each test case contains a single integer $n$ ($1 \le n \le 100$).

**Output**

For each test case, print a single integer — the maximum cost among all cells in the grid.
## 思路
设位置为第 $i$ 行第 $j$ 列则值为 $a(i,j)=(i-1)n+j$\
\
相邻格共享边因此最多 $4$ 个邻居且每个邻居与自身相差 $1$ 或 $n$\
\
最大值一定出现在靠右下的位置因为邻居与自身都更大
- 当 $n\le 2$ 可直接得答案为 $n=1\Rightarrow 1$ 以及 $n=2\Rightarrow 9$
- 当 $n\ge 3$ 只需比较右下角 $2\times 2$ 内的候选

令 $x=a(n-1,n-1)=n^2-n-1$ 则其代价为 $x+(x+1)+(x+n)+x+(x-1)=5x=5n^2-5n-5$\
\
令 $y=a(n,n-1)=n^2-1$ 则其代价为 $y+(y+1)+y+(y-n)=4y-n+1=4n^2-n-4$\
\
其余两格的邻居数更少且代价更小因此最大值为 $\max(5n^2-5n-5,\ 4n^2-n-4)$\
\
比较差值 $(5n^2-5n-5)-(4n^2-n-4)=n^2-4n-1$\
因此 $n\in\{3,4\}$ 时答案为 $4n^2-n-4$ 且 $n\ge 5$ 时答案为 $5n^2-5n-5$
## 代码
```c++
#include <bits/stdc++.h>

using namespace std;

int main() {
    int t;
    cin >> t;
    while (t --) {
        long long n;
        cin >> n;

        if (n == 1) cout << 1 << endl;
        else if (n == 2) cout << 9 << endl;
        else if (n == 3 || n == 4) cout << 4*pow(n, 2) - n - 4 << endl;
        else cout << 5*pow(n, 2) - 5 * n - 5 << endl;
    }
}
```