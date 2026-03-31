---
title: Game
published: 2026-03-23
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法]
category: '算法'
draft: false 
lang: ''
---

# Game
## 题面
Alice and Bob are playing a card game. The game consists of $3$ rounds, in each round both players score some points (from $0$ to $k$), and for each round, Alice's score differs from Bob's score. The player who scores more points in a round is considered the winner of that round.

In the first round, Alice scored $a_1$ points, and Bob scored $b_1$. In the second round, Alice scored $a_2$ points, and Bob scored $b_2$.

The winner of the game is the one who has the higher total score. If Alice's total score equals Bob's total score, the player who won more rounds is declared the winner. Alice wants to understand if Bob has a chance to win, or if she will definitely win regardless of the results of the $3$\-rd round. Help her determine this!

Please note that in each round, a player can score at least $0$ and at most $k$ points. Additionally, for each round, Alice's score differs from Bob's score.

**Input**

The first line contains a single integer $t$ ($1 \le t \le 10^4$) — the number of test cases.

Each test case consists of three lines:

-   the first line contains a single integer $k$ ($1 \le k \le 50$) — the maximum number of points that can be scored in a round;
-   the second line contains two integers $a_1$ and $b_1$ ($0 \le a_1, b_1 \le k$; $a_1 \ne b_1$) — the points scored by Alice and Bob respectively in the first round;
-   the third line contains two integers $a_2$ and $b_2$ ($0 \le a_2, b_2 \le k$; $a_2 \ne b_2$) — the points scored by Alice and Bob respectively in the second round.

**Output**

For each test case, output NO if Alice will win regardless of the results of the third round, or YES if Bob has a chance to win.

## 思路
因为题目说 “获胜者为总分较高的一方”，所以我们比较第三轮的极端情况：“爱丽丝不得分而鲍勃得到最大分”，在这个情况下，如果鲍勃的总分都没有爱丽丝的总分高，那么鲍勃就不可能赢，反之，鲍勃有机会赢得比赛

## 代码
```c++
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int T;
    cin >> T;
    while (T --) {
        int k;
        cin >> k;
        int a1, b1, a2, b2;
        cin >> a1 >> b1 >> a2 >> b2;
        if (a1 + a2 < b1 + b2 + k) {
            cout << "YES" << endl;
        }
        else {
            cout << "NO" << endl;
        }
    }
}

```