---
title: Replace-and-Sum
published: 2026-04-14
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法, 每日一题]
category: '算法'
draft: false 
lang: ''
---

## 题面
Today, KQ has an exam at the Grail Academy. A strict teacher gave a task that KQ could not solve. He was given two arrays $a$ and $b$ of length $n$. KQ is allowed to perform the following operations on the arrays:

-   Choose an index $i$ ($1\le i\lt n$) and replace $a_i$ with $a_{i+1}$.
-   Choose an index $i$ ($1\le i\le n$) and replace $a_i$ with $b_i$.

Now he has $q$ queries. Each query is described by two numbers $l$ and $r$. His task is to find the maximum value of the sum $(a_l + a_{l+1} + a_{l+2} + \dots + a_r)$ for each query, if he can perform any number of operations on any elements of the array. Since he is not skilled enough for this, he asks for your help.

**Input**

Each test consists of several test cases. The first line contains one integer $t$ ($1\le t\le 10^4$) — the number of test cases. The description of the test cases follows.

The first line of each test case contains two integers $n$, $q$ $(1\le n, q\le 2 \cdot 10^5)$.

The second line of each test case contains $n$ integers $a_1, a_2,...,a_n$ $(1\le a_i\le 10^4)$.

The third line of each test case contains $n$ integers $b_1, b_2,...,b_n$ $(1\le b_i\le 10^4)$.

The following $q$ lines contain two integers $l$ and $r$ $(1\le l\le r\le n)$.

It is guaranteed that the sum of the values of $n$ and the sum of the values of $q$ across all test cases do not exceed $2 \cdot 10^5$.

**Output**

For each test case, output $q$ numbers separated by spaces — the maximum values of the sums $(a_l + a_{l+1} + a_{l+2} + \dots + a_r)$.
## 思路
理清题意，操作有两种类型：

- 选择索引 $i$，并赋值：$a_i = a_{i + 1}$
- 选择索引 $i$，并赋值：$a_i = b_i$

而题目的要求是让KQ进行若干次上述操作，使得对于给出的查询 $l, r$ 能够使 $a_l + a_{l+1} + a_{l+2} + \dots + a_r$ **最大**

可以注意到，对于一个固定位置 $i$，它修改后的值来源于自己右边或者是 $b$ 中的同位置元素

因此我们可以定义答案数组 $F$，并从右往左进行：

对于 $F_n$，它右边不可能存在数字，所以 $F_n = \max(a_n, b_n)$

对于 $F_i 1 \leq i \le n$，要考虑自己右边的情况，所以 $F_i = \max(a_i, b_i, F_{i + 1})$

所以最终我们所求的那个最大值就是
$$
\sum_{i = l}^r F_i
$$

为了降低时间复杂度，我们使用前缀和进行预处理，将查询的时间复杂度降低到 $O(1)$

> [!NOTE]
> 为什么要降低时间复杂度？
> 
> 注意到本题的数据范围很大，而如果对每个查询都循环区间，时间复杂度就是 $O(r - l + 1)$
>
> 总的复杂度会变成 $O(nq)$，有很大概率发生 `TLE`
>
> 所以我们需要一个时间复杂度更低的方法拿到结果

所以定义前缀和数组 $P$，有如下：
$$
P_0 = 0, P_i = P_{i - 1} + F_i
$$

所以：
$$
\sum_{i = l}^r F_i = P_r - P_{l - 1}
$$
## 代码
```c++
#include <bits/stdc++.h>

using namespace std;

int main() {
    int t;
    cin >> t;
    while (t --) {
        int n, q;
        cin >> n >> q;
        vector<int> a(n), b(n);
        for (int i = 0; i < n; i ++) cin >> a[i];
        for (int i = 0; i < n; i ++) cin >> b[i];

        vector<long long> F(n), P(n + 1, 0);

        F[n - 1] = max(a[n - 1], b[n - 1]);
        for (int i = n - 2; i >= 0; i --) {
            F[i] = max((long long)max(a[i], b[i]), F[i + 1]);
        }
        
        for (int i = 0; i < n; i ++) {
            P[i + 1] = P[i] + F[i];
        }
        
        for (int i = 0; i < q; i ++) {
            int l, r;
            cin >> l >> r;
            l --, r --;
            cout << P[r + 1] - P[l] << " \n"[i == q - 1];
        }
    }
}
```