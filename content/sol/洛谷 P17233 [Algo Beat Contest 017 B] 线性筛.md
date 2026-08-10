+++
date = '2026-08-10T21:45:52+08:00'
draft = false
title = '洛谷 P17233 [Algo Beat Contest 017 B] 线性筛'
description = ' '
tags = ["洛谷", "DP"]
+++

[题目链接](https://www.luogu.com.cn/problem/P17233)


### 题目分析

设 $dp_i$ 为初始下标为 $i$ 的元素被删除所需的操作次数， $cnt_i$ 为小于 $i$ 的完全立方数的个数．

因为每次操作时，元素的相对顺序和删除规则是固定的，因此一个元素下一次操作后的下标完全取决于当前的下标．

具体地，考虑当前下标为 $i$ 的元素：

- 若 $i$ 是完全立方数，则它在这一步就会被直接删除，还需要的操作次数为 $1$
- 若 $i$ 不是完全立方数，则在它前面的元素有 $cnt_i$ 个会被删除，下一步的下标变成 $i-cnt_i$ ，那么它被删除还需要的操作次数就等于初始下标为 $i-cnt_i$ 的元素被删除所需的操作次数

由此可得转移方程：

- 当 $i$ 是完全立方数时，$dp_i=1$
- 当 $i$ 不是完全立方数时，$dp_i=dp_{i-cnt_i}+1$



### 参考代码

```cpp
void solve() {
    int n;
    cin >> n;
    vector<int> a(n + 1), dp(n + 1);
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
    }
    int t = 0, cnt = 0;
    int k = 1, kkk = 1;
    for (int i = 1; i <= n; i++) {
        if (i == kkk) {
            dp[i] = 1;
            k++;
            kkk = k * k * k;
            cnt++;
        } else {
            dp[i] = dp[i - cnt] + 1;
        }
        if (dp[i] > t) {
            t = dp[i];
        }
    }
    vector<vector<int>> res(t + 1);
    for (int i = 1; i <= n; i++) {
        res[dp[i]].push_back(a[i]);
    }
    cout << t << '\n';
    for (int i = 1; i <= t; i++) {
        for (int x : res[i]) {
            cout << x << ' ';
        }
        cout << '\n';
    }
}
```
