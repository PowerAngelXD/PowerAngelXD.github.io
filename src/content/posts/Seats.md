---
title: Seats
published: 2026-05-02
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法, 每日一题]
category: '算法'
draft: false 
lang: ''
---
## 题面
Cordell manages a row of $n$ seats at the Scuola Comunale di Musica Piova where students are strictly forbidden from sitting next to each other.

You are given a binary string$^{\text{∗}}$ $s$, where $s_i = \mathtt{1}$ indicates that the $i$\-th seat has been occupied by a student, and $s_i = \mathtt{0}$ indicates that it is free now. It is guaranteed that no two adjacent seats are occupied currently. Cordell needs to add more students until it is impossible to seat anyone else in the row. However, she wants to achieve this state with as few students as possible.

Your task is to calculate the minimum **total** number of students seated when it is impossible to seat anyone else in the row.

$^{\text{∗}}$A binary string is a string where each character is either $\mathtt{0}$ or $\mathtt{1}$.

**Input**

Each test contains multiple test cases. The first line contains the number of test cases $t$ ($1 \le t \le 10^4$). The description of the test cases follows.

The first line of each test case contains a single integer $n$ ($1 \le n \le 2 \cdot 10^5$) — the number of seats in the row.

The second line of each test case contains the binary string $s$ of length $n$ ($s_i \in \{\mathtt{0}, \mathtt{1}\}$). It is guaranteed that no two adjacent characters are both $\mathtt{1}$.

It is guaranteed that the sum of $n$ over all test cases does not exceed $2 \cdot 10^5$.

**Output**

For each test case, output a single integer — the minimum total number of seated students.
## 思路
### 1 什么时候算无法再坐人
规则是不允许出现相邻的两个 $1$

如果某个位置是 $0$ 且它左右相邻位置都不是 $1$ ，那么把它变成 $1$ 仍然合法，说明还可以继续坐人

因此最终状态等价于：**每个 $0$ 的左右至少有一个位置是 $1$**

换成字符串的局部形态就是 中间不能出现 $\mathtt{000}$ 且两端不能出现 $\mathtt{00}$

### 2 把问题拆成若干段连续的 $0$
题目保证初始时没有相邻的 $1$ 所以所有的 $1$ 都是固定存在的学生

这些 $1$ 会把整行座位切成若干段连续的 $0$ 每段内部怎么加人不会影响别的段

所以答案可以写成
$$
\text{ans}=\#(\mathtt{1}\text{ 的个数})+\sum \text{need}(\text{每段 }0)
$$

### 3 一段连续 $0$ 最少要补多少个 $1$
考虑一段连续的 $0$ 长度为 $len$

在这段里放入一个新学生 会占用一个位置 并且让它左右相邻位置都变成不可再放的位置

也就是一次放置最多覆盖连续三个位置的需求 因此每需要覆盖 $3$ 个空位就要放一个学生

用哨兵统一讨论，令原串左右各加一个 $\mathtt{1}$ 方便找到每段 $0$ 的左右端点

真实的边界需要单独修正，设：
$$
c=[\text{这段贴左边界}]+[\text{这段贴右边界}]
$$
其中 $c\in\{0,1,2\}$ 且方括号表示条件成立取 $1$ 否则取 $0$

那么这一段的最少新增人数是：
$$
\text{need}=\left\lfloor \frac{len+c}{3}\right\rfloor
$$

这个式子对应三种常见情况

夹在两个 $1$ 之间时 $c=0$ 得到 $\left\lfloor \frac{len}{3}\right\rfloor$
贴一侧边界时 $c=1$ 得到 $\left\lfloor \frac{len+1}{3}\right\rfloor$
两侧都是边界也就是整串全为 $0$ 时 $c=2$ 得到 $\left\lfloor \frac{len+2}{3}\right\rfloor$

### 4 扫描实现要点
从左到右扫描原串 统计已有的 $1$ 直接加入答案

当遇到一个 $0$ 段的起点 记录段左端点 $L$

当遇到一个 $0$ 段的终点 计算 $len$ 与 $c$ 然后把 $\left\lfloor \frac{len+c}{3}\right\rfloor$ 加到答案
判断是否贴边界可以用 $L=1$ 表示贴左边界 用 $R=n$ 表示贴右边界

## 代码
```c++
#include <bits/stdc++.h>

using namespace std;

int main() {
    int t;
    cin >> t;
    while (t --) {
        int n;
        string s;
        cin >> n >> s;

        s = '1' + s + '1';
        int ans = 0;
        int L = 0;
        for (int i = 1; i <= n; i ++) {
            if (s[i] == '0') {
                if (s[i - 1] == '1') L = i;
                if (s[i + 1] == '1') {
                    int len = i - L + 1;
                    int c = (L == 1) + (i == n);
                    ans += (len + c) / 3;
                }
            } else {
                ans ++;
            }
        }

        cout << ans << '\n';
    }
    return 0;
}*n*
```