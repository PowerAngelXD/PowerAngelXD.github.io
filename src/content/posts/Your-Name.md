---
title: Your-Name
published: 2026-05-28
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法, 每日一题]
category: '算法'
draft: false 
lang: ''
---
## 题面
khba is writing his girlfriend's name. He has $n$ cubes, each with one lowercase Latin letter written on it. They are arranged in a row, forming a string $s$. His girlfriend's name is also a string $t$, consisting of $n$ lowercase Latin letters.

To prove his love, he must check whether it is possible to rearrange the letters of string $s$ so that it becomes her name $t$.

**Input**

The first line contains an integer $q$ ($1 \le q \le 1000$) — the number of test cases.

The first line of each test case contains an integer $n$ ($1 \le n \le 20$).

The second line of each test case contains two distinct strings $s$ and $t$, each consisting of $n$ lowercase Latin letters.

**Output**

For each test case, output "YES" if the letters of $s$ can be arranged to form $t$; otherwise, output "NO".

You can output the answer in any case (upper or lower). For example, the strings "yEs", "yes", "Yes" and "YES" will be recognized as positive responses.
## 思路
我们只需要检查字符串 $s$ 是否有可能排列出字符串 $t$ 即可\
\
但是需要注意的是：
- 对应字符都要出现
- 对应字符个数要满足重排 $t$ 的需求，换言之，对应字符可以比需求字符多，但是不能少

## 代码
```c++
#include <bits/stdc++.h>

using namespace std;

int main() {
    int t;
    cin >> t;
    while (t --) {
        int n;
        string s, t;
        cin >> n >> s >> t;

        map<char, int> counts;
        for (int i = 0; i < n; i ++) {
            if (counts.find(s[i]) == counts.end()) {
                counts[s[i]] = 1;
            }
            else if (counts.find(s[i]) != counts.end()) counts[s[i]] ++;
        }

        map<char, int> matches;
        for (int i = 0; i < n; i ++) {
            if (matches.find(t[i]) == matches.end()) {
                matches[t[i]] = 1;
            }
            else if (matches.find(t[i]) != matches.end()) matches[t[i]] ++;
        }

        bool success = true;
        for (int i = 0; i < n; i ++) {
            if (matches.find(t[i]) == matches.end()) {
                success = false;
                break;
            }

            if (matches[t[i]] > counts[t[i]]) {
                success = false; 
                break;
            }
        }

        if (success) cout << "YES" << endl;
        else cout << "NO" << endl;
    }
}
```