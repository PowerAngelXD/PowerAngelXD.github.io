---
title: Yes-or-Yes
published: 2026-05-20
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法, 每日一题]
category: '算法'
draft: false 
lang: ''
---
## 题面
Last Christmas, your friend Fernando gifted you a string $s$ consisting only of the characters $\mathtt{Y}$ and $\mathtt{N}$, representing "Yes" and "No", respectively.

You can repeatedly apply the following operation on $s$:

-   Choose any two adjacent characters and replace them with their [logical OR](https://en.wikipedia.org/wiki/Logical_disjunction).

Formally, in each operation, you can choose an index $i$ ($1 \leq i \leq |s|-1$), remove the characters $s_i$ and $s_{i+1}$, then insert:

-   A single $\mathtt{Y}$ if at least one of $s_i$ or $s_{i+1}$ is $\mathtt{Y}$;
-   A single $\mathtt{N}$ if both $s_i$ and $s_{i+1}$ are $\mathtt{N}$.

Note that after each operation, the length of $s$ decreases by $1$.

Unfortunately, Fernando does not want you to combine "Yes OR Yes", as he has experienced trauma relating to a certain song.

Determine whether it is possible to reduce $s$ to a single character by repeatedly applying the operation above, **without** ever combining two $\mathtt{Y}$'s.

**Input**

Each test contains multiple test cases. The first line contains the number of test cases $t$ ($1 \le t \le 500$). The description of the test cases follows.

The only line of each test case contains the string $s$ ($2\le |s|\le 100$). It is guaranteed that $s_i = \mathtt{Y}$ or $\mathtt{N}$.

**Output**

For each test case, print "YES" if the string can be reduced to a single character by repeatedly applying the described operation, and "NO" otherwise.

You can output the answer in any case (upper or lower). For example, the strings "yEs", "yes", "Yes", and "YES" will be recognized as positive responses.

## 思路
观察题目所给的合并条件：

- 合并 `NN`，最终变成 `N`，而 `Y` 的个数不变
- 合并 `NY/YN`，最终变成 `Y`， `Y` 的个数不变，因为原串也只有一个 `Y`
- 合并 `YY`，最终变成 `Y`，此时 `Y` 的个数改变了，但这个操作是不可行的

因此，字符串里 `Y` 的个数就是初始 `Y` 的个数

因此，给出结论，当 `Y` 的个数小于等于 $1$ 的时候输出 `YES`，否则就是 `NO`
## 代码
```c++
#include <bits/stdc++.h>

using namespace std;

int main() {
    int t;
    cin >> t;
    while (t --) {
        string s;
        cin >> s;

        int y = 0;
        for (int i = 0; i < s.length(); i ++) {
            if (s[i] == 'Y') y ++;
        }

        if (y <= 1) cout << "YES" << endl;
        else cout << "NO" << endl;
    }
}
```