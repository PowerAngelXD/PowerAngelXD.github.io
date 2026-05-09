---
title: Shifted-MEX
published: 2026-05-09
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法, 每日一题]
category: '算法'
draft: false 
lang: ''
---
## 题面
You are given an array of $n$ integers $a_1, a_2, \ldots, a_n$. You are allowed to perform the following operation once.

-   Select an integer $x$ (which may be negative), and for each value $i$ $(1 \leq i \leq n)$, set $a_i = a_i + x$.

For example, if $a = [1, 3, 4, 2]$, and you perform the operation with $x = 3$, $a$ is now equal to $[4, 6, 7, 5]$.

Output the maximum possible value of $\operatorname{MEX}(a)$ $^{\text{∗}}$ after the operation is performed.

$^{\text{∗}}$$\operatorname{MEX}(a)$ is defined as the smallest non-negative integer that is not present in the array. For example, $\operatorname{MEX}([1, 2, 0, 5])$ is $3$, and $\operatorname{MEX}([1, 2, 4, 9])$ is $0$.

**Input**

The first line of the input contains a single integer $t$ ($1 \leq t \leq 1000$) — the number of test cases.

The first line of each test case contains a single integer $n$ ($1 \le n \le 3000$) — the length of array $a$.

The second line contains $n$ integers $a_1, a_2, \ldots, a_n$ ($-10^9 \le a_i \le 10^9$) — the array $a$.

It is guaranteed that the sum of $n$ over all test cases does not exceed $3000$.

**Output**

For each test case, output the maximum possible value of $\operatorname{MEX}(a)$ after the operation has been performed.

## 思路
理清题意：

本题允许把所有元素同时加上同一个整数 $x$，因此数组中元素之间的相对差值不变 \
设操作后数组为 $b_i=a_i+x$，若希望 $\operatorname{MEX}(b)\ge k$，则 $0,1,\ldots,k-1$ 必须都在 $b$ 中出现 \
把条件换回原数组可得，$a$ 中必须包含： $-x,-x+1,\ldots,-x+(k-1)$ 这一段长度为 $k$ 的连续整数 \
反过来若 $a$ 中存在连续段 $y,y+1,\ldots,y+(L-1)$，选择 $x=-y$ 就能把它整体平移成 $0,1,\ldots,L-1$，从而保证 $\operatorname{MEX}(b)\ge L$ \
因此最大可得的 $\operatorname{MEX}$ 恰好等于原数组去重后最长连续整数段的长度 

因此，我们先对数组排序并去重，因为 $\operatorname{MEX}$ 只关心某个值是否出现过，重复元素不会带来额外贡献 \
线性扫描去重后的有序数组，用 $cur$ 表示当前连续段长度，用 $best$ 表示历史最大值

当相邻两个数满足 $v_i=v_{i-1}+1$ 时令 $cur$ 增加 $1$，否则将 $cur$ 重置为 $1$，并持续更新 $best$ \
最终输出 $best$ 即为答案

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
        vector<long long> a(n);
        for (int i = 0; i < n; i ++) cin >> a[i];

        sort(a.begin(), a.end());
        a.erase(unique(a.begin(), a.end()), a.end());

        int best = 0;
        int cur = 0;
        for (size_t i = 0; i < a.size(); i ++) {
            if (i == 0 || a[i] != a[i - 1] + 1) cur = 1;
            else cur ++;
            best = max(best, cur);
        }

        cout << best << endl;
    }
    return 0;
}

```