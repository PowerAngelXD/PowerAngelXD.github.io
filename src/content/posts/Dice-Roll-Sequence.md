---
title: Dice-Roll-Sequence
published: 2026-03-28
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法，每日一题]
category: '算法'
draft: false 
lang: ''
---

## 题面
Consider the following cube $D$ where numbers $x$ and $7-x$ **lie on opposite sides**:

<div align="center">
    <img src="https://espresso.codeforces.com/e047423e2d0117d489bd89afd6732f829aa2c224.png" >
</div>

A sequence $b$ of integers from $1$ to $6$ is called a dice roll sequence if it satisfies the following condition:

-   All pairs of adjacent elements lie on adjacent $^{\text{∗}}$ sides of the cube.

For example, $[1,4,2]$ is a dice roll sequence, while $[3,4,6,3]$ is not because $3$ and $4$ are not on adjacent sides of the dice. Additionally, $[2,2,4]$ is not a dice roll sequence because $2$ and $2$ are on the same (not adjacent) side of the dice.

Given a sequence $a$ of $n$ integers from $1$ to $6$, you can perform the following operation any number of times (possibly zero).

-   Select an index $1 \le i \le n$ and an integer $1 \le x \le 6$. Then, change the value of $a_i$ to $x$.

Please determine the **minimum number of operations** required to make $a$ a dice roll sequence.

$^{\text{∗}}$Two sides of the cube $S$ and $T$ are called adjacent if they share **exactly** one edge of the cube. **Do note that** this condition implies $S \neq T$ as well.

**Input**

Each test contains multiple test cases. The first line contains the number of test cases $t$ ($1 \le t \le 10^4$). The description of the test cases follows.

The first line of each test case contains a single integer $n$ ($1 \le n \le 3 \cdot 10^5$).

The second line of each test case contains $n$ integers $a_1,a_2,\ldots,a_n$ ($1 \le a_i \le 6$).

It is guaranteed that the sum of $n$ over all test cases does not exceed $3 \cdot 10^5$.

**Output**

For each test case, output the minimum number of operations required to make $a$ a dice roll sequence.

## 思路
由于要保证相邻的两个元素不能在骰子上是“相对”的，所以我们只需要遍历一遍这个序列，然后找相邻的两个元素是否相对，利用
$$
a[i] + a[i - 1] = 7
$$
来进行判断
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
        
        int count = 0;
        for (int i = 1; i < n; i ++) {
            if (a[i] + a[i - 1] == 7 || a[i] == a[i - 1]) {
                count ++;
                a[i] = 0;
            }
        }
        cout << count << endl;
    }
}
```