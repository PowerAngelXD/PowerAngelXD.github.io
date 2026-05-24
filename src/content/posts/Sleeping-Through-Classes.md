---
title: Sleeping-Through-Classes
published: 2026-05-24
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法, 每日一题]
category: '算法'
draft: false 
lang: ''
---
## 题面
You have $n$ classes today, which are numbered from $1$ to $n$.

The classes are described by a binary string$^{\text{∗}}$ $s$ of length $n$. We call class $i$ important **if and only if** $s_i = \mathtt 1 $ . For each important class, you **must** stay awake and listen to it.

You are very tired and wish to sleep through as many classes as possible. However, falling asleep takes time. If you listen to an important class $i$, then you cannot fall asleep for the next $k$ classes, i.e., you must also stay awake in classes $i+1, i+2, \ldots, i+k$ (or until the end of the day, if fewer than $k$ classes remain).

For classes that are not important, you may sleep through them unless the rule above forces you to stay awake.

Your task is to find out the maximum number of classes you can sleep through today.

$^{\text{∗}}$A binary string is a string where each character is either $\mathtt{0}$ or $\mathtt{1}$.

**Input**

Each test contains multiple test cases. The first line contains the number of test cases $t$ ($1 \le t \le 500$). The description of the test cases follows.

The first line of each test case contains two integers $n$ and $k$ ($1 \le n, k \le 100$).

The second line of each test case contains the string $s$ of length $n$ ($s_i = \mathtt 0$ or $\mathtt 1$).

**Output**

For each test case, output a single integer — the maximum number of classes you can sleep through today.
## 思路
重要课位置固定必须清醒，因此只需统计哪些课被强制清醒\
\
每个 $s_i=\mathtt{1}$ 会使区间 $[i,\min(n,i+k)]$ 全部清醒，区间并集大小记为 $\text{awake}$\
\
可以得到结论：答案为 $n-\text{awake}$，其本质是一个贪心
## 代码
```c++
#include <bits/stdc++.h>

using namespace std;

int main() {
    int t;
    cin >> t;
    while (t --) {
        int n,k;
        cin >> n >> k;
        string s;
        cin >> s;

        int b = -1;
        int a = 0;
        for (int i = 0; i < n; i ++) {
            if (s[i] == '1') {
                a ++;
                b = max(b, i + k);
            } 
            else if (i <= b) {
                a ++;
            }
        }

        cout << n - a << endl;
    }
    return 0;
}

```