---
title: Array-and-Permutation
published: 2026-03-25
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法]
category: '算法'
draft: false 
lang: ''
---
## 题面
Given a permutation $p$ of length $n$ $^{\text{∗}}$ and an array $a$ of length $n$.

We call the permutation $p$ generating for the array $a$ if the array $a$ can be obtained from the permutation $p$ by applying some number of operations (possibly zero) of the following type:

Choose an index $i$ ($1 \le i \lt n$) and perform one of two replacements:

-   $p_{i} := p_{i + 1}$;
-   $p_{i + 1} := p_{i}$.

In other words, in one operation, you can choose two adjacent elements of the array and copy the value of one into the other.

You are required to report whether the permutation $p$ is generating for the array $a$.

$^{\text{∗}}$A permutation of length $n$ is an array consisting of $n$ distinct integers from $1$ to $n$ in arbitrary order. For example, $[2,3,1,5,4]$ is a permutation, but $[1,2,2]$ is not a permutation ($2$ appears twice in the array), and $[1,3,4]$ is also not a permutation ($n=3$ but there is $4$ in the array).

**Input**

Each test contains multiple test cases. The first line contains the number of test cases $t$ ($1 \le t \le 10^4$). The description of the test cases follows.

The first line of each test case contains a single integer $n$ ($2 \le n \le 2 \cdot 10^{5}$) — the length of the array and the permutation.

The second line of each test case contains $n$ integers $p_1, p_2, \ldots, p_n$ ($1 \le p_{i} \le n$) — the elements of the permutation.

The third line of each test case contains $n$ integers $a_1, a_2, \ldots, a_n$ ($1 \le a_{i} \le n$) — the elements of the array.

It is guaranteed that the sum of $n$ across all test cases does not exceed $2 \cdot 10^{5}$.

**Output**

For each test case, output "YES" if the permutation $p$ is generating for the array $a$, otherwise output "NO".

You may output each letter in any case (lowercase or uppercase). For example, the strings "yEs", "yes", "Yes", and "YES" will be accepted as a positive answer.

## 思路
阅读题目，注意到我们无法创造新的数字，只能用存在的数字进行复制替换

因此，在若干次操作后，整个数组会变成一个有很多种重复数字的块

而由题目的“生成排列”的定义可知：这个生成排列本质是从给定的排列 $p$ 中得到的子序列

因此，我们需要对数组的块进行压缩：

```c++
vector<int> b;
b.reserve(n);
for (int x : a) {
    if (b.empty() || b.back() != x) b.push_back(x);
}
```

压缩之后，我们就需要判断给定的排列是否为得到压缩后的排列 $b$ 的子序列，如果是则输出 “YES”

在这里，我们使用双指针法，当找到了子序列中的元素时，另一指针 $j$ 自增，如果最后 $j = b.size()$，则说明子序列找到了，使情况得到满足：

```c++
int j = 0;
for (int x : p) {
    if (j < (int)b.size() && x == b[j]) ++j;
}
cout << (j == (int)b.size() ? "YES" : "NO") << '\n';
```

## 代码
```c++
#include <bits/stdc++.h>
using namespace std;

int main() {
    int t;
    cin >> t;
    while (t--) {
        int n;
        cin >> n;

        vector<int> p(n), a(n);
        for (int i = 0; i < n; ++i) cin >> p[i];
        for (int i = 0; i < n; ++i) cin >> a[i];

        vector<int> b;
        b.reserve(n);
        for (int x : a) {
            if (b.empty() || b.back() != x) b.push_back(x);
        }

        int j = 0;
        for (int x : p) {
            if (j < (int)b.size() && x == b[j]) ++j;
        }

        cout << (j == (int)b.size() ? "YES" : "NO") << '\n';
    }

    return 0;
}
```