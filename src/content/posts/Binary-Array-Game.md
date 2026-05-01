---
title: Binary-Array-Game
published: 2026-05-01
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法, 每日一题]
category: '算法'
draft: false 
lang: ''
---
## 题面
Alice and Bob are playing a game on an array $a$ of size $n$, containing only numbers 0 and 1. Alice moves first, with each player alternating turns.

In a player's turn, he or she chooses two integers $l$ and $r$ such that $1 \leq l \color{red}{ \lt } r \leq |a|$ (here, $|a|$ denotes the current length of $a$). Then, the subarray $[a_l,a_{l+1},\ldots,a_r]$ is replaced with a single number $1-\operatorname{min}(a_l,a_{l+1},\ldots,a_r)$. That is, if all numbers in the subarray are $1$, then the subarray $[a_l,a_{l+1},\ldots,a_r]$ is removed, and the number $0$ is inserted where the subarray was. Otherwise, the subarray $[a_l,a_{l+1},\ldots,a_r]$ is removed, and the number $1$ is inserted where the subarray was.

The game ends when there is exactly one number left in the array (where the player to go cannot make legal moves). Alice wins if the final number is $0$. Otherwise, Bob wins. Please determine who wins this game with optimal play.

**Input**

Each test contains multiple test cases. The first line contains the number of test cases $t$ ($1 \le t \le 100$). The description of the test cases follows.

The first line of each test case contains a positive integer $n$ ($3 \le n \le 100$), denoting the length of the array $a$.

The second line of each test case contains $n$ integers $a_1,a_2,\ldots,a_n$ ($0 \le a_i \le 1$).

**Output**

For each test case, output Alice if Alice wins, and Bob otherwise. You may output each character in any case. For example, the answers Alice, alice, ALICE, AliCe will all be interpreted the same.
## 思路
这题最关键的就是把操作“翻译”成一句话：

- 选择的区间 **全为 1** ⇒ 替换成 `0`
- 选择的区间 **包含 0** ⇒ 替换成 `1`

接下来观察 Bob 的必胜手段：

- 只要轮到 Bob 操作时，当前数组长度至少为 2 且数组里 **存在某个 0**
- Bob 直接选择整个区间 `[1..n]`
  - 因为区间里包含 0，所以会被替换成 `1`
  - 数组长度变为 1，游戏立刻结束
  - 最终数为 1 ⇒ Bob 当场获胜

因此对 Alice 来说，只要她第一步操作后数组里还残留任何一个 `0`，就等价于“把必胜局面送给 Bob”  

也就是说：**Alice 想赢，她的第一步必须把所有的 0 一次性消掉，让数组变成全 1**

那么问题就变成：什么时候 Alice 能一步把数组变成全 1？

- 如果 $a_1 = 0$ 且 $a_n = 0$：
  - Alice 想把所有 0 一次覆盖，选的区间必须同时包含第 1 位和第 n 位
  - 由于区间必须连续，这样的区间只能是整个 $[1..n]$`
  - 但选整个数组时区间包含 0，会被替换成 $1$，数组长度直接变成 1，最终为 1 ⇒ Alice 立刻输
  - 所以这种情况下 Alice 无法避免让 Bob 下一步“全选秒杀”，结论是 **Bob 必胜**

- 否则（$a_1 = 1$ 或 $a_n = 1$）：
  - 若数组本身全为 1：Alice 直接选整个 $[1..n]$，替换成 $0$，当场结束 ⇒ **Alice 必胜**
  - 若数组里存在 0：
    - 如果 $a_1 = 1$，Alice 选区间 $[2..n]$，其中必定包含某个 0，因此会被替换成 $`，数组变成 $[1,1]$
    - 如果 $a_n = 1$，Alice 选区间 $[1..n-1]$，同理也能变成 $[1,1]$
    - 得到 $[1,1]$ 后轮到 Bob，他只能选整个区间（全 1），替换成 $0$，最终剩下 $0$ ⇒ **Alice 获胜**

综上，胜负只由两端决定：

- $a_1 = 0$ 且 $a_n = 0$ ⇒ 输出 `Bob`
- 否则 ⇒ 输出 `Alice`

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

        if (a[1] == 0 && a[n] == 0) {
            cout << "Bob" << endl;
        }
        else cout << "Alice" << endl;
    }
}
```