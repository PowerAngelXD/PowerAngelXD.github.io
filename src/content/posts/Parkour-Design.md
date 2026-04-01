---
title: Parkour-Design
published: 2026-03-22
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法, 每日一题]
category: '算法'
draft: false 
lang: ''
---
## 题面
Today, Alex wants to build a parkour course for Steve to train his parkour skills on. A parkour course is a sequence $p_0 \to p_1 \to \ldots \to p_k$ of integer coordinates on the plane. Here, a contiguous pair of coordinates is called a move, denoted as $p_{i-1} \to p_i$.

Alex knows that Steve can only perform the following types of moves:

-   $(x_i,y_i) \to (x_i+2,y_i+1)$;
-   $(x_i,y_i) \to (x_i+3,y_i)$;
-   $(x_i,y_i) \to (x_i+4,y_i-1)$.

Note that Steve will not perform any other type of moves. For example, Steve can perform $(0,0) \to (2,1)$ and $(2,1) \to (5,1)$, but will **never** perform moves such as $(2,1) \to (3,2)$, $(3,0) \to (5,-1)$, or $(4,-1) \to (6,-1)$ (even though they may look very easy).

You are given an integer coordinate $(x,y)$ on the plane.

Please determine if it is possible to make a parkour course $q_0,q_1,\ldots,q_k$ that satisfies the following conditions:

-   $q_0=(0,0)$;
-   $q_k=(x,y)$;
-   The parkour course **only consists of** moves that Steve can perform.

**Input**

Each test contains multiple test cases. The first line contains the number of test cases $t$ ($1 \le t \le 10^3$). The description of the test cases follows.

The only line of each test case contains two integers $x$ and $y$ ($1 \le x \le 10^9$, $-10^8 \le y \le 10^8$).

**Output**

If it is possible to make a parkour course that satisfies the conditions, output "YES" on a separate line.

If it is **impossible** to make a parkour course that satisfies the conditions, output "NO" on a separate line.

You can output the answer in any case. For example, the strings "yEs", "yes", and "Yes" will also be recognized as positive responses.

## 思路
开始的时候，想使用dfs进行暴力搜索，从给定的目标坐标 `x, y` 一直逆推，检查是否为0

但是这个方法时间复杂度过高且冗余，所以我们需要换一个更简单的方法

注意到，若分别执行 $a$，$b$，$c$ 次三种动作（顺序任意），会有下面的方程：
$$
\begin{equation*}
   \begin{cases}
       x = 2a + 3b + 4c \\
       y = a - c
   \end{cases}
\end{equation*}
$$

通过消元，可以得到： $a = y + c$

代入第一个方程：$x = 2(y + c) + 3b + 4c$

化简得到：$x - 2y = 3(b + 2c)$

由 $a$，$b$，$c$ 三个量的意义可知，满足不等式：$x - 2y \geq 0$

因此，我们需要让这个不等式成立，因此令 $d = x - 2y$，先计算$d$是否满足 $d\space mod \space 3 = 0$，同时，$d$ 应该非负:

```c++
if (d < 0 || d % 3 != 0) {
    cout << "NO\n";
    continue;
}
```

之后，当这个判定成功之后，我们需要得到 $b + 2c$，因此我们令 $k = \frac{x - 2y}{3} = b + 2c$

同时，由：$a = y + c$，且a非负，可以得到：$c \geq -y$

我们基于上面的不等式，对 $k = \frac{x - 2y}{3} = b + 2c$ 可以移项得到：$b = k - 2c$

这里，由于 $b$ 也是非负，所以有：$c \leq \lfloor\frac{k}{2}\rfloor$

因此，我们得到了一个关于 $c$ 的不等式组：
$$
\begin{equation*}
    \begin{cases}
        c \geq -y \\
        c \leq \lfloor\frac{k}{2}\rfloor \\
        c \geq 0
    \end{cases}
\end{equation*}
$$
整理得：
$$
max(0, -y) \leq c \leq \lfloor\frac{k}{2}\rfloor
$$
因此，我们只需要判断这个不等式是否成立即可，最简单的判断方法则是判断两个端点的关系是否成立：
```c++
long long needC = max(0LL, -y);
long long maxC = k / 2;

if (maxC >= needC) cout << "YES\n";
else cout << "NO\n";
```
至此，这题结束

## 完整代码
```c++
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int t;
    cin >> t;
    while (t--) {
        long long x, y;
        cin >> x >> y;

        long long d = x - 2 * y;

        if (d < 0 || d % 3 != 0) {
            cout << "NO\n";
            continue;
        }

        long long k = d / 3;
        long long needC = max(0LL, -y);
        long long maxC = k / 2;

        if (maxC >= needC) cout << "YES\n";
        else cout << "NO\n";
    }

    return 0;
}
```
