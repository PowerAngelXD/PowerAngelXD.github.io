---
title: Blackslex-and-Password
published: 2026-05-17
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法, 每日一题]
category: '算法'
draft: false 
lang: ''
---
## 题面
Blackslex is designing a log-in system for Gean Dev and discovered that most users use weak passwords.

To resolve this issue, he posed the following conditions, dependent on two variables $k$ and $x$, for all passwords. Each password is a string $s$ of length $n$ satisfying these properties.

-   $s$ uses only the first $k$ lowercase letters of the English alphabet.
-   For every pair of indices $1 \le i \lt j \le n$ such that $(j-i)$ is divisible by $x$, the letters $s_i$ and $s_j$ are different.

Find the smallest integer $n$ such that no valid string of length $n$ exists.

**Input**

The first line contains a single integer $t$ ($1 \le t \le 500$) — the number of test cases.

The first and only line of each test case contains two integers $k$ and $x$ ($1 \le k \le 26$, $1 \le x \le 15$).

**Output**

For each test case, output the minimal $n$.

## 思路
将下标按 $i\bmod x$ 分组，若 $(j-i)$ 可被 $x$ 整除则 $i\equiv j\pmod x$，因此约束仅发生在同一组内\
\
同一组内任意两位置字符必须不同，因此每组最多容纳 $k$ 个位置，否则鸽巢原理必有重复字母\
\
长度为 $n$ 时共有 $x$ 组，各组大小为 $\lfloor n/x\rfloor$ 或 $\lceil n/x\rceil$，最大组大小为 $\lceil n/x\rceil$\
\
可行当且仅当 $\lceil n/x\rceil\le k$，等价于 $n\le kx$\
\
当 $n=kx$ 时可构造，每个余数组依次填入 $k$ 个不同字母，组与组之间允许重复\
因此最小不可行长度为 $kx+1$，直接输出 $n=kx+1$ 即可

## 代码
```c++
#include <bits/stdc++.h>

using namespace std;

int main() {
    int t;
    cin >> t;
    while (t --) {
        int k, x;
        cin >> k >> x;

        int ans = k * x + 1;

        cout << ans << endl;
    }
}

```
