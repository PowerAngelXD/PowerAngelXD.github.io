---
title: Impost-or-Sus
published: 2026-05-18
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法, 每日一题]
category: '算法'
draft: false 
lang: ''
---
## 题面
A string $w$ consisting of lowercase Latin letters is called suspicious if and only if all of the following conditions hold:

-   The letter $\mathtt{s}$ appears at least twice, and
-   For every occurrence of the letter $\mathtt{u}$, the two nearest occurrences of $\mathtt{s}$ are the same number of characters away from the $\mathtt{u}$.

After watching you finish a string task, your friend Aka has gifted you a string $r$ consisting only of letters $\mathtt{s}$ and $\mathtt{u}$. You can perform the following operation on $r$:

-   Choose an index $i$ ($1\le i\le |r|$), and set $r_i$ to $\mathtt{s}$.

Determine the minimum number of operations needed to make $r$ suspicious. It can be shown that, under the given constraints, it is always possible to transform $r$ into a suspicious string.

**Input**

Each test contains multiple test cases. The first line contains the number of test cases $t$ ($1 \le t \le 10^4$). The description of the test cases follows.

The only line of each test case contains the string $r$ ($3\le |r|\le 2\cdot 10^5$). It is guaranteed that $r_i = \mathtt{s}$ or $\mathtt{u}$.

It is guaranteed that the sum of $|r|$ over all test cases does not exceed $2\cdot 10 ^ 5$.

**Output**

For each test case, output a single integer — the minimum number of operations needed to make $r$ suspicious.
## 思路
将目标串记为 $w$ 且只含 $\mathtt{s},\mathtt{u}$ 操作等价于选择一些位置把 $\mathtt{u}\to\mathtt{s}$\
\
若 $w_1=\mathtt{u}$ 或 $w_{|w|}=\mathtt{u}$ 则最近的两次 $\mathtt{s}$ 只能在同一侧，而到端点的距离必不相等，因此首尾必须为 $\mathtt{s}$\
\
对任意位置 $i$ 的 $\mathtt{u}$ 设其左右最近的 $\mathtt{s}$ 与它的距离同为 $d$ 则这两个 $\mathtt{s}$ 必在 $i-d$ 与 $i+d$\
\
若 $d>1$ 则 $i+1$ 也是 $\mathtt{u}$ 且它到两侧最近 $\mathtt{s}$ 的距离分别为 $d-1$ 与 $d+1$ 不相等，矛盾\
\
故必有 $d=1$ ，即每个 $\mathtt{u}$ 都被形如 $\mathtt{sus}$ 包住 等价于 $w$ 不包含子串 $\mathtt{uu}$\
\
综上，满足：suspicious 的充要条件为：首尾为 $\mathtt{s}$ 且不存在相邻的 $\mathtt{u}$