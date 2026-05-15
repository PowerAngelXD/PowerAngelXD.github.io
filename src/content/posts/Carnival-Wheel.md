---
title: Carnival-Wheel
published: 2026-05-15
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法, 每日一题]
category: '算法'
draft: false 
lang: ''
---
## 题面
You have a prize wheel divided into $l$ sections, numbered from $0$ to $l-1$. The sections are arranged in a circle, so after section $l-1$, the numbering continues again from section $0$.

Initially, the prize pointer is at section $a$. Each time you spin the wheel, the pointer moves exactly $b$ sections forward. That is, after one spin, the pointer moves from section $a$ to section $(a+b)\bmod l$, after two spins to $(a+2b)\bmod l$, and so on $^{\text{∗}}$.

You may spin the wheel any number of times (including zero). After you stop, the section where the pointer finally lands determines your prize: you receive an amount equal to the number of that section.

What is the maximum prize you can obtain?

$^{\text{∗}}$Here, $x \bmod y$ denotes the remainder from dividing $x$ by $y$.

**Input**

Each test contains multiple test cases. The first line contains the number of test cases $t$ ($1 \le t \le 500$). The description of the test cases follows.

The first line of each test case contains three integers $l, a$, and $b$ ($1 \le l, b \le 5000$, $0 \le a \le l-1$).

**Output**

For each test case, output the maximum prize that can be obtained.
## 思路
设旋转 $k$ 次落点为 $x_k=(a+k\cdot b)\bmod l$ \
\
令 $g=\gcd(l,b)$ 则 $x_k\equiv a\pmod g$ 因为 $l\equiv 0\pmod g$ 且 $k\cdot b\equiv 0\pmod g$ \
\
所以所有可达落点都满足 $x\equiv a\pmod g$ \
\
令 $l'=l/g,\ b'=b/g$ 则 $\gcd(l',b')=1$ 因而 $k\cdot b'\bmod l'$ 可遍历 $0\sim l'-1$ \
\
令 $r=a\bmod g$ 则可达集合为 $\{r,\ r+g,\ r+2g,\ \dots,\ r+(l'-1)g\}$ \
最大值为 $r+(l'-1)g=r+l-g$ \
\
每组直接输出 $\mathrm{ans}=l-g+(a\bmod g)$ 时间复杂度 $O(1)$ 
## 代码
```c++
#include <bits/stdc++.h>

using namespace std;

int main() {
    int t;
    cin >> t;
    while (t --) {
        int l, a, b;
        cin >> l >> a >> b;

        int g = gcd(l, b);
        int r = a % g;
        int ans = l - g + r;

        cout << ans << endl;
    }
}
```