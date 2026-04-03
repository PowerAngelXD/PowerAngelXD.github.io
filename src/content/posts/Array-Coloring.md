---
title: Array-Coloring
published: 2026-04-03
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法, 每日一题]
category: '算法'
draft: false 
lang: ''
---
# $\color{red}{\text{RED}}$ AND $\color{blue}{\text{BLUE}}$
## 题面
You have $n$ cards arranged in a row. The $i$\-th card has the integer $a_i$ written on it. All integers are distinct.

You must color each card either $\color{red}{\text{red}}$ or $\color{blue}{\text{blue}}$ such that the following conditions are satisfied:

-   Any two adjacent cards in the row have different colors.
-   If you rearrange the cards so that the numbers on them are in increasing order, any two adjacent cards in the new row must also have different colors.

Determine if such a coloring exists.

**Input**

Each test contains multiple test cases. The first line contains the number of test cases $t$ ($1 \le t \le 200$). The description of the test cases follows.

The first line of each test case contains a single integer $n$ ($2 \le n \le 100$) — the length of the array.

The second line of each test case contains $n$ integers $a_1, a_2, \ldots, a_n$ ($1 \le a_i \le n$).

It is guaranteed that all elements of $a$ are distinct.

**Output**

For each test case, output "YES" if you can color the cards so that the conditions are satisfied, and "NO" otherwise.

You can output the answer in any case (upper or lower). For example, the strings "yEs", "yes", "Yes", and "YES" will be recognized as positive responses.
## 思路
读题，发现题目对于这个数组有两个约束条件：

- 原数组的相邻下标是不同色的
- 排序之后的数组相邻下标依然是不同色的

第一个条件会确定整个数组的着色方式（红蓝红蓝....或者是蓝红蓝红....）

因此我们可以借助一个 `map<int, int> pos` ，并记 $pos[i]$ 为数值 $i$ 在原数组中的下标

那么之后，我们只需要验证第二个约束条件是否满足

由于这个颜色是相邻不能相同，存在奇偶性，所以我们只需要验证：$pos[i] \space mod 2$ 与 $pos[i + 1] \space mod 2$ 是否相等即可

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
        vector<int> a(n + 1);
        for (int i = 1; i <= n; i ++) cin >> a[i];
        map<int, int> pos;
        for (int i = 1; i <= n; i ++) {
            pos[a[i]] = i;
        }

        bool flag = false;
        for (int i = 1; i <= n - 1; i ++) {
            if (pos[i] % 2 == pos[i + 1] % 2) {
                cout << "NO" << endl;
                flag = true;
                break;
            }
        }
        if (!flag)
            cout << "YES" << endl;
    }
}
```