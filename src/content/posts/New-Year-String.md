---
title: New-Year-String
published: 2026-05-10
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法, 每日一题]
category: '算法'
draft: false 
lang: ''
---
## 题面
We will call a string consisting of characters 0, 2, 5, and/or 6 a New Year string if **at least one** of the two conditions is met:

-   it contains the string 2026 as a continuous substring;
-   it does not contain the string 2025 as a continuous substring.

For example, the strings 20252026, 21026, 20262026, 000 are New Year strings. The strings 2025, 20256, 20252025, 000202500020226 are not New Year strings.

You are given a string $s$. You can perform the following operation any number of times (possibly zero):

-   choose a character in the string $s$ and replace it with 0, 2, 5, or 6 (you can choose any one of these four characters).

Calculate the minimum number of operations needed to make the string $s$ a New Year string.

**Input**

The first line contains one integer $t$ ($1 \le t \le 10^4$) — the number of test cases.

Each test case consists of two lines:

-   the first line contains one integer $n$ ($4 \le n \le 20$) — the number of characters in $s$;
-   the second line contains $s$ — a sequence of $n$ characters 0, 2, 5, and/or 6.
-   
**Output**

For each test case, output one integer — the minimum number of operations needed to make the string $s$ a New Year string.
## 思路
理清题意，满足题目要求的字符串只需要下面两个条件至少满足一个即可：

- 包含子串 `2026`
- 不包含子串 `2025`

所以如果一个字符串不满足要求，那么它一定不包含子串 `2026` 且包含 `2025`

注意到我们只需要一次修改，将 `2025` 改为 `2026` 即可让字符串满足要求

所以答案只能是 $0$ 或者 $1$
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
        string s;
        cin >> s;

        bool a = (s.find("2025") != string::npos);
        bool b = (s.find("2026") != string::npos);

        if (b || !a) cout << 0 << endl;
        else cout << 1 << endl;
    }
}
```