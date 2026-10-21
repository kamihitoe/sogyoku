# Tricks

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

{{< details title="证明" closed="true" >}}

对任意非负整数 $k$，有 $4k \oplus (4k+1) \oplus (4k+2) \oplus (4k+3) = 0$．

因为这四个数的二进制除最低两位外均相同，高位异或后为 $0$；而最低两位是 `00`、`01`、`10`、`11`，异或后也是 $0$．

设 $n = 4q + r$， $0 \le r \le 3$，则 $S(n) = (4q) \oplus (4q+1) \oplus \cdots \oplus (4q+r)$，分别讨论 $r=0,1,2,3$ 四种情况即可．

{{< /details >}}


### 判断一个数是否为 2 的幂次

```cpp
bool isPowerOfTwo(int x) {
    return x > 0 && (x & -x) == x;
}
```


### 判断四点能否构成正方形

条件：四条边等长且 $>0$ 、对角线等长

```cpp
struct Point {
    ll x, y;
};
bool isSquare(vector<Point>& points) {
    vector<ll> d;
    for (int i = 0; i < 4; ++i) {
        for (int j = i + 1; j < 4; ++j) {
            ll dx = points[i].x - points[j].x;
            ll dy = points[i].y - points[j].y;
            d.push_back(dx * dx + dy * dy);
        }
    }
    sort(d.begin(), d.end());
    return d[0] > 0 &&
    d[0] == d[1] && d[1] == d[2] && d[2] == d[3] &&
    d[4] == d[5];
}
```


### 使用 multiset 模拟双端优先队列

```cpp
multiset<ll> st;
st.insert(x);                 // 插入元素
ll min_value = *st.begin();   // 最小值
ll max_value = *st.rbegin();  // 最大值
st.erase(st.begin());         // 删除最小值
st.erase(prev(st.end()));     // 删除最大值
```



