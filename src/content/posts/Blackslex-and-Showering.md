---
title: Blackslex-and-Showering
published: 2026-05-16
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法, 每日一题]
category: '算法'
draft: false 
lang: ''
---
## 题面
After his IMO medalist friend showered for two hours so that "he doesn't have to shower this again this week," Blackslex will be late for class!

In order to get to class, Blackslex must take the crowded elevator to many floors in a certain order. Because he is a hacker, he can skip visiting up to one floor without the other people knowing. His time taken is the sum of the absolute differences between consecutive floor numbers. Find the minimum time taken, given that he can skip up to one floor.

More formally, given an array $a = [a_1, a_2, \ldots, a_n]$ of $n$ integers, you can choose up to one index $k \in \{1, 2, \ldots, n\}$ to erase such that the sum

$$
\sum_{i=1}^{n-2} |b_i - b_{i+1}|
$$
 is minimized, where $b = [a_1, \ldots, a_{k-1}, a_{k+1}, \ldots, a_n]$ is the array after erasing element $a_k$. Report the minimum sum.

**Input**

The first line contains one integer $t$ ($1 \le t \le 10^4$) — the number of test cases.

The first line of each test case contains one integer $n$ ($3 \le n \le 2 \cdot 10^5$) — the size of the array.

The second line contains $n$ integers $a_1, a_2, \ldots, a_n$ ($1 \le a_i \le 100$).

It is guaranteed that the sum of $n$ does not exceed $2 \cdot 10^5$ over all test cases.

**Output**

For each test case, output a single real number — the minimum time taken.
## 思路
先计算不删除时的总耗时 $S=\sum_{i=1}^{n-1}\lvert a_i-a_{i+1}\rvert$\
\
删除一个位置 $k$ 只会改变与 $a_k$ 相邻的边 其余项保持不变\
\
进行讨论：
- 若 $k=1$ 或 $k=n$ 仅移除一条边 得到 $S'=S-\lvert a_1-a_2\rvert$ 或 $S'=S-\lvert a_{n-1}-a_n\rvert$\
- 若 $1<k<n$ 原来经过 $a_k$ 的贡献为 $\lvert a_{k-1}-a_k\rvert+\lvert a_k-a_{k+1}\rvert$ 删除后改为跨越边 $\lvert a_{k-1}-a_{k+1}\rvert$

因而 $S'=S-\lvert a_{k-1}-a_k\rvert-\lvert a_k-a_{k+1}\rvert+\lvert a_{k-1}-a_{k+1}\rvert$\
定义节省量 $\mathrm{save}(k)=S-S'$ 中间位置有 $\mathrm{save}(k)=\lvert a_{k-1}-a_k\rvert+\lvert a_k-a_{k+1}\rvert-\lvert a_{k-1}-a_{k+1}\rvert\ge 0$ 由三角不等式成立\
因此目标等价为最大化 $\mathrm{save}(k)$ 最终答案为 $S-\max_k \mathrm{save}(k)$
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
        vector<int> a(n);
        for (int i = 0; i < n; i ++) {
            cin >> a[i];
        }

        long long sum = 0;
        for (int i = 0; i + 1 < n; i ++) {
            sum += llabs((long long)a[i] - a[i + 1]);
        }

        long long best = 0;
        best = max(best, llabs((long long)a[0] - a[1]));
        best = max(best, llabs((long long)a[n - 2] - a[n - 1]));

        for (int k = 1; k + 1 < n; k ++) {
            long long l = llabs((long long)a[k - 1] - a[k]);
            long long r = llabs((long long)a[k] - a[k + 1]);
            long long b = llabs((long long)a[k - 1] - a[k + 1]);
            best = max(best, l + r - b);
        }

        cout << (sum - best) << "\n";
    }
}

```