---
title: Heapify-1
published: 2026-03-26
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法]
category: '算法'
draft: false 
lang: ''
---
# Heapify-1
## 题面
You are given a permutation $a$ of length $n$ $^{\text{∗}}$.

You can perform the following operation any number of times (possibly zero):

-   Select an index $i$ ($1 \le i \le \frac{n}{2}$), and swap $a_i$ and $a_{2i}$.

For example, when $a=[1,4,2,3,5]$, you can swap $a_2$ and $a_4$ to make it $[1,3,2,4,5]$, but you cannot swap $a_2$ and $a_3$.

Please determine if the sequence $a$ can be sorted in increasing order.

$^{\text{∗}}$A permutation of length $n$ is an array consisting of $n$ distinct integers from $1$ to $n$ in arbitrary order. For example, $[2,3,1,5,4]$ is a permutation, but $[1,2,2]$ is not a permutation ($2$ appears twice in the array), and $[1,3,4]$ is also not a permutation ($n=3$ but there is $4$ in the array).

**Input**

Each test contains multiple test cases. The first line contains the number of test cases $t$ ($1 \le t \le 10^4$). The description of the test cases follows.

The first line of each test case contains a single integer $n$ ($1 \le n \le 2 \cdot 10^5$).

The second line of each test case contains $n$ **distinct** integers $a_1,a_2,\ldots,a_n$ ($1 \le a_i \le n$).

It is guaranteed that the sum of $n$ over all test cases does not exceed $2 \cdot 10^5$.

**Output**

If $a$ can be sorted in increasing order, output "YES" on a separate line. Otherwise, output "NO" on a separate line.

You can output the answer in any case. For example, the strings "yEs", "yes", and "Yes" will also be recognized as positive responses.
## 思路
注意到题目只允许交换 $a_{i}$ 和 $a_{2i}$ 位置的元素，因此，有多条延伸出来的链：
$$
a_1 \rightarrow a_2 \rightarrow a_4 \rightarrow a_8 \\
a_3 \rightarrow a_6 \rightarrow a_{12} \rightarrow a_{24} \\
a_5 \rightarrow a_{10} \rightarrow a_{20} \rightarrow a_{40} \\
\dots
$$
因此，每个交换只能在这一个个链上进行，我们也只需要验证：排序的方式不能跨链条进行，否则无法进行排序

如果对每条链条进行反复交换尝试排序，最终得到的结果是有序的，那么就说明可以排序，否则就是不可以排序

所以在代码的编写上，我们需要首先从每个链条的开头进行：
```c++
for (int i = 1; i <= n; i += 2)
```
由于一条链上不可能只有一次交换，所以我们需要多次交换

确定多次交换之后，我们还需要一层循环能够让我们沿着一条链路进行下去：$k = 2i, 4i, 8i \dots$，并比较 $a[k/2], a[k]$ 如果前者大，就进行交换操作；本质就是一个冒泡排序

但是，如果我们不想要过多的循环次数，让循环次数确定下来呢？

通过我们的分析，一条链的长度设为 $L$，由于我们知道一条链：$i \rightarrow 2i \rightarrow 4i \rightarrow 8i \rightarrow \dots \leq n$

所以很容易得到：
$$
i \cdot 2^{L - 1} \leq n
$$

所以可以推得：
$$
L = \lfloor log_2{\frac{n}{i}} \rfloor + 1
$$
因此可以写出如下代码用于确定循环次数：
```c++
for (int j = i; j <= n; j *= 2)
```
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

        for (int i = 1; i <= n; i += 2) {
            for (int j = i; j <= n; j *= 2) {
                for(int k=i*2;k<=n;k*=2) {
                    if(a[k/2]>a[k]) 
                        swap(a[k/2],a[k]);
                }
            }
        }

        if(is_sorted(begin(a),end(a)))
            cout<<"YES\n";
        else cout<<"NO\n";
    }
}
```