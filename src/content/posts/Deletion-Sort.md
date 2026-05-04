---
title: Deletion-Sort
published: 2026-05-04
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法, 每日一题]
category: '算法'
draft: false 
lang: ''
---
## 题面
AksLolCoding is playing a game on an array $a$ of $n$ positive integers. During each turn:

-   If $a$ is **non-decreasing**$^{\text{∗}}$, the game ends.
-   Otherwise, AksLolCoding can choose **any** single element and remove it from the array.

Determine the **minimum** possible number of elements that can be remaining in the array after the game ends.

$^{\text{∗}}$$a$ is non-decreasing if $a_i\leq a_{i+1}$ for all $1\leq i\leq m-1$, where $m$ is the length of $a$.

**Input**

The first line contains an integer $t$ ($1 \leq t \leq 1000$), the number of test cases.

The first line of each test case contains an integer $n$ ($1 \leq n \leq 10$).

The second line of each test case contains $n$ integers, the elements of $a$ ($1 \leq a_i \leq 100$).

**Output**

For each test case, output an integer: the minimum possible number of elements left once the array is sorted.
## 思路
这题的关键是注意游戏会在数组变成非递减时立刻结束，所以我们的目标不是尽快变有序，而是尽量拖到最后再有序，从而多删元素

- 如果一开始就满足 $a_1 \le a_2 \le \cdots \le a_n$，那么游戏直接结束，无法删除任何元素，所以答案是 $n$
- 否则数组一定存在相邻逆序对，即存在某个 $i$ 使得 $a_i > a_{i+1}$，记 $x=a_i$，$y=a_{i+1}$，此时 $x>y$

接下来我们可以一直删除除 $x,y$ 之外的所有元素，因为删掉其它位置不会改变 $x$ 与 $y$ 的相对顺序，而且它们始终相邻并满足 $x>y$，因此数组始终不是非递减，游戏不会提前结束

此时：

- 当数组只剩下 $[x,y]$ 时仍然不是非递减，还可以再删一次，删掉其中一个后数组长度变成 $1$
- 长度为 $1$ 的数组必然是非递减，所以游戏结束，此时剩余元素最少为 $1$

综上答案只有两种情况，要么是 $n$，要么是 $1$，实现时只需检查原数组是否非递减即可
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

        bool flag = false;
        int prev = a[0];
        for (int i = 1; i < n; i ++) {
            if (prev > a[i]) {
                flag = true;
                break;
            }
            prev = a[i];
        }

        if (flag) {
            cout << 1 << endl;
        }
        else cout << n << endl;
    }
}
```