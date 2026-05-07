---
title: New-Year-Cake
published: 2026-05-07
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法, 每日一题]
category: '算法'
draft: false 
lang: ''
---
# New Year Cake
## 题面
Monocarp is going to bake a New Year cake.

The cake must consist of **at least one** layer. The size of the top layer of the cake must be $1$; the size of the layer below it must be $2$; the layer below that must be $4$, and so on (each layer, except for the top one, is twice the size of the layer above it).

Additionally, each layer must be covered with either white or dark chocolate. To cover a layer of size $k$, Monocarp will need $k$ kilograms of chocolate. Each layer must be covered with exactly one type of chocolate, and **these types must alternate** (if some layer is covered with dark chocolate, both the layer directly below it and the layer directly above it must be covered with white chocolate, and vice versa).

Monocarp has $a$ kilograms of white chocolate and $b$ kilograms of dark chocolate. He wants to calculate the maximum number of layers that the cake can consist of, ensuring that he has enough chocolate of both types.

**Input**

The first line contains one integer $t$ ($1 \le t \le 10^4$) — the number of test cases.

Each test case consists of one line containing two integers $a$ and $b$ ($1 \le a, b \le 10^6$).

**Output**

For each test case, output one integer — the maximum possible number of layers in the cake.
## 思路
读题：\
层大小被规则完全固定为 $1,2,4,8,\dots$，想要更多层只能从上往下依次做，不能跳层也不能改大小\
同时由题意：相邻层颜色必须交替，因此整个蛋糕只有两种可能的配色序列：顶层白或顶层黑\
固定顶层颜色后，第 $i$ 层需要的颜色与用量都确定为 $2^{i-1}$，此时只需从上往下模拟扣除巧克力\
模拟过程维护当前层需求 $need$，初始为 $1$，每成功做一层后令 $need \leftarrow 2\cdot need$\
当轮到某种颜色但剩余量小于 $need$ 时立刻停止，因为这一层既不能换颜色也不能换大小，继续下去不可能\
那么我们只需要分别模拟顶层白与顶层黑两种情况，答案取两者最大值即可

## 代码
```c++
#include <bits/stdc++.h>

using namespace std;

int s(long long white, long long dark, bool sw) {
    long long need = 1;
    int lay = 0;
    while (true) {
        bool use_white = (lay % 2 == 0) ? sw : !sw;
        if (use_white) {
            if (white < need) break;
            white -= need;
        } else {
            if (dark < need) break;
            dark -= need;
        }
        lay ++;
        need <<= 1;
    }
    return lay;
}

int main() {
    int t;
    cin >> t;
    while (t --) {
        long long a, b;
        cin >> a >> b;

        int ans = max(s(a, b, true), s(a, b, false));
        cout << ans << '\n';
    }
}

```
