---
title: Ashmal
published: 2026-05-14
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法, 每日一题]
category: '算法'
draft: false 
lang: ''
---
## 题面
You have an array $a$ of $n$ strings $a_{1}, a_{2}, \ldots, a_{n}$, each consisting of lowercase English letters, and an empty string $s$.

In the $i$\-th ($1 \le i \le n$) step, you should do one of the following:

-   add $a_{i}$ to the beginning of $s$, or
-   add $a_{i}$ to the end of $s$.

For example, if before the $i$\-th step $s = \mathtt{aba}$ and $a_{i} = \mathtt{bba}$, after the $i$\-th step, $s$ will be equal to $\mathtt{ababba}$ or $\mathtt{bbaaba}$.

What's the lexicographically smallest string $s$ you can reach after $n$ steps?

A string $a$ is lexicographically smaller than a string $b$ of the same length, if and only if the following holds:

-   in the first position where $a$ and $b$ differ, the string $a$ has a letter that appears earlier in the alphabet than the corresponding letter in $b$.

**Input**

Each test contains multiple test cases. The first line contains the number of test cases $t$ ($1 \le t \le 500$). The description of the test cases follows.

The first line of each test case contains a single integer $n$ ($1 \le n \le 1000$) — size of array $a$. The next line contains $n$ strings $a_{1}, a_{2}, \ldots, a_{n}$ ($1 \le |a_i| \le 4000$), each consisting of lowercase English letters.

It is guaranteed that the sum of $n$ over all test cases does not exceed $1000$, and **the total length of all strings in the input** (over all test cases) does not exceed $4000$.

**Output**

For each test case, print the lexicographically minimum string $s$ you can reach after $n$ steps.
## 思路
设处理到第 $i$ 步后的当前字符串为 $s$，本步只有两种选择 $a_i+s$ 或 $s+a_i$，目标是使最终字符串字典序最小\
\
关键性质 若 $x \le y$（字典序），则对任意固定字符串 $c$ 有 $cx \le cy$ 且 $xc \le yc$，即同前缀或同后缀拼接保持字典序\
\
因此对固定 $a_i$，函数 $F(s)=\min(a_i+s,\ s+a_i)$ 关于 $s$ 单调不减\
\
令 $best_i$ 为前 $i$ 个字符串操作后可达的字典序最小结果，有递推:
$$best_i = \min\left(best_{i-1}+a_i,\ a_i+best_{i-1}\right)$$

> [!NOTE]
> **证明**
> 
> 用归纳，任取上一步可达的任意 $s$，都有 $best_{i-1}\le s$，由单调性得 $F(best_{i-1})\le F(s)$，故本步从最小状态转移得到的即为全局最小\
> 算法 初始化 $s=\epsilon$，依次读入 $a_i$ 并令 $s=\min(s+a_i,\ a_i+s)$，输出最终 $s$\
> 复杂度 设该测试用例总字符数为 $L$，每步构造并比较两次拼接，整体可视为 $O(nL)$，在约束 $\sum L \le 4000$ 下足够
