---
title: Beautiful-Numbers
published: 2026-05-05
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法, 每日一题]
category: '算法'
draft: false 
lang: ''
---
## 题面
Let's define $F(x)$ as the sum of the digits of $x$. An integer $x$ is considered beautiful if $F(F(x)) = F(x)$.

You are given an integer $x$. In one move, you can choose any digit in the number and replace it with another. The resulting number cannot have leading zeros.

Your task is to calculate the minimum number of moves (possibly zero) required to make the given number beautiful.

**Input**

The first line contains a single integer $t$ ($1 \le t \le 10^4$) — the number of test cases.

The only line of each test case contains a single integer $x$ ($1 \le x \le 10^{18}$).

**Output**

For each test case, print a single integer — the minimum number of moves (possibly zero) required to make the given number beautiful.
## 思路
**先分析题意：**

设 $y$ 的十进制位数为 $k$，

若 $k\ge 2$，则 $y \ge 10^{k-1}$，数位和最多是 $9k$，

因此有 $F(y) \le 9k < 10^{k-1} \le y$，从而当 $y\ge 10$ 时一定有 $F(y) < y$

这说明方程 $F(y)=y$ 只能在 $y$ 为一位数时成立，也就是 $y \in [0,9]$

因此 $F(F(x)) = F(x)$ 等价于 $F(x)$ 本身是一位数，即 $F(x) \le 9$
于是题目变成把 x 的数位和变到不超过 9，并且操作次数最少

那么，

设原数位和 $S = F(x)$
若 $S \le 9$，$x$ 已经漂亮，答案为 0
若 $S > 9$，至少需要把数位和减少 $need = S - 9$

### 每次操作的最大收益
一次操作只改一位数字，为了让步数尽量少，我们只考虑把某位改小，因为改大只会让数位和更大，需要额外操作再降回来

把某一位从原数字 $d$ 改成更小的数字，相当于让数位和减少一定的量，而且这个减少量有上限

- 对于非最高位，可以改成 $0$，因此这一位最多能让数位和减少 $d$

- 对于最高位，不能改成 $0$，否则出现前导零，最小只能改成 $1$，因此这一位最多能让数位和减少 $d-1$
  
那么：把每一位的这个上限记为 $cap$，最高位 $cap=d-1$，其余位 $cap=d$

之后，我们使用贪心来选择最少次数：

我们要用尽量少的位，使 $cap$ 之和至少达到 $need$

因为每次操作的代价都相同，用 $k$ 次操作能获得的总减少量最多就是最大的 $k$ 个 $cap$ 的和

因此只需要把所有 $cap$ 从大到小排序，依次取最大的去抵消 $need$，直到 $need \le 0$

取了多少个 cap，就是最少操作次数

## 代码
```c++
#include <bits/stdc++.h>

using namespace std;

long long F(long long x) {
    auto s = to_string(x);
    long long result = 0;
    for (auto c : s) {
        result += c - '0';
    }
    return result;
}

int main() {
    int t;
    cin >> t;
    while (t --) {
        long long x;
        cin >> x;

        long long sum = F(x);
        if (sum <= 9) {
            cout << 0 << '\n';
            continue;
        }

        int need = static_cast<int>(sum - 9);

        string s = to_string(x);
        vector<int> caps;
        caps.reserve(s.size());

        for (int i = 0; i < (int)s.size(); i ++) {
            int d = s[i] - '0';
            caps.push_back(i == 0 ? d - 1 : d);
        }

        sort(caps.rbegin(), caps.rend());

        int ans = 0;
        for (int cap : caps) {
            if (need <= 0) break;
            need -= cap;
            ans ++;
        }

        cout << ans << '\n';
    }
}

```