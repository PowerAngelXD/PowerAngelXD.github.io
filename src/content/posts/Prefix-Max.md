---
title: Prefix-Max
published: 2026-04-11
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法, 每日一题]
category: '算法'
draft: false 
lang: ''
---
## 题面
You are given an array of $n$ integers $a_1, a_2, \ldots, a_n$.

The value of an array is the sum of the maximums of each prefix of the array. More formally, the value of an array $a$ is $\sum_{i=1} ^{n} \operatorname{max}(a_1, \ldots, a_i)$. For example, the value of the array \[$1, 2, 1$\] is $\operatorname{max}(1) + \operatorname{max}(1, 2) + \operatorname{max}(1, 2, 1) = 1 + 2 + 2 = 5$.

You can choose two indices $i$ and $j$ and swap elements $a_i$ and $a_j$; this operation can be applied **at most** one time.

Find the maximum possible value of the array $a$ after at most one operation.

**Input**

The first line of the input contains a single integer $t$ ($1 \leq t \leq 100$) — the number of test cases.

The first line of each test case contains a single integer $n$ ($2 \le n \le 50$) — the length of the array $a$.

The second line contains $n$ integers $a_1, a_2, \ldots, a_n$ ($1 \le a_i \le 10^4$) — the array $a$.

**Output**

For each test case, output the maximum possible value of the array $a$ after the swap has been performed.
## 思路
理清题目中的数组价值的含义为：
$$
V(a) = \sum^n_{i=1} max(a_1, ..., a_i);
$$
因此我们肯定需要一个函数来计算这个价值，我们将其命名为 `get_v`

由于题目要求的是交换任意一对索引之后的数组价值最大值，且交换操作只能执行一次，我们可以考虑暴力枚举进行

> [!NOTE]
> **为什么可以考虑暴力枚举？**
> 注意到 $2 \leq n \leq 50$，使得总方案数很小，所以时间复杂度可控

## 代码
```c++
#include <bits/stdc++.h>

using namespace std;

int get_v(std::vector<int> a) {
    int result = 0;
    for (int i = 0; i < a.size(); i ++) {
        int mx = a[i];
        for (int j = 0; j <= i; j ++) {
            if (a[j] > mx) mx = a[j];
        }
        result += mx;
    }
    return result;
}

int main() {
    int t;
    cin >> t;
    while (t --) {
        int n;
        cin >> n;
        vector<int> a(n);
        for (int i = 0; i < n; i ++) cin >> a[i];

        int ans = get_v(a);

        for (int i = 0; i < n; i ++) {
            for (int j = i + 1; j < n; j ++) {
                int tmp = a[i], ri = 0, rj = 0;
                a[i] = a[j];
                a[j] = tmp;
                ans = max(ans, get_v(a));
                a[i] = ri; a[j] = rj;
            }
        }
        cout << ans << endl;
    }
}
```