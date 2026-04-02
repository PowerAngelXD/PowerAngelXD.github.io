---
title: DBMB-and-the-Array
published: 2026-04-02
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法, 每日一题]
category: '算法'
draft: false 
lang: ''
---
## 题面
DBMB had a birthday yesterday. He was gifted an array $a$ of $n$ elements and a number $x$. But there is one problem: he only likes arrays where the sum of the elements equals $s$. To make the array appealing to him, you can perform the following operation any number of times:

-   Choose an index $i$ ($1 \le i \le n$) and add $x$ to the number $a_i$.

For example, if he was given the array $[1, 2, 3, 5]$ and $x = 2$, you can choose index $3$ and get the array $[1, 2, 5, 5]$. Your task is to determine whether the array can appeal to DBMB after any number of operations.

**Input**

Each test consists of several test cases. The first line contains a single integer $t$ ($1 \le t \le 1000$) — the number of test cases. The following describes the test cases.

The first line of each test case contains three integers $n$, $s$, $x$ ($1 \le n, x \le 10$, $1 \le s \le 100$).

The second line of each test case contains $n$ integers $a_1, a_2, \dots a_n$ ($1 \le a_i \le 10$) — the elements of the array gifted to DBMB.

**Output**

For each test case, output "YES" if the array can appeal to DBMB. Otherwise, output "NO".

You can output each letter in any case (lowercase or uppercase). For example, the strings "yEs", "yes", "Yes", and "YES" will be accepted as a positive answer.
## 思路
题目的意思是对一个数组任意一个元素加上给定的 $x$ 若干次，检查得到的最终数组的各元素之和是否为想要的目标数 $s$

也就是说，当这个数组的初始和为 $S_0$ 时，经过若干次操作之后，一定满足：
$$
S = S_0 + kx, k \geq 0
$$
为了求出这个符合条件的 $k$，进行移项，得到：
$$
k = \frac{S - S_0}{x}
$$
若 $k$ 合法，则一定满足下面的条件：
- $S \geq S_0$
- $(S - S_0)\space mod \space x = 0$（$S$ 与 $S_0$的差必须能被 $x$ 整除）
如果 $k$ 能达到上述条件，那么就能够出现这样一个方式使得最终数组的和为期望的 $s$
## 代码
```c++
#include <bits/stdc++.h>

using namespace std;

int sum(vector<int>& a) {
    int result = 0;
    for (auto& n: a) {
        result += n;
    }
    return result;
}

int main() {
    int t;
    cin >> t;
    while (t --) {
        int n, s, x;
        cin >> n >> s >> x;
        vector<int> a(n + 1);
        for (int i = 1; i <= n; i ++) cin >> a[i];

        int cur = sum(a);
        if (cur <= s && (s - cur) % x == 0) {
            cout << "YES" << endl;
        }
        else {
            cout << "NO" << endl;
        }
    }
}
```