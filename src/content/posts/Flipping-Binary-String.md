---
title: Flipping-Binary-String
published: 2026-04-15
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法, 每日一题]
category: '算法'
draft: false 
lang: ''
---
## 题面
You are given a binary string $s$ of length $n$. You can perform the following operation on the string:

-   Choose an index $i$, and flip the bit present at all other indices except for the index $i$. In other words, choose an integer $i$ ($1 \le i \le n$), and for all $j$ such that $1 \le j \le n$ and $j \ne i$, if $s_j = 0$, then set $s_j:=1$, otherwise set $s_j:=0$.

You can perform the operation any number of times, but each index can be chosen by **at most one** operation.

Your task is to make all bits present in the string $s$ equal $0$, or report that it is impossible to do so. You don't have to minimize the number of operations.

**Input**

Each test contains multiple test cases. The first line contains the number of test cases $t$ ($1 \le t \le 10^4$). The description of the test cases follows.

The first line of each test case contains a single integer $n$ ($1 \le n \le 2 \cdot 10^5$).

The second line of each test case contains the binary string $s$ of length $n$.

It is guaranteed that the sum of $n$ over all test cases does not exceed $2 \cdot 10^5$.

**Output**

For each test case, output $-1$ if it is impossible to transform all bits to $0$. Otherwise, output two lines in the following format:

-   In the first line, print the number of operations $x$ ($0 \leq x \leq n$).
-   In the second line of each test case, print $x$ numbers – the indices you select in each operation in order. You should guarantee that each index is chosen at most once.

If there are multiple possible solutions, you may output any.
## 思路
这题最关键的是把“操作”翻译成按位异或关系

我们设一个变量 $x_i \in \{0, 1\}$，表示下标 $i$ 是否被选择进行操作：

- $x_i = 1$：这个下标被选过一次
- $x_i = 0$：这个下标没被选

对于任意一个位置 $j$，它会被所有 $i \ne j$ 的操作翻转，因此最终状态满足：

$$
s'_j = s_j \oplus \bigoplus_{i \ne j} x_i
$$

我们再定义总异或：

$$
K = \bigoplus_{i=1}^{n} x_i
$$

那么就有：

$$
\bigoplus_{i \ne j} x_i = K \oplus x_j
$$

题目要求最终全为 $0$，即 $s'_j = 0$，代入可得：

$$
x_j = s_j \oplus K
$$

这句话很重要：**只要 $K$ 确定，所有 $x_j$ 都被唯一确定**

接下来做可行性判断，对上式整体异或：

$$
K = \bigoplus_{j=1}^{n} x_j
= \bigoplus_{j=1}^{n} (s_j \oplus K)
= \left(\bigoplus_{j=1}^{n} s_j\right) \oplus (n \bmod 2) \cdot K
$$

设 $P = \bigoplus s_j$（也就是字符串里 $1$ 的个数奇偶性），分情况：

- 当 $n$ 为偶数时：$K = P$，一定有解
- 当 $n$ 为奇数时：推出必须 $P = 0$，否则无解

因此无解条件只有一个：**$n$ 为奇数且 $1$ 的个数为奇数**

构造方式也就很直接：

1. 先求 $P$
2. 如果 $n$ 是奇数且 $P=1$，输出 $-1$
3. 否则可行，取
	- 偶数 $n$：$K = P$
	- 奇数 $n$：可取 $K = 0$
4. 按照 $x_j = s_j \oplus K$ 逐位计算，若 $x_j = 1$，就把下标 $j+1$ 放进答案

最终答案数组里的下标就是要执行操作的位置，每个下标最多出现一次，完全符合题意
## 代码
```c++
#include <bits/stdc++.h>

using namespace std;

int main() {
    int t;
    cin >> t;
    while (t --) {
        int n;
        string s;
        cin >> n >> s;
        int pp = 0;
        for (auto c: s) { 
            if (c == '1') pp ++;
        }
        int p = pp % 2;
        if (n % 2 != 0 && p == 1) cout << -1 << endl;
        else {
            int k;
            if (p == 1) k = 1;
            else k = p;
            vector<int> ans, x;
            for (int i = 0; i < n; i ++) {
                x.push_back((s[i] - '0') ^ k);
                if (x[i] == 1) ans.push_back(i + 1);
            }
            cout << ans.size() << '\n';
            for (int i = 0; i < (int)ans.size(); i++) {
                cout << ans[i] << " \n"[i == (int)ans.size() - 1];
            }
            if (ans.empty()) cout << '\n';
        }
    }
}
```