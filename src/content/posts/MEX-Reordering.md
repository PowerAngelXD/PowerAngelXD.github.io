---
title: MEX-Reordering
published: 2026-05-06
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法, 每日一题]
category: '算法'
draft: false 
lang: ''
---
## 题面
You are given an integer array $a$ consisting of $n$ elements. Denote $f(l, r) = \operatorname{MEX}([a_l, a_{l + 1}, \ldots, a_r])$ $^{\text{∗}}$.

Determine if there is a way to reorder the array $a$ such that for **every** $i$ ($1 \le i \le n - 1$), $f(1, i) \neq f(i + 1, n)$. In other words, for every split point $i$, the $\operatorname{MEX}$ of the prefix must be different from the $\operatorname{MEX}$ of the suffix.

$^{\text{∗}}$The minimum excluded (MEX) of a collection of integers $c_1, c_2, \ldots, c_k$ is defined as the smallest non-negative integer $x$ which does not occur in the collection $c$.

**Input**

Each test contains multiple test cases. The first line contains the number of test cases $t$ ($1 \le t \le 500$). The description of the test cases follows.

The first line of each test case contains a single integer $n$ ($2 \le n \le 100$) — the length of the array.

The second line of each test case contains $n$ integers $a_1, a_2, \ldots, a_n$ ($0 \le a_i \le n$).

**Output**

Output "YES" if you can reorder $a$ so that the condition from the statement is satisfied, and "NO" otherwise. You can output the answer in any case (upper or lower). For example, the strings "yEs", "yes", "Yes", and "YES" will be recognized as positive responses.
## 思路

先理解 **MEX** 的含义

MEX 是**最小没有出现的非负整数**

例如 $\operatorname{MEX}([1,2,3])=0$

例如 $\operatorname{MEX}([0,2,4])=1$

例如 $\operatorname{MEX}([0,1,3])=2$

本题要重排数组，使得对每个切分点 $i\,(1\le i\le n-1)$ 都满足

$\operatorname{MEX}(a_1,\ldots,a_i)\ne \operatorname{MEX}(a_{i+1},\ldots,a_n)$

**关键观察**

MEX 是否等于 $0$ 或 $1$ 只取决于 $0$ 和 $1$ 有没有出现

如果一段里没有 $0$，那么这一段的 MEX 一定是 $0$

如果一段里有 $0$ 但没有 $1$，那么这一段的 MEX 一定是 $1$

如果一段里同时有 $0$ 和 $1$，那么这一段的 MEX 至少是 $2$

因此只需要统计数组里 $0$ 的个数 $\text{cnt0}$ 和 $1$ 的个数 $\text{cnt1}$ 就能判定答案


**分情况讨论**

- 情况一 $\text{cnt0}=0$

无论怎么重排，任意前缀和后缀都不包含 $0$，因此它们的 MEX 都是 $0$，一定会出现相等

答案是 **NO**

- 情况二 $\text{cnt0}=1$

把唯一的 $0$ 放到最后一个位置即可

此时对任意 $i<n$，前缀不含 $0$，所以前缀 MEX 为 $0$，而后缀一定包含这个 $0$，所以后缀 MEX 大于 $0$，两边永远不可能相等

答案是 **YES**

- 情况三 $\text{cnt0}\ge 2$

如果 $\text{cnt1}=0$

只要一段里出现了 $0$，由于没有 $1$，这段的 MEX 就会变成 $1$；不管怎么重排，总能找到一个切分点让左右两边都至少有一个 $0$，此时前缀 MEX 和后缀 MEX 都是 $1$，违反条件

答案是 **NO**

如果 $\text{cnt1}>0$

可以构造一种重排来保证所有切分点都不相等

把所有 $0$ 放在数组末尾，并确保至少一个 $1$ 在这些 $0$ 的前面

切在第一个 $0$ 之前时，前缀不含 $0$，所以前缀 MEX 为 $0$

后缀含 $0$，所以后缀 MEX 不会是 $0$

切在 $0$ 区间内部或之后时，后缀会变成只包含若干个 $0$ 的集合，后缀 MEX 为 $1$

而前缀因为已经包含 $0$ 且 $1$ 在 $0$ 之前，所以前缀同时包含 $0$ 和 $1$，前缀 MEX 至少为 $2$

因此两边仍然不相等

答案是 **YES**

**综上**

只有两种情况输出 **NO**：
- $\text{cnt0}=0$
- $\text{cnt1}=0$ 且 $\text{cnt0}\ge 2$
- 
其余情况全部输出 **YES**

时间复杂度为 $O(n)$，只需要一次遍历统计即可

