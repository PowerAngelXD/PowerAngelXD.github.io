---
title: Digit-String.md
published: 2026-06-02
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法, 每日一题]
category: '算法'
draft: false 
lang: ''
---
# Digit String
## 题面
You are given a string $s$ consisting of digits from $1$ to $4$.

Let's say that the string is beautiful if it is **impossible** to select some of its elements and write them out (in the same order as they appear in the string) to form a number that is a multiple of $4$. For example, the strings 31, 222, 213 are beautiful, while the strings 143, 3123, 1322 are not. The empty string is considered beautiful.

Your task is to calculate the minimum possible number of elements in the string $s$ that need to be removed in order to make it beautiful.

**Input**

The first line contains a single integer $t$ ($1 \le t \le 10^4$) — the number of test cases.

The only line of each test case contains a string $s$ ($1 \le |s| \le 3 \cdot 10^5$), consisting of digits from $1$ to $4$.

Additional constraint on the input: the sum of the lengths of $s$ over all test cases doesn't exceed $3 \cdot 10^5$.

**Output**

For each test case, print a single integer — the minimum possible number of elements in the string $s$ that need to be removed in order to make it beautiful.

## 思路
一个数字是 $4$ 的倍数，当且仅当其末两位是 $4$ 的倍数，或者其本身是个位数字且为 $4$ \
\
核心思路是删除所有使子序列满足 $4$ 的倍数条件的字符 \
由于数字范围为 $1$ 至 $4$，若子序列包含 $4$，则该子序列本身就是 $4$ 的倍数，因此所有的 $4$ 必须被删除 \
\
在剩余的数字 $1, 2, 3$ 中，可能构成 $4$ 的倍数的两位数组合仅有 $12$ 和 $32$ \
\
为了让字符串变“美丽”，必须保证任意的 $1$ 或 $3$ 之后不能出现 $2$ \
这意味着最终保留的子序列必须呈现前段全为 $2$、后段全为 $1$ 或 $3$ 的形式 \
\
代码通过枚举分割点 $i$，利用前缀和与后缀和，计算以 $i$ 为界时前半部分中 $2$ 的个数与后半部分中 $1, 3$ 个数之和 \
取所有分割点情况下的最大保留长度 $best$，则最小删除次数（答案）即为 $n - best$

## 代码
```c++
#include <bits/stdc++.h>

using namespace std;

int main() {
    int t;
    cin >> t;
    while (t --) {
        string s;
        cin >> s;

        int n = (int)s.size();
        vector<int> suf13(n + 1, 0);
        for (int i = n - 1; i >= 0; i --) {
            suf13[i] = suf13[i + 1] + (s[i] == '1' || s[i] == '3');
        }

        int prefix2 = 0;
        int best = 0;
        for (int i = 0; i <= n; i ++) {
            best = max(best, prefix2 + suf13[i]);
            if (i == n) break;
            if (s[i] == '2') prefix2 ++;
        }

        cout << (n - best) << endl;
    }
}

```