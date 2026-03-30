---
title: Sieve-of-Erato67henes
published: 2026-03-27
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法]
category: '算法'
draft: false 
lang: ''
---
# Sieve-of-Erato67henes
## 题面
You are given $n$ **positive** integers $a_1,a_2,\ldots,a_n$.

Please determine if it is possible to select any number of elements in $a$, so that their product is $67$.

Note that you may not select zero elements, as the product of zero elements is not defined in this problem.

**Input**

Each test contains multiple test cases. The first line contains the number of test cases $t$ ($1 \le t \le 10^4$). The description of the test cases follows.

The first line of each test case contains a single integer $n$ ($1 \le n \le 5$).

The second line of each test case contains $n$ positive integers $a_1,a_2,\ldots,a_n$ ($1 \le a_i \le 67$).

**Output**

If it is possible to select elements so that their product is $67$, output "YES" on one line. Otherwise, output "NO" on one line.

You can output the answer in any case. For example, the strings "yEs", "yes", and "Yes" will also be recognized as positive responses.
## 思路
很简单，题目给的 $67$ 是一个质数，同时注意到题目说了：****选择任意多个元素****，因此我们只需要判断选取的数有没有 $67$ 就可以了
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
        vector<int> a(n);
        for (int i = 0; i < n; i ++) cin >> a[i];

        if (find(a.begin(), a.end(), 67) != a.end())
            cout << "YES" << endl;
        else cout << "NO"<< endl;
    }
}
```