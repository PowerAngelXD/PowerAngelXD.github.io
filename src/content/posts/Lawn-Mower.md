---
title: Lawn-Mower
published: 2026-03-29
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法，每日一题]
category: '算法'
draft: false 
lang: ''
---

## 题面
The exit from the summer cottage is fenced with a fence consisting of $n$ boards, each $1$ meter wide. To the left and right of the exit are the fences of other plots. Some of the boards of the fence want to be removed for the construction of a bathhouse (possibly all or none), while there is an automatic lawn mower on the plot that is $w$ meters wide and must not leave the plot through a hole in the fence.

The lawn mower will be able to exit the plot if there are at least $w$ consecutive removed boards among the numbers of the removed boards. Determine the maximum number of boards that can be removed from the fence.

**Input**

Each test contains multiple test cases. The first line contains the number of test cases $t$ ($1 \le t \le 10^4$). The description of the test cases follows.

The only line of each test case contains two integers $n$ and $w$ ($1 \leq n \leq 10^{9}$, $1 \leq w \leq 10^{9}$) — the number of boards in the fence and the width of the lawnmower in meters, respectively.

**Output**

For each test case, output a single number — the maximum number of boards that can be removed from the fence.
## 思路
由题目可知，对于 $w$ 宽度的割草机，如果也有连续的 $w$ 空白，则割草机会钻过去

想要实现拆掉数量最大，可以考虑极端情况：每一个长度为 $w$ 的区间内都有一个木板

则保留的木板数量为：$\lfloor \frac{n}{w} \rfloor$，这样，我们就得到了最大可拆模板数量： 
$$
n - \lfloor \frac{n}{w} \rfloor
$$
## 代码
```c++
#include <bits/stdc++.h>

using namespace std;

int main() {
    int t;
    cin >> t;

    while (t --) {
        long long n, w;
        cin >> n >> w;
        cout << n - n / w << '\n';
    }

    return 0;
}
```