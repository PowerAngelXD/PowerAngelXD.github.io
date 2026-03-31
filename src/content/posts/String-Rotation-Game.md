---
title: String-Rotation-Game
published: 2026-03-31
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法]
category: '算法'
draft: false 
lang: ''
---
## 题面
Define a **block** in a string as a contiguous substring of characters of the same type that cannot be extended either to the left or the right. For example, in the string aabcccdaa, there are five blocks:

-   aa ($1$\-st to $2$\-nd characters)
-   b ($3$\-rd character)
-   ccc ($4$\-th to $6$\-th characters)
-   d ($7$\-th character)
-   aa ($8$\-th to $9$\-th characters).

You are playing a game where you are given a string $s$ of length $n$. You can cyclically rotate $^{\text{∗}}$ the string however you want. Your score is then calculated as the number of blocks in the final string. Please find the maximum score possible.

$^{\text{∗}}$Formally, choose an index $1 \leq i \leq n$, and replace the string $s_1s_2\ldots s_n$ with the string $s_{i+1}s_{i+2}\ldots s_ns_1s_2\ldots s_{i}$. For example, the string abcde can be rotated to string deabc by choosing $i=3$.

**Input**

Each test contains multiple test cases. The first line contains the number of test cases $t$ ($1 \le t \le 500$). The description of the test cases follows.

The first line of each test case contains a single integer $n$ ($1 \le n \le 100$).

The second line of each test case contains the string $s$ of length $n$.

Strings $s$ consist of lowercase Latin characters only.

**Output**

For each testcase, output a single integer denoting the maximum score you can achieve.
## 思路
这题所描述的本质上是一个环，且题目要求旋转之后的块数要尽可能最大化，因此我们可以把问题转化成：“环上相邻字符是否不同”

我们可以设一个变量 $D$ 来表示环上的相邻字符不同的个数（换言之，不同边的个数）

而对于题目中的循环操作，本质是将这个环切开，得到一个线性的链

之后，就可以计算出块数：$N = D + 1$
> [!WARNING] 为什么要+1 ?
> 最终的答案是“块的最大值”，而如果不+1，最终计算的只是基于第一个块之后的新块的数量
> 因此，我们加上这个1，才能体现总的块数量
> 
> **e.g.** 对于 aaaaa，如果执行旋转之后，不+1，最终会得到块的数量为0，这很明显是错误的

当然，也要注意特殊情况：当所有相邻字符都不同，此时，有 $N = D$
## 代码
```c++
#include <bits/stdc++.h>

using namespace std;

int main() {
    int t;
    cin >> t;
    while (t--) {
        int n;
        string s;
        cin >> n >> s;

        int D = 0;
        for (int i = 0; i < n; i ++) {
            if (s[i] != s[(i + 1) % n]) {
                D ++;
            }
        }

        int ans = (D == n ? n : D + 1);
        cout << ans << endl;
    }
    return 0;
}
```

