---
title: Eating-Game
published: 2026-05-08
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法, 每日一题]
category: '算法'
draft: false 
lang: ''
---
There are $n$ players playing a game at a circular table. The $i$\-th player has $a_i$ dishes to eat. They take turns eating the food, and any player can go first.

During their turn, if player $i$ has any dishes remaining, they must eat exactly one dish. Then, player $(i \bmod n) + 1$ starts their turn. This continues until all dishes are finished.

The player who eats the last dish is considered the winner. Determine the number of players that can possibly be winners.

**Input**

The first line contains an integer $t$ ($1 \leq t \leq 5000$), the number of test cases.

The first line of each test case contains an integer $n$ ($1 \leq n \leq 10$).

The second line of each test case contains $n$ integers, the elements of $a$ ($1 \leq a_i \leq 10$).

**Output**

For each test case, output a line with the answer.
## 思路
按题意，很明显这个“最有可能成为赢家”的玩家就是有最多菜的玩家，即：$a_i = a_{max}$

因此我们只需要统计出来有多少最大值的玩家就行了
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

        int M = *max_element(a.begin(), a.end());
        int c = 0;
        for (auto i: a) {
            if (i == M) c ++;
        }

        cout << c << endl;
    }
}
```