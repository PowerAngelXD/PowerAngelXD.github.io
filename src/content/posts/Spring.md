---
title: Spring
published: 2026-04-21
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法, 每日一题]
category: '算法'
draft: false 
lang: ''
---
## 题面
Alice, Bob, and Carol visit a spring to collect water. Alice visits every $a$ days (on days $a, 2a, 3a, \dots$), Bob visits every $b$ days (on days $b, 2b, 3b, \dots$), and Carol visits every $c$ days (on days $c, 2c, 3c, \dots$).

When only one person visits, they collect $6$ liters of water. If multiple people visit, the water is divided equally: two people take $3$ liters each, and three people take $2$ liters each.

Your task is to calculate how much water Alice, Bob, and Carol collect over $m$ days, starting from day $1$.

**Input**

The first line contains a single integer $t$ ($1 \le t \le 10^4$) — the number of test cases.

The only line of each test case contains four integers $a$, $b$, $c$, and $m$ ($1 \le a, b, c \le 10^6$; $1 \le m \le 10^{17}$).

**Output**

For each test case, print three integers — the number of liters of water that Alice, Bob, and Carol collect over $m$ days.
## 思路
先把问题从按天模拟改成按事件计数

第 $d$ 天谁会来只由整除关系决定
- Alice 来当且仅当 $a \mid d$
- Bob 来当且仅当 $b \mid d$
- Carol 来当且仅当 $c \mid d$

因此前 $m$ 天内某类事件出现次数都可写成 $\left\lfloor \frac{m}{x} \right\rfloor$

设
$$
A=\left\lfloor \frac{m}{a} \right\rfloor,\quad
B=\left\lfloor \frac{m}{b} \right\rfloor,\quad
C=\left\lfloor \frac{m}{c} \right\rfloor
$$

$$
AB=\left\lfloor \frac{m}{\mathrm{lcm}(a,b)} \right\rfloor,\quad
AC=\left\lfloor \frac{m}{\mathrm{lcm}(a,c)} \right\rfloor,\quad
BC=\left\lfloor \frac{m}{\mathrm{lcm}(b,c)} \right\rfloor
$$

$$
ABC=\left\lfloor \frac{m}{\mathrm{lcm}(a,b,c)} \right\rfloor
$$

这样设的原因是一个人的收益不仅取决于自己来了多少天 还取决于和别人重合了多少天

以 Alice 为例做容斥修正
- 先按每次来都拿 $6$ 计入 得到 $6A$
- 与 Bob 同天时每次应从 $6$ 变 $3$ 需减去 $3AB$
- 与 Carol 同天时同理 需减去 $3AC$
- 三人同天在前两步被重复扣减 实际应拿 $2$ 只需比 $6$ 少 $4$ 因此前面多减了 $2$ 需加回 $2ABC$

得到
$$
W_A = 6A - 3AB - 3AC + 2ABC
$$

同理
$$
W_B = 6B - 3AB - 3BC + 2ABC
$$

$$
W_C = 6C - 3AC - 3BC + 2ABC
$$
## 代码
```c++
#include <bits/stdc++.h>

using namespace std;

int main() {
    int t;
    cin >> t;
    while (t --) {
        long long a, b, c, m;
        cin >> a >> b >> c >> m;

        long long na = m/a, nb = m/b, nc = m/c;
        long long nab = m/lcm(a,b), nbc = m/lcm(b,c), nac = m/lcm(a,c), nabc = m/lcm(lcm(a, b), c);
        long long wa = 6*na - 3*nab - 3*nac + 2*nabc, 
        wb = 6*nb - 3*nab - 3*nbc + 2*nabc, 
        wc = 6*nc - 3*nac - 3*nbc + 2*nabc;

        cout << wa << " " << wb << " " << wc << endl;
    }
}
```
