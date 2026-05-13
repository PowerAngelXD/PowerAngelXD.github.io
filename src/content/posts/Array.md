# Array
## 题面
You are given an integer array $a$ of length $n$.

For each index $i$, find **the maximum number** of indices $j$ such that $j \gt i$ and $|a_i - k| \gt |a_j - k|$, over all possible integer values of $k$.

**Input**

Each test contains multiple test cases. The first line contains the number of test cases $t$ ($1 \le t \le 100$). The description of the test cases follows.

The first line of each test case contains an integer $n$ ($1 \le n \le 5000$).

The second line contains $n$ integers $a_1, a_2, \ldots, a_n$ ($-10^9 \le a_i \le 10^9$).

It is guaranteed that the sum of $n$ over all test cases does not exceed $5000$.

**Output**

For each test case, output $n$ integers denoting the answer.
## 思路
对固定的位置 $i$，我们可以任选整数 $k$，并统计有多少个 $j\gt i$ 使得 $|a_i-k|\gt|a_j-k|$，目标是在所有 $k$ 中让这个数量最大化\
\
核心观察是把 $a_j$ 与 $a_i$ 的大小关系分开考虑\
当 $a_j\gt a_i$ 时，比较 $|a_i-k|$ 与 $|a_j-k|$ 可推出成立条件等价于 $k\gt\dfrac{a_i+a_j}{2}$，因此这类 $j$ 只有在 $k$ 取在 $a_i$ 右侧时才可能被计入\
当 $a_j\lt a_i$ 时，同理可推出成立条件等价于 $k\lt\dfrac{a_i+a_j}{2}$，因此这类 $j$ 只有在 $k$ 取在 $a_i$ 左侧时才可能被计入\
当 $a_j=a_i$ 时严格不等式永远不成立，因此不影响答案\
\
因为同一个 $k$ 不可能同时满足 $k\gt a_i$ 与 $k\lt a_i$，所以对某个固定的 $i$ 来说，能被计入的 $j$ 必然只来自后缀中比 $a_i$ 大的一侧或比 $a_i$ 小的一侧\
因此答案就是两侧计数的最大值\
$ans_i=\max\Bigl(\#\{j\mid j\gt i,\ a_j\gt a_i\},\ \#\{j\mid j\gt i,\ a_j\lt a_i\}\Bigr)$\
\
这个上界可以通过极端取值的 $k$ 直接达到\
取足够大的 $k$ 会让所有满足 $a_j\gt a_i$ 的位置都满足 $|a_i-k|\gt|a_j-k|$，得到第一项\
取足够小的 $k$ 会让所有满足 $a_j\lt a_i$ 的位置都满足 $|a_i-k|\gt|a_j-k|$，得到第二项

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
        vector<int> a(n);
        for (int i = 0; i < n; i ++) {
            cin >> a[i];
        }

        vector<int> ans(n);
        for (int i = 0; i < n; i ++) {
            int less = 0, greater = 0;
            for (int j = i + 1; j < n; j ++) {
                if (a[j] < a[i]) less ++;
                else if (a[j] > a[i]) greater ++;
            }
            ans[i] = max(less, greater);
        }

        for (int i = 0; i < n; i ++) {
            cout << ans[i] << " \n"[i == n - 1];
        }
    }
}
```
# Array
## 题面
You are given an integer array $a$ of length $n$.

For each index $i$, find **the maximum number** of indices $j$ such that $j \gt i$ and $|a_i - k| \gt |a_j - k|$, over all possible integer values of $k$.

**Input**

Each test contains multiple test cases. The first line contains the number of test cases $t$ ($1 \le t \le 100$). The description of the test cases follows.

The first line of each test case contains an integer $n$ ($1 \le n \le 5000$).

The second line contains $n$ integers $a_1, a_2, \ldots, a_n$ ($-10^9 \le a_i \le 10^9$).

It is guaranteed that the sum of $n$ over all test cases does not exceed $5000$.

**Output**

For each test case, output $n$ integers denoting the answer.
## 思路
对固定的位置 $i$，我们可以任选整数 $k$，并统计有多少个 $j\gt i$ 使得 $|a_i-k|\gt|a_j-k|$，目标是在所有 $k$ 中让这个数量最大化\
\
核心观察是把 $a_j$ 与 $a_i$ 的大小关系分开考虑\
当 $a_j\gt a_i$ 时，比较 $|a_i-k|$ 与 $|a_j-k|$ 可推出成立条件等价于 $k\gt\dfrac{a_i+a_j}{2}$，因此这类 $j$ 只有在 $k$ 取在 $a_i$ 右侧时才可能被计入\
当 $a_j\lt a_i$ 时，同理可推出成立条件等价于 $k\lt\dfrac{a_i+a_j}{2}$，因此这类 $j$ 只有在 $k$ 取在 $a_i$ 左侧时才可能被计入\
当 $a_j=a_i$ 时严格不等式永远不成立，因此不影响答案\
\
因为同一个 $k$ 不可能同时满足 $k\gt a_i$ 与 $k\lt a_i$，所以对某个固定的 $i$ 来说，能被计入的 $j$ 必然只来自后缀中比 $a_i$ 大的一侧或比 $a_i$ 小的一侧\
因此答案就是两侧计数的最大值\
$ans_i=\max\Bigl(\#\{j\mid j\gt i,\ a_j\gt a_i\},\ \#\{j\mid j\gt i,\ a_j\lt a_i\}\Bigr)$\
\
这个上界可以通过极端取值的 $k$ 直接达到\
取足够大的 $k$ 会让所有满足 $a_j\gt a_i$ 的位置都满足 $|a_i-k|\gt|a_j-k|$，得到第一项\
取足够小的 $k$ 会让所有满足 $a_j\lt a_i$ 的位置都满足 $|a_i-k|\gt|a_j-k|$，得到第二项

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
        vector<int> a(n);
        for (int i = 0; i < n; i ++) {
            cin >> a[i];
        }

        vector<int> ans(n);
        for (int i = 0; i < n; i ++) {
            int less = 0, greater = 0;
            for (int j = i + 1; j < n; j ++) {
                if (a[j] < a[i]) less ++;
                else if (a[j] > a[i]) greater ++;
            }
            ans[i] = max(less, greater);
        }

        for (int i = 0; i < n; i ++) {
            cout << ans[i] << " \n"[i == n - 1];
        }
    }
}
```
