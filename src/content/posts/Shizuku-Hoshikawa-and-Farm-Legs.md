---
title: Shizuku-Hoshikawa-and-Farm-Legs
published: 2026-05-26
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法, 每日一题]
category: '算法'
draft: false 
lang: ''
---
## 题面
Nothing's ever been the same since... that summer with her.

— Shizuku Hoshikawa

Kaori wants to spend the day with Shizuku! However, the zoo is closed, so they are visiting Farmer John's farm instead.

At Farmer John's farm, Shizuku counts $n$ legs. [It is known that](https://codeforces.com/contest/1996/problem/A) only chickens and cows live on the farm; a chicken has $2$ legs, while a cow has $4$.

Count how many different configurations of Farmer John's farm are possible. Two configurations are considered different if they contain either a different number of chickens, a different number of cows, or both.

Note that Farmer John's farm may contain zero chickens or zero cows.

**Input**

The first line contains a single integer $t$ ($1 \leq t \leq 100$)  — the number of test cases.

The only line of each test case contains a single integer $n$ ($1\leq n \leq 100$).

**Output**

For each test case, output a single integer, the number of different configurations of Farmer John's farm that are possible.
## 思路
根据题意列出方程： $2x + 4y = n$，设 $x$ 为鸡的数量，$y$ 为牛的数量\
\
很明显的一个情况，$n$ 为奇数的时候，答案必定为 $0$\
\
而如果为偶数，$y$ 可以从 $0$ 取到 $\frac{n}{4}$，所以方案数量为 $\frac{n}{4} + 1$

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

        if (n % 2 != 0) cout << 0 << endl;
        else cout << n / 4 + 1 << endl;
    }
}

```