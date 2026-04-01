---
title: Reverse-a-Permutation
published: 2026-04-01
description: 'CodeFore每日一题题解'
image: ''
tags: [CF题解, 算法, 每日一题]
category: '算法'
draft: false 
lang: ''
---
## 题面
A permutation of length $n$ is an array consisting of $n$ distinct integers from $1$ to $n$ in arbitrary order. For example, $[2,3,1,5,4]$ is a permutation, but $[1,2,2]$ and $[1,3,4]$ are not permutations.

You are given a permutation $p$ of length $n$. You can perform the following operation **exactly once**:

-   Choose two integers $l,$ $r$ ($1\le l\le r\le n$).
-   Reverse the segment $[l, r]$ in the permutation $p$.

Your task is to output the lexicographically maximum permutation that can be obtained by performing this operation. A permutation $a$ is lexicographically greater than a permutation $b$ if for the first position $i$ where they differ, it holds that $a_i\gt b_i$.

**Input**

Each test consists of several test cases. The first line contains a single integer $t$ ($1\le t\le 10^4$) — the number of test cases. The description of the test cases follows.

The first line of each test case contains the number $n$ ($1\le n\le 2\cdot 10^5$).

The second line of each test case contains $n$ distinct integers $p_1, p_2,...,p_n$ $(1\le p_i\le n)$.

It is guaranteed that the sum of the values of $n$ across all test cases does not exceed $2\cdot 10^5$.

**Output**

For each test case, output the lexicographically maximum permutation that can be obtained with one operation.
## 思路
按照题意，我们需要确定一个区间 $[l, r]$ 用于反转，并保证之后的串的字典序是最大的，也就是保证这个排列目前处于“完美状态”

> [!NOTE]
> **这个“完美状态”如何理解？**
> 
> 给出例子：$[4, 3, 1, 2]$
>
> - 遍历到第 $1$ 位：对应值为 $4$，对于长度为 $4$ 的排列来说，不可能有比 $4$ 还大的值了，所以目前的排列处于完美状态
>
> - 遍历到第 $2$ 位：对应值为 $3$，对于长度为 $4$ 的排列来说，第二位理论上的字典序最大值就是 $n - 1$ 也就是这里的 $3$ (这里用 $n$ 指代排列长度)
>
> - 遍历到第 $3$ 位：对应值为 $1$，但是在一个这个排列中，有比这个值更大的数字存在，所以这个位置上的数字不是完美状态
>
> 以上，就是对一个排列的字典序的所谓“完美状态”的理解
>
> 本质上，就是看在给定的元素组成的所有有可能的排列中，字典序最大的那一个，对应位置的各个元素也就被称为所谓的“完美状态”

由上，不难看出，对于这个应该反转的起点 $l$ ，其实就是第一个破坏整个排列完美状态的位置

而对应的 $r$，就是本应该在 $l$ 位置的数的位置，在上面的例子中，$r = 4$

> [!NOTE]
> 推广例子：$[5, 3, 1, 2, 4]$
> 
> 很明显，按照上面的推理，位置 $2$ 的 $3$ 并不完美，所以 $l = 2$
>
> 接下来，我们就要查找，在位置 $2$ 上，到底应该适合什么数字？
>
> 计算，很明显，位置 $2$ 上的数字应该是 $n - l + 1$
> > **推导**
> >
> > 位置 $1$ 上的数字应该是 $n - 1 + 1 = n$
> >
> > 位置 $2$ 上的数字应该是 $n - 2 + 1 = n - 1$
> >
> > 位置 $3$ 上的数字应该是 $n - 3 + 1 = n - 2$
> >
> > $\Rightarrow$ 位置 $i$ 上的数字应该是 $n - i + 1$

对于上述问题给出的例子来说，这里的 $l$ 就是 $3$；而由上面的推导，不难看出， $r$ 就是满足 $p[i] = n - l + 1$ 的位置

所以，对于刚刚的例子，这个应该反转的区间就是 $[3, 4]$，反转之后得到的排列，正是字典序最大的（$[4, 3, 2, 1]$）

因此在代码实现上，我们只需要对这个区间左端点 $l$ 以前的排列正常输出，而在区间内的元素进行倒序输出即可
## 代码
```c++
#include <bits/stdc++.h>

using namespace std;

int main() {
	int t;
	cin >> t;

	while(t --)  {
		int n;
		cin >> n;

		int p[n + 5], l = 1;

		for(int i = 1; i <= n; i ++) {
			cin >> p[i];
		}

		while(l <= n && p[l] == n - l + 1) l ++;
		int r = -1;

		for(int i = l; i <= n; i ++) {
			if(p[i] == n - l + 1) r = i;
		}

		for(int i = 1; i < l; i ++ ) cout << p[i] << ' ';

		if(r != -1) {
			for( int i = r; i >= l; i --) cout << p[i] << ' ';
			for( int i = r + 1; i <= n; i ++) cout << p[i] << ' ';
		}
		cout << '\n';
	}
}
```