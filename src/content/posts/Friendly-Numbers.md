---
title: Friendly-Numbers
published: 2026-03-24
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法, 每日一题]
category: '算法'
draft: false 
lang: ''
---

## 题面
For an integer $x$, we call another integer $y$ friendly if the following condition holds:

-   $y - d(y) = x$, where $d(y)$ is the sum of the digits of $y$.

For a given integer $x$, determine how many friendly numbers it has.

**Input**

Each test contains multiple test cases. The first line contains the number of test cases $t$ ($1 \le t \le 500$). The description of the test cases follows.

Each test case consists of a single line containing one integer $x$ ($1 \le x \le 10^{9}$).

**Output**

For each test case, output one integer — the answer to the problem.

## 思路
对题目所给的式子变形可以得到：$y = x + d(y)$

也就是求符合条件的 $y$ 的个数

$x$ 是输入，已经给出，我们只需要确定 $d(y)$ 即可

进行如下推理：

若 $y$ 有 $k$ 位，则 $d(y) \leq 9k$ 
> [!NOTE]
> 因为 $d(y)$ 表示 $y$ 各位上的和，而每个数字位最大值为9

而注意到，当 $k \geq 11$ 时，$y \geq 10^11$

此时有：$x = y - d(y) \geq 10^10 - 99 \geq 10^9$

明显不符合题目条件：$1 \le x \le 10^{9}$

因此，$y$ 最多只有10位

根据上面的推论，不难得到：$d(y) \leq 9 \times 10 = 90$

因此可以得到 $y$ 的上界：$y = x + d(y) \leq x + 90$

因此，我们只需要暴力枚举 $[x, x + 90]$ 内范围的数，看他们是否满足条件即可，满足给对应计数器加一，否则不进行操作

## 代码
```c++
#include <bits/stdc++.h>

using namespace std;

int calc(int num) {
    string str = to_string(num);
    int result = 0;
    for (auto ch: str) {
        result += ch - '0';
    }
    return result;
}

int main() {
    int t;
    cin >> t;

    while (t --) {
        int x;
        cin >> x;
        if (x == 1)
            cout << 0 << endl;
        else {
            int count = 0;
            for (int y = x + 1; y < x + 91; y ++) {
                int d = calc(y);
                if (y - d == x) {
                    count ++;
                }
            }
            cout << count << endl;
        }
    }
}
```