---
title: Divisible-Permutation
published: 2026-04-10
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法, 每日一题]
category: '算法'
draft: false 
lang: '' 
---
## 题面
You are given an integer $n$. Construct a permutation $^{\text{∗}}$ $p$ of length $n$ satisfying the following condition:

-   $\lvert p_i - p_{i+1} \rvert$ is divisible by $i$ for every $1 \le i \le n-1$.

It can be proven that such a permutation always exists under the constraints of the problem.

$^{\text{∗}}$A permutation of length $n$ is an array consisting of $n$ distinct integers from $1$ to $n$ in arbitrary order. For example, $[2,3,1,5,4]$ is a permutation, but $[1,2,2]$ is not a permutation ($2$ appears twice in the array), and $[1,3,4]$ is also not a permutation ($n=3$ but there is $4$ in the array).

**Input**

Each test contains multiple test cases. The first line contains the number of test cases $t$ ($1 \le t \le 100$). The description of the test cases follows.

The only line of each test case contains a single integer $n$ ($2 \le n \le 100$) — the length of the permutation $p$ to be constructed.

**Output**

For each test case, output $n$ integers $p_1,p_2,\ldots,p_n$ ($1 \le p_i \le n$, all $p_i$\-s are distinct) — the permutation you constructed.

If there are multiple valid permutations, you may output any of them.
## 思路
这题是一道构造题，按照题意，我们需要一个排列，满足：

在条件：$1 \le i \le n-1$ 下， $\lvert p_i - p_{i+1} \rvert$ 都可被 $i$ 整除

因此，如果我们让这个排列满足： $\lvert p_i - p_{i+1} \rvert = i$，即可满足题目的要求

因此我们构造：$1, n, 2, n - 1, 3, n - 2, ...$ 即可

在构造的时候，我们使用双指针维护两端，最终得出结果
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
        vector<int> p(n);
        int l = 1, r = n;
        int i = n - 1;
        for (; i > 0;) {
            p[i] = l;
            i --;
            p[i] = r;
            i --;

            l ++;
            r --;
        }
        
        if (l == r) {
            p[i] = l;
        }

        for (int i = 0; i < n; i ++) {
            cout << p[i] << " \n"[i == n - 1];
        }
    }
}
```
