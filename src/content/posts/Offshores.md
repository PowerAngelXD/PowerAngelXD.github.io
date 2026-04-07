---
title: Offshores
published: 2026-04-07
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法, 每日一题]
category: '算法'
draft: false 
lang: ''
---
## 题面
Mark loves money very much, and he keeps most of it in the bank. He stores his money in $n$ different banks. In the $i$\-th bank, he has $a_i$ rubles.

One day, Mark decided to gather all his money in one bank, so he will be transferring money from one bank account to another. All interbank transfers are arranged the same way: he can transfer only $x$ rubles in one transfer, and considering all fees, $y$ rubles will be credited to the other account (since banks want to make a profit, it holds that $y \leq x$).

It is possible that Mark will not be able to transfer all his money to one bank, but he wants to find the maximum number of rubles that can end up in any bank.

**Input**

Each test contains multiple test cases. The first line contains the number of test cases $t$ ($1 \le t \le 10^4$). The description of the test cases follows.

The first line of each test case contains three integers $n$, $x$, and $y$ ($2 \le n \le 2 \cdot 10^5$, $1 \le y \le x \le 10^9$) — the number of banks, the transfer amount, and the credited amount.

The second line of each test case contains $n$ integers $a_1, a_2, \ldots, a_n$ ($1 \le a_i \le 10^9$) — the initial amount of rubles in each bank.

It is guaranteed that the sum of the values of $n$ across all test cases does not exceed $2 \cdot 10^5$.

**Output**

For each test case, output a single number — the maximum number of rubles that can be obtained in any bank.
## 思路
首先理解题意：进行若干次转账操作，使得出现一家银行金额最大

> [!WARNING]
> **易错：最大银行的选择**
> 
> 我在初次看到这道题的时候误以为：“这个银行必定是未存钱前的金额最大的银行”
>
> 但事实不是这样，当有：$a=[100,99], x=10, y=9$ 时，选择第二个银行（99）反而是最优解
>
> 解释：当 $x=10$ 时，第一个银行转账十次，而第二个银行只用转账九次，多余资金达到了 $9$

理清上面的内容，我们就要着手解决问题

我们可以假设第 $k$ 个银行是我们要求的银行，对于任意一个银行，我们知道，最多转出次数为：
$$
\lfloor \frac{a_i}{x} \rfloor
$$

如果要保证这个银行存有的资金是最多的，那么需要满足下面的条件：

- 其它银行将全部的能转移的次数全部给了自己
- 自己不向外转钱

那么我们可以得到第 $k$ 个银行的最大金额为：
$$
S = a_k + (\sum_{i=1}^n \lfloor \frac{a_i}{x} \rfloor - \lfloor \frac{a_k}{x} \rfloor) \cdot y
$$

因此，我们只需要对所有银行求出其最大金额 $S$，并从中找出最大的即可

## 代码
```c++
#include <bits/stdc++.h>

#define int long long

using namespace std;

signed main() {
    signed t;
    cin >> t;
    while (t --) {
        int n, x, y;
        cin >> n >> x >> y;
        vector<int> a(n + 1);
        for (int i = 1; i <= n; i ++) cin >> a[i];
        
        int b = 0;
        for (int i = 1; i <= n; i ++) b += a[i] / x;
        
        vector<int> r;
        for (int i = 1; i <= n; i ++) {
            int R = 0;
            R = a[i] + (b - a[i] / x) * y;
            r.push_back(R);
        }

        cout << *max_element(r.begin(), r.end()) << endl;
    }
}
```