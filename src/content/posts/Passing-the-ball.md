---
title: Passing-the-ball
published: 2026-04-30
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法, 每日一题]
category: '算法'
draft: false 
lang: ''
---
## 题面
During a physical education class, $n$ students are lined up, numbered from $1$ to $n$ from left to right.

For each student, it is known that if they receive the ball, they will pass it either to the neighbor on their left or to the neighbor on their right. This is specified by a string $s$ of $n$ characters. Each character of the string is either L or R, where $s_i$ is L if the $i$\-th student passes the ball to student $(i-1)$, or $s_i$ is R if the $i$\-th student passes the ball to student $(i+1)$. The first student always passes the ball to the second, and the last one to the second last (in other words, the string $s$ starts with the character R and ends with the character L).

Consider the following process:

-   first, the first student receives the ball;
-   then, exactly $n$ times, the following occurs: the student who currently has the ball passes it to their neighbor (according to the rules described above).

Your task is to determine how many students will receive the ball at least once during this process.

**Input**

The first line contains a single integer $t$ ($1 \le t \le 10000$) — the number of test cases.

Each test case consists of two lines:

-   the first line contains a single integer $n$ ($2 \le n \le 50$) — the number of students;
-   the second line contains $s$ — a sequence of $n$ characters L and R. The first character of the sequence is R, and the last is L.
  
**Output**

For each test case, print one integer — the number of students who will receive the ball at least once during the described process.

## 思路
题目中每个学生拿到球后传给左/右邻居是**确定的**，所以我们可以把它看成一张“每个点只有一条出边”的有向图
从 1 号同学开始拿球，沿着边一直走，并统计在“初始接球 + 接下来恰好 $n$ 次传球”过程中到过多少个不同的点即可

为了让模拟更直接，先预处理一个“下一跳”数组 `nxt`：

- 定义 $nxt[i]\,(1 \le i \le n)$ 为：当第 $i$ 个学生拿到球时，**下一次传球后**球会到达的学生编号（也就是下一位接球者）
- 这样每次传球都可以写成一句：$cur \leftarrow nxt[cur]$，不需要反复判断边界和 `L/R`

预处理规则：

- 边界位置是固定的：$nxt[1]=2$，$nxt[n]=n-1$
- 对于中间位置 $2 \le i \le n-1$：
  - 若 $s_i=\texttt{'L'}$，则 $nxt[i]=i-1$
  - 若 $s_i=\texttt{'R'}$，则 $nxt[i]=i+1$

然后按题意模拟并记录出现过的人：

- 用 `visited` 记录是否接到过球。初始时 $cur=1$，先标记 `visited[1]=true`（因为 1 号一开始就接到球）
- 再进行恰好 $n$ 次传球：每次令 $cur \leftarrow nxt[cur]$，并标记 `visited[cur]=true`
- 最后统计 `visited` 为 `true` 的人数，就是答案

由于 $n \le 50$，每个测试用例直接模拟即可，时间复杂度为 $O(n)$
