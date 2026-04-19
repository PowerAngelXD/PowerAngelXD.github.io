---
title: Right-Maximum
published: 2026-04-19
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法, 每日一题]
category: '算法'
draft: false 
lang: ''
---
# Right Maximum
## 题面
You are given an array $a$ consisting of $n$ integers.

While the array is not empty, an operation is performed consisting of two steps:

-   first, the maximum element in the array is chosen (if there are multiple maximum elements, the rightmost maximum is chosen);
-   then, all elements after the chosen element, **including it**, are removed from the array.

Your task is to calculate the number operations that will be performed before the array becomes empty.

**Input**

The first line contains one integer $t$ ($1 \le t \le 10^4$) — the number of test cases.

Each test case consists of two lines:

-   the first line contains one integer $n$ ($2 \le n \le 2 \cdot 10^5$);
-   the second line contains $n$ integers $a_1, a_2, \dots, a_n$ ($1 \le a_i \le n$).

Additional constraint on the input: the sum of $n$ over all test cases does not exceed $2 \cdot 10^5$.

**Output**

For each test case, print one integer — the number of operations that will be performed.
## 思路
题目很简单，我们只需要规定一个右边界 $r$，让这个右边界等于每一次找到的最大值所在的索引即可，每一次查找就算一次次数，最终得到想要的结果

但是，如果直接暴力写法，会导致 `TLE`

所以，我们采用下面的方法：

先预处理一个数组 $pos$，其中 $pos[i]$ 表示前缀区间 $[0, i]$ 内最右侧最大值的下标；这样，转移时只要维护当前最大值 $mx$ 和位置即可

如果 $a[i] \ge mx$，就更新 $mx=a[i]$ 且 $pos[i]=i$

否则继承前一个结果，令 $pos[i]=pos[i-1]$

这样一遍扫描就能把所有前缀的答案算完，预处理复杂度是 $O(n)$

然后模拟操作时不再重新扫描前缀找最大值，而是设当前剩余数组右边界为 $r$，对应区间是 $[0, r-1]$；那么，本次应选择的下标就是 $pos[r-1]$

执行删除后新的右边界直接跳到 $r=pos[r-1]$

每跳一次就代表一次操作，答案加 $1$

原本暴力做法每次都要在前缀内找最大值，最坏复杂度是

$$
O(n+(n-1)+\cdots+1)=O(n^2)
$$

优化后是一次线性预处理加若干次 $O(1)$ 跳转，总复杂度为 $O(n)$

结合数据范围 $\sum n \le 2\times 10^5$，线性做法可以稳定通过
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

        vector<int> pos(n);
        int mx = a[0];
        pos[0] = 0;
        for (int i = 1; i < n; i ++) {
            if (a[i] >= mx) {
                mx = a[i];
                pos[i] = i;
            } 
            else {
                pos[i] = pos[i - 1];
            }
        }

        int r = n;
        int ans = 0;
        while (r > 0) {
            ans ++;
            r = pos[r - 1];
        }
        cout << ans << endl;
    }
}
```
