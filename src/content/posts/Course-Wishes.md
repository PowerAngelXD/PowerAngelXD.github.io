---
title: Course-Wishes
published: 2026-04-16
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法, 每日一题]
category: '算法'
draft: false 
lang: ''
---
## 题面
qwqkawaii is registering for $n$ ($n\le 50$) courses. In the registration system, he can submit a course wish for each course, which indicates his priority for taking that course.

Course wishes are divided into $k+1$ ($k\le 20$) priority levels, where level $1$ is the highest priority and level $k+1$ is the lowest.

The first $k$ wish levels have capacity limits: for each $1 \le i \le k$, at most $a_i$ courses can be assigned wish level $i$. Note that wish level $k+1$ has no capacity limit.

Initially, the $i$\-th course has wish level $b_i$, and it is guaranteed that this initial assignment satisfies all capacity limits. Now qwqkawaii wants to adjust all his courses to wish level $k + 1$. To achieve this, he can apply the following operation at most $1000$ times:

-   Select a course $i$ ($1\le i\le n$), then increase $b_i$ by $1$.

Note that:

-   A course at level $k+1$ cannot be selected;
-   After every single operation, all capacity limits **must** still be satisfied.

Your task is to construct a valid adjustment sequence with at most $1000$ operations, or report that it is impossible.

**Input**

Each test contains multiple test cases. The first line contains the number of test cases $t$ ($1 \le t \le 50$). The description of the test cases follows.

The first line of each test case contains two integers $n$ and $k$ ($1 \le n \le 50$, $1 \le k \le 20$) — the number of courses and the number of priority levels (excluding the lowest priority level).

The second line contains $k$ integers $a_1, a_2, \ldots, a_k$ ($1 \le a_i \le n$) — the capacity limits of the first $k$ wish levels.

The third line contains $n$ integers $b_1, b_2, \ldots, b_n$ ($1 \le b_i \le k+1$) — the initial wish levels of the courses.

It is guaranteed that the initial assignment satisfies all capacity limits.

**Output**

For each test case, if it is impossible to reach the target state, print a single integer $-1$.

Otherwise, print the number of operations $m$ ($0 \le m \le 1000$) on the first line of output.

Then print one line with $m$ integers $u_1, u_2, \ldots, u_m$ ($1 \le u_i \le n$), denoting that in the $i$\-th operation, you increase the wish level $b_{u_i}$ of course $u_i$ by $1$.
## 思路
题目要求构造一种操作顺序，把所有课程从当前等级逐步提升到 $k+1$，并且每一步后都不能违反容量限制

考虑贪心：每次都从当前“最高的非空等级”中取一门课上移

设当前最高非空等级为 $x$（$1 \le x \le k$），把一门课从 $x$ 移到 $x+1$：

- $x$ 层人数减少，一定不会超容量
- 由于 $x$ 已经是当前最高层，所以 $x+1..k$ 都为空，特别是 $x+1$ 层原本为 $0$
- 移动后 $x+1$ 层最多变成 $1$，而题目给出 $a_i \ge 1$，因此也合法
- 若 $x=k$，则进入 $k+1$（无容量限制），同样合法

因此这个过程一定可以持续进行，直到 $1..k$ 全部为空，此时所有课程都在 $k+1$

总操作数是固定的：
$$
m = \sum_{i=1}^{n}(k+1-b_i)
$$
由约束 $n \le 50, k \le 20$，有 $m \le 50 \times 20 = 1000$，满足题目要求

实现时可用 `levels[1..k+1]` 存每个等级的课程编号，每轮从 $k$ 向 $1$ 找最高非空层并执行一次移动，记录课程编号到答案即可。复杂度为 $O(mk)$
## 代码
```c++
#include <bits/stdc++.h>

using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int t;
    cin >> t;
    while (t--) {
        int n, k;
        cin >> n >> k;

        vector<int> a(k + 1), b(n + 1);
        for (int i = 1; i <= k; i++) cin >> a[i];
        for (int i = 1; i <= n; i++) cin >> b[i];

        vector<vector<int>> levels(k + 2);
        vector<int> cnt(k + 2, 0);

        for (int i = 1; i <= n; i++) {
            levels[b[i]].push_back(i);
            if (b[i] <= k) cnt[b[i]]++;
        }

        vector<int> ans;
        ans.reserve(1000);

        bool fail = false;

        while (true) {
            int x = 0;
            for (int lv = k; lv >= 1; lv--) {
                if (!levels[lv].empty()) {
                    x = lv;
                    break;
                }
            }

            if (x == 0) break;

            if (x < k && cnt[x + 1] + 1 > a[x + 1]) {
                fail = true;
                break;
            }

            int id = levels[x].back();
            levels[x].pop_back();

            ans.push_back(id);

            cnt[x]--;
            if (x + 1 <= k) cnt[x + 1]++;

            levels[x + 1].push_back(id);

            if ((int)ans.size() > 1000) {
                fail = true;
                break;
            }
        }

        if (fail) {
            cout << -1 << '\n';
            continue;
        }

        cout << ans.size() << '\n';
        for (int i = 0; i < (int)ans.size(); i++) {
            if (i) cout << ' ';
            cout << ans[i];
        }
        cout << '\n';
    }

    return 0;
}
```