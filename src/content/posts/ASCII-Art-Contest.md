---
title: ASCII-Art-Contest
published: 2026-05-25
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法, 每日一题]
category: '算法'
draft: false 
lang: ''
---
## 题面
Three leading AI-powered creative assistants—Gemini, ChatGPT, and Claude—enter the first ever ASCII Art Contest, where they must impress a panel of human judges with their text-based masterpieces.

Each participant receives a score between 80 and 100 (inclusive). The organizers want to announce the final standing only if the judges' opinions are "close enough"; otherwise, they will ask the judges to reconsider.

Given the three integer scores of Gemini, ChatGPT, and Claude, determine the contest result:

-   If the maximum score and the minimum score differ by at least 10 points, print check again (the judging seems inconsistent, so the panel must re-evaluate).
-   Otherwise, print final X, where X is the median of the three scores (the score that would be in the middle if all three were sorted in non-decreasing order).

**Input**

A single line contains three integers $g, c, \ell$, representing the scores of Gemini, ChatGPT, and Claude respectively.

-   $80\le g, c, \ell \le 100$

**Output**

Print the required answer in a line.
## 思路
直接按题目意思求就行，没啥多说的
## 代码
```c++
#include <bits/stdc++.h>

using namespace std;

int main() {
    int g,c,t;
    cin >> g >> c >> t;
    int mx = max(max(g, c), t);
    int mn = min(min(g, c), t);
    if (mx - mn >= 10) cout << "check again" << endl;
    else {
        int mid = g + c + t - mx - mn;
        cout << "final " << mid << endl;
    }
}

```