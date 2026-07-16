+++
date = '2026-07-14T16:41:39+08:00'
draft = false
title = 'Tricks'
description = '一些技巧'
+++


### 子集和问题

计算数组内元素能够组合出的所有子集和．

```cpp
bitset<N> bs;
bs[0] = 1;
for (int i = 0; i < n; i++) {
    bs |= bs << a[i];
}
```

其中 `N` 为所有子集和的最大值，`bs[k]==1` 表示 `k` 是子集和之一．



### 1~n 连续异或和

设 $S(n) = 1 \oplus 2 \oplus 3 \oplus \cdots \oplus n$ ，则有：
$$
S(n) = \begin{cases}
n, & n \bmod 4 = 0 \\
1, & n \bmod 4 = 1 \\
n + 1, & n \bmod 4 = 2 \\
0, & n \bmod 4 = 3 
\end{cases}
$$

```cpp
ll xor_sum(ll n) {
    if (n % 4 == 0) return n;
    if (n % 4 == 1) return 1;
    if (n % 4 == 2) return n + 1;
    return 0;
}
```


