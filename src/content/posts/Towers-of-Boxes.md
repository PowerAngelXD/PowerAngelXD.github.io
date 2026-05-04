---
title: Towers-of-Boxes
published: 2026-05-04
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法, 每日一题]
category: '算法'
draft: false 
lang: ''
---
## 题面
Monocarp has $n$ identical boxes. The weight of each box is $m$, and the durability of each box is $d$.

To save space, Monocarp wants to build several "towers" of boxes by stacking them on top of each other. Each tower will consist of a positive (greater than $0$) integer number of boxes stacked on top of each other. To ensure that no box breaks, the following condition must be met:

-   for each box, the **total** weight of all boxes above it must not exceed the durability of that box.

Help Monocarp calculate the minimum number of towers he can achieve, given that each of the $n$ boxes must be used.

**Input**

The first line contains a single integer $t$ ($1 \le t \le 10^4$) — the number of test cases.

Each test case consists of a single line containing three integers $n, m, d$ ($1 \le n, m, d \le 50$).

**Output**

For each test case, print one integer — the minimum number of towers.
## 思路
### 关键转化
所有盒子完全相同，每个盒子的重量为 $m$ 耐久为 $d$， 因此任意一座塔只和它的高度有关

设某一座塔从上到下共有 $h$ 个盒子

第 $k$ 个盒子上方有 $k-1$ 个盒子，因而它承受的上方总重量为 $(k-1)\cdot m$

因为 $k$ 越大，上方盒子越多，承受重量越大；所以在同一座塔中最危险的是最底下的盒子

最底下的盒子上方共有 $h-1$ 个盒子，承受重量为 $(h-1)\cdot m$

题目要求对每个盒子都满足：上方总重量不超过耐久 $d$
由于最底下盒子的承受重量最大，只要最底下不破，上面的盒子承受重量更小，一定也不破

因此一座塔可行当且仅当
$$
(h-1)\cdot m \le d
$$

对不等式进行变形
$$
h-1 \le \left\lfloor \frac{d}{m} \right\rfloor
$$
$$
h \le \left\lfloor \frac{d}{m} \right\rfloor + 1
$$

令单座塔允许的最大高度为
$$
H=\left\lfloor \frac{d}{m} \right\rfloor + 1
$$
则每座塔最多放 $H$ 个盒子

### 最小塔数
现在问题等价为 将 $n$ 个盒子分成若干组，每组大小在 $[1,H]$ 之间，使得组数最少

显然每座塔最多放 $H$ 个盒子，所以塔的数量至少为
$$
\left\lceil \frac{n}{H} \right\rceil
$$

同时这个下界一定能达到，所以采用贪心将盒子尽量装满
前若干座塔各放 $H$ 个盒子，最后一座塔放剩余的 $n\bmod H$ 个盒子，若余数为 $0$ 则不存在最后一座塔

因为最后一座塔的高度也不超过 $H$ 所以一定满足耐久条件

因此最小塔数就是：
$$
\left\lceil \frac{n}{H} \right\rceil
$$

为了只用整数计算，可以把上式写成
$$
\left\lceil \frac{n}{H} \right\rceil = \left\lfloor \frac{n+H-1}{H} \right\rfloor
$$
## 代码：
```c++
#include <bits/stdc++.h>

using namespace std;

int main() {
    int t;
    cin >> t;
    while (t --) {
        long long n, m, d;
        cin >> n >> m >> d;

        long long H = d / m + 1; 
        long long ans = (n + H - 1) / H;

        cout << ans << '\n';
    }
}

```
