+++
date = '2026-02-11T10:24:34+08:00'
draft = false
title = '并查集(DSU)'
+++

## 模板

```cpp
#include <vector>
#include <numeric>

struct DSU {
    vector<ll> f, sz;
    ll sets;  // 连通块个数

    DSU(ll n) : f(n+1), sz(n+1, 1), sets(n) {
        iota(f.begin(), f.end(), 0);
    }

    ll find(ll x) {
        while (x != f[x]) x = f[x] = f[f[x]];  // 路径减半
        return x;
    }
    void merge(ll x, ll y) {
        x = find(x), y = find(y);
        if (x == y) return;
        if (sz[x] < sz[y]) swap(x, y);  // 按规模合并
        f[y] = x;
        sz[x] += sz[y];
        sets--;
    }
};
```
