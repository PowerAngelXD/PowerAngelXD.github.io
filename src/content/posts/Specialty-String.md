---
title: Specialty-String
published: 2026-05-13
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法, 每日一题]
category: '算法'
draft: false 
lang: ''
---
## 题面
AksLolCoding is playing a game on a string $s$ of length $n$. Initially, $s$ contains only lowercase Latin characters.

In one turn, AksLolCoding can choose any pair of integers $(i,j)$ such that:

-   $1 \leq i \lt j \leq n$;
-   $s_i = s_j \neq *$; and
-   $s_k = *$ for all $i \lt k \lt j$.

If no such $i,j$ exists, then the game ends. Otherwise, AksLolCoding sets $s_i:=*$ and $s_j:=*$.

Once the game ends, AksLolCoding wins if and only if every character in $s$ is equal to $*$. Determine if it is possible for AksLolCoding to win.

**Note:** $*$ represents ASCII character 42.

**Input**

The first line contains an integer $t$ ($1 \leq t \leq 100$), the number of test cases.

The first line of each test case contains an integer $n$ ($1 \leq n \leq 5000$), the length of the string.

The second line of each test case contains a string $s$ consisting of lowercase Latin characters.

The sum of $n$ over all test cases does not exceed $5000$.

**Output**

Output the answer to each test case on its own line. If AksLolCoding can win, output "YES" (without quotes). Else, output "NO" (without quotes).

You can output the answer in any case (upper or lower). For example, the strings "yEs", "yes", "Yes", and "YES" will be recognized as positive responses.
## 思路
分析题意：将 $*$ 视为删除字符，条件 $s_i=s_j$ 且 $(i,j)$ 间全为 $*$ 等价于当前未删除序列中出现相邻相等字符\
一次操作即删除一对相邻相等字符，问能否通过反复删除将字符串清空\
\
我们可以观察到：\
\
用栈维护当前消除后的序列，从左到右扫描字符 $c$\
若栈非空且栈顶为 $c$，则形成相邻对可消除，弹栈\
否则将 $c$ 入栈\
扫描结束栈空则输出 YES，否则输出 NO
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

        stack<char> stk;
        for (int i = 0; i < n; i ++) {
            if (!stk.empty() && stk.top() == s[i]) stk.pop();
            else stk.push(s[i]);
        }

        if (stk.empty()) cout << "YES" << endl;
        else cout << "NO" << endl;
    }
    return 0;
}
```