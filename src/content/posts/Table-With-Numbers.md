---
title: Table-With-Numbers
published: 2026-04-09
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法, 每日一题]
category: '算法'
draft: false 
lang: ''
---
# Table-with-Numbers
## 题面
Peter drew a table of size $h \times l$, filled with zeros. We will number its rows from $1$ to $h$ from top to bottom, and columns from $1$ to $l$ from left to right. Ned came up with an array of numbers $a_1, a_2, \ldots, a_n$ and wanted to modify the table.

Ned can choose $2k \leq n$ numbers from his array and split them into $k$ pairs. After that, for each resulting pair $x, y$, he takes the cell located in row $x$ and column $y$, and adds $1$ to the number in that cell. If such a cell does not exist, then this pair does nothing to the table.

Peter supported Ned's initiative and asked him to maximize the sum of the numbers in the table. Help Ned understand what the maximum sum he can achieve is.

**Input**

Each test contains multiple test cases. The first line contains the number of test cases $t$ ($1 \le t \le 500$). The description of the test cases follows.

The first line of each test case contains three integers $n$, $h$, and $l$ ($2 \le n \le 100$, $1 \le h, l \le 1000$) — the size of the array, the height of the table, and the width of the table, respectively.

The second line of each test case contains $n$ numbers $a_1$, $a_2$, $\ldots$, $a_n$ ($1 \le a_i \le 1000$) — the array itself.

**Output**

For each test case, output the maximum possible sum of the numbers in the table.
## 思路
先把题目理清楚：

- 表格初始全是 $0$，每做一个有效配对 $(x,y)$（满足 $1\le x\le h,\ 1\le y\le l$），表格总和就 $+1$
- 所以题目本质是：**最多能做出多少个有效配对**

### 1. 按“可担任角色”分类

一个数在配对里可能担任“行号”或“列号”

- 若 $a_i \le h$，它可以当行号
- 若 $a_i \le l$，它可以当列号

据此分 4 类：

1. `both`：$a_i \le h$ 且 $a_i \le l$（行列都能当）
2. `r`：$a_i \le h$ 且 $a_i > l$（只能当行）
3. `c`：$a_i > h$ 且 $a_i \le l$（只能当列）
4. `u`：$a_i > h$ 且 $a_i > l$（行列都不能当）

记数量分别为 $B,R,C$（第 4 类无需统计）

### 2. 哪些配对有效？

一个配对要有效，必须“有行也有列”：

- 有效：`r+c`、`both+r`、`both+c`、`both+both`
- 无效：`r+r`、`c+c`、以及任何含 `u` 的配对

因此，我们只要最大化上面 4 种有效配对的总数


### 3. 贪心构造（最优）

#### 第一步：先配 `r` 和 `c`

- 能配多少配多少：$x=\min(R,C)$
- 这一步每对都必定有效，且不会消耗 `both` 这种“万能资源”

更新：

- 答案加 $x$
- $R\leftarrow R-x,
\ C\leftarrow C-x$

#### 第二步：剩余单侧角色用 `both` 补齐

第一步后，$R,C$ 至多只有一个还大于 $0$。这些剩余元素必须和 `both` 配才有效

- 可补数量：$y=\min(B, R+C)$
- 答案加 $y$
- $B\leftarrow B-y$

#### 第三步：剩余 `both` 两两互配

- 每两枚 `both` 可以组成一对有效配对
- 贡献 $\left\lfloor B/2\right\rfloor$

最终答案：

$$
	ext{ans}=x+y+\left\lfloor\frac{B}{2}\right\rfloor
$$

> [!NOTE]
> **验证贪心正确性**
> 
> - `r` 和 `c` 彼此配对时，不需要 `both`，这是对资源最节省的做法
> - `both` 既能当行又能当列，是稀缺的“通用补位”
> 	如果过早让 `both` 参与配对，会挤占后续补齐单侧角色的能力，不> 会更优
> - 当单侧角色补齐后，剩下只有 `both`，唯一有效用法就是 `both + both`
> 
> 所以按上述顺序贪心，不会错过更优方案
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
        int n, h, l;
        cin >> n >> h >> l;

        vector<int> a(n + 1);
        for (int i = 1; i <= n; i ++) cin >> a[i];

        long long both = 0, r = 0, c = 0;
        for (int i = 1; i <= n; i ++) {
            if (a[i] <= h && a[i] <= l) {
                both ++;
            } else if (a[i] <= h) {
                r ++;
            } else if (a[i] <= l) {
                c ++;
            }
        }

        long long ans = 0;

        long long cross = min(r, c);
        ans += cross;
        r -= cross;
        c -= cross;

        long long b = min(both, r + c);
        ans += b;
        both -= b;

        ans += both / 2;

        cout << ans << '\n';

    }

    return 0;
}
```