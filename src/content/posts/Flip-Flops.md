---
title: Flip-Flops
published: 2026-05-12
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法, 每日一题]
category: '算法'
draft: false 
lang: ''
---
## 题面
OtterZ set up a battle with $n$ monsters in order to increase his combat power. Each monster has a combat power $a_i$ and OtterZ has a combat power of $c$. He has $k$ flip flops and can perform the following operations:

1.  Kill an alive monster $i$ if $a_i \le c$; then $c$ becomes $c + a_i$.
2.  Throw a flip flop at an alive monster $i$; the flip-flop will be broken and the monster will become angrier, then $a_i$ becomes $a_i + 1$.

Help OtterZ obtain the maximum possible $c$ after the battle.

**Input**

Each test contains multiple test cases. The first line contains the number of test cases $t$ ($1 \le t \le 500$). The description of the test cases follows.

The first line of each test case contains three integers $n$, $c$ and $k$ ($1 \le n \le 100$, $0 \le c,k \le 10 ^ 9$).

The second line contains $n$ integers $a_1,a_2,\ldots,a_n$ ($0\le a_i\le 10 ^ 9$).

**Output**

For each test case, output an integer — the maximum possible combat power.
## 思路
先观察两种操作对战力 $c$ 的影响，击杀会让 $c:=c+a_i$ 从而单调不减，而扔拖鞋只会让某个 $a_i:=a_i+1$ 变大并不会直接改变 $c$，因此拖鞋的价值只可能体现在让被击杀的怪物贡献更大 \
\
注意拖鞋会让怪物更强，因此它永远不可能帮助你更早击杀某个怪物，只会让击杀条件 $a_i\le c$ 更难满足，所以不会存在通过拖鞋解锁击杀的情况 \
\
设当前战力为 $c$，准备击杀的怪物当前战力为 $a$，若先扔 $x$ 次拖鞋则它会变为 $a+x$，要保证还能立刻击杀必须满足 $a+x\le c$，即 $x\le c-a$ \
\
并且在满足可击杀的前提下，每多扔 $1$ 次拖鞋，击杀后得到的增量就多 $1$，因为击杀收益从 $a$ 变为 $a+1$，所以对这只怪物的最优选择一定是尽量多扔 \
\
因此若决定现在杀这只怪物，最优扔法为 $x=\min\left(k,\;c-a\right)$，随后更新 $k:=k-x$，并更新 $c:=c+(a+x)$ \
\
由此得到贪心，先将数组 $a$ 升序排序并从小到大扫描，若当前 $a_i>c$ 则直接停止，否则对该怪物计算 $x=\min\left(k,\;c-a_i\right)$，更新 $k,c$ 后继续处理下一个
> [!NOTE]
> 考虑击杀顺序，击杀只会增大 $c$，因此越早击杀越不会使后续变差，而拖鞋对未击杀怪物只会使其更难被击杀，因此我们只需要关心按某个顺序尽可能多地击杀怪物并在每次击杀前把拖鞋用到上限 
> 
>一个重要的停止条件是若当前所有存活怪物中最小的战力都满足 $a_{\min}>c$，则剩余任何怪物都无法击杀，因为其战力更大且拖鞋只会继续增大它们，所以此时答案就是当前的 $c$
>