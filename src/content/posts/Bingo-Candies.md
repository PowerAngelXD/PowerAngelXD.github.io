---
title: Bingo-Candies
published: 2026-04-20
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法, 每日一题]
category: '算法'
draft: false 
lang: ''
---
# Bingo Candies
## 题面
Alice has a magic board. The board is described as a $n\times n$ grid; each tile has a colored candy in it. The color of the candy in the $i$\-th row, $j$\-th column is $a_{i,j}$.

Bob wants to know if he can rearrange the board in some way so that no row or column consists of $n$ candies of the same color.

Your task is to determine whether such a rearrangement exists.

**Input**

Each test contains multiple test cases. The first line contains the number of test cases $t$ ($1 \le t \le 500$). The description of the test cases follows.

The first line of each test case contains an integer $n$ ($1\le n\le 100$), denoting the size of the board.

The following $n$ lines contain $n$ integers each; the $j$\-th integer on the $i$\-th line is $a_{i,j}$ ($1\le a_{i,j}\le n^2$), denoting the color of candies on the board.

It is guaranteed that the sum of $n$ over all test cases does not exceed $500$.

**Output**

For each test case, print "YES" if a valid rearrangement exists, and "NO" otherwise.

You can output the answer in any case (upper or lower). For example, the strings "yEs", "yes", "Yes", and "YES" will be recognized as positive responses.
## 思路
题目的要求是求一个重排，使得每一行/列上没有完全相同的元素

很明显，要满足这个条件，最多的那个元素的个数必须有一个上界

很明显，对于题目的 $n \times n$ 的矩阵，这个上界是 $n(n-1)$

因此我们只需要计算出现次数最多的元素的个数是否大于 $n(n-1)$ 即可
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
        map<int, int> count;

        for (int i = 0; i < n; i ++) {
            for (int j = 0; j < n; j ++) {
                int a;
                cin >> a;
                count[a] ++;
            }
        }
        bool flag = false;
        for (int i = 1; i <= n * n; i ++) {
            if (count[i] > n * (n - 1)) flag = true;
        }

        if (flag) cout << "NO\n";
        else cout << "YES\n";
    }
}
```