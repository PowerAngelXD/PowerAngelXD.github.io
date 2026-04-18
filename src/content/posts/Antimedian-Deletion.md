---
title: Antimedian-Deletion
published: 2026-04-18
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法, 每日一题]
category: '算法'
draft: false 
lang: ''
---
# Antimedian Deletion
## 题面
You are given a permutation $^{\text{∗}}$ $p$ of size $n$. You may perform the following operation any number of times:

-   Choose a subarray $^{\text{†}}$ of size $3$. Then, delete either the smallest or largest element within it.

For example, for the permutation $[2,4,5,3,1]$, you may choose the subarray $[\mathbf{2},\mathbf{4},\mathbf{5},3,1]$. Since $5=\operatorname{max}(2,4,5)$. you can delete $5$ to obtain the array $[2,4,3,1]$. You may also choose to delete $2$ instead as $2=\operatorname{min}(2,4,5)$.

For each $i$ from $1$ to $n$, find the minimum length of an obtainable array that contains the number $p_i$. Note that this problem is to be solved independently for each $i$.

$^{\text{∗}}$A permutation of length $n$ is an array consisting of $n$ distinct integers from $1$ to $n$ in arbitrary order. For example, $[2,3,1,5,4]$ is a permutation, but $[1,2,2]$ is not a permutation ($2$ appears twice in the array), and $[1,3,4]$ is also not a permutation ($n=3$ but there is $4$ in the array).

$^{\text{†}}$An array $a$ is a subarray of an array $b$ if $a$ can be obtained from $b$ by the deletion of several (possibly, zero or all) elements from the beginning and several (possibly, zero or all) elements from the end.

**Input**

Each test contains multiple test cases. The first line contains the number of test cases $t$ ($1 \le t \le 500$). The description of the test cases follows.

The first line of each test case contains a single integer $n$ ($1 \le n \le 100$) — the length of the array.

The second line of each test case contains $n$ integers $p_1, p_2, \ldots, p_n$ ($1 \le p_i \le n$). It is guaranteed that each element from $1$ to $n$ appears exactly once.

**Output**

For each test case, output $n$ numbers on a new line: the answer for $i=1,2,\ldots,n$.
## 思路
看清题目要求：

在一个长度为 $n$ 的排列中反复进行下面的操作：
- 选一个长度为3的连续子数组，然后删掉这个数组里的最大值或者最小值

而很明显，由于这个操作给定的目标长度必须是3，而每次长度必定会 $-1$，导致最终长度必定不会小于2

所以，结论很容易出来：
- $n=1, ans=1$
- $n \leq 2, ans=2, 2, 2, 2 \dots$
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
        for (int i = 0; i < n; i ++) cin >> p[i];

        if (n == 1) cout << 1 << endl;
        else {
            for (int i = 0; i < n; i ++) {
                cout << 2 << " \n"[i == n - 1];
            }
        }
    }
    
}
```