---
title: Operations-with-Inversions
published: 2026-05-22
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法, 每日一题]
category: '算法'
draft: false 
lang: ''
---
## 题面
Given an array $a_1, a_2, \ldots, a_n$. In one operation, you can choose a pair of indices $i, j$ such that $1 \le i \lt j \le n$, $a_i \gt a_j$, and remove the element at index $j$ from the array. After that, the size of the array will decrease by $1$, and the relative order of the elements will not change.

Determine the maximum number of operations that can be performed on the array if they are applied optimally.

**Input**

Each test contains multiple test cases. The first line contains the number of test cases $t$ ($1 \le t \le 50$). The description of the test cases follows.

The first line of each test case contains an integer $n$ ($1 \le n \le 100$) — the size of the initial array.

The second line of each test case contains $n$ natural numbers $a_1, a_2, \ldots, a_n$ ($1 \le a_i \le n$).

**Output**

For each test case, output the maximum number of operations that you can perform on the given array.
## 思路
操作只会删除右端点 $j$ 的元素，数组相对顺序保持不变，因此每个位置是否可删只取决于它左侧是否出现过更大的值\
\
对位置 $j$ 而言，若存在 $i<j$ 使 $a_i>a_j$，则可选择这对 $(i,j)$ 直接删除 $a_j$\
反之，若 $a_j\ge \max(a_1,\ldots,a_{j-1})$，则左侧不存在更大值，后续只会删掉一些元素但不会凭空产生更大值，因此 $a_j$ 永远不可删\
\
因此答案等于满足 $a_j<\max(a_1,\ldots,a_{j-1})$ 的下标个数\
实现时维护前缀最大值 $mx$，从左到右扫描，若 $mx>a_j$ 则计入答案，否则更新 $mx=a_j$
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
        vector<int> a(n);
        for (int i = 0; i < n; i ++) cin >> a[i];

        int ans = 0;
        int mx = a[0];
        for (int i = 1; i < n; i ++) {
            if (mx > a[i]) ans++;
            else mx = a[i];
        }

        cout << ans << endl;
    }
}
```