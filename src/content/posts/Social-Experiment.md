---
title: Social-Experiment
published: 2026-04-13
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法, 每日一题]
category: '算法'
draft: false 
lang: ''
---
## 题面
Right now, the largest social experiment in the history of Codeforces is taking place, involving $n$ people. They are required to form teams of $2-3$ people, after which each team chooses one of two civilizations to participate in the social experiment.

The organizers of this social experiment want to know what the possible difference in the number of people in the civilizations could be. Find the minimum possible difference.

**Input**

Each test consists of several test cases. The first line contains a single integer $t$ $(1 \le t \le 10^4)$ — the number of test cases. The following lines describe the test cases.

In the only line of each test case, there is an integer $n$ — the number of people participating in the social experiment $(2 \le n \le 10^{4})$.

**Output**

For each number $n$, output the minimum possible difference between the number of people in the civilizations.
## 思路
题目本质要求的就是两种文明选择人数的最小差值

而成组方面，要么是2人成组，要么是3人成组

所以注意到，有两种特判案例：
- $n=2$ 答案就是2
- $n=3$ 答案就是3

当 $n \geq 4$ 时：

如果 $n$ 为偶数，那么两种文明平分队伍，都是2人队，答案为0

如果 $n$ 为奇数，那么两种文明的人数最小差必定是1

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
        if (n == 2 || n == 3) cout << n << endl;
        else {
            cout << n % 2 << endl;
        }
    }
}
```