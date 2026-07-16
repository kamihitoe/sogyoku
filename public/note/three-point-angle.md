# 三点最大夹角


### 题意简述

在给定的 $N$ 个互异的点中选出三个点 $P_i, P_j, P_k$，使得 $\angle P_i P_j P_k$ 最大。

$3 \leq N \leq 2000$ ，$0^\circ \leq \angle P_i P_j P_k \leq 180^{\circ}$。



### 思路

#### 朴素解法

枚举所有三点组合 $(P_i, P_j, P_k)$ ，时间复杂度 $O(N^3)$ 。

#### 优化思路

枚举顶点 $P_j$ ，计算其余的点相对于 $P_j$ 的极角，寻找极角之差最接近且不大于 $180^\circ$ 的点对 $(P_i,P_k)$。

具体步骤：外层循环枚举 `j` ，内层循环通过 `atan2(dy, dx)` 计算极角并排序，用双指针寻找 $\le180^\circ$ 的最大差值，时间复杂度 $O(N^2 \log N)$。

细节上，处理跨越 $-\pi$ / $\pi$ 边界的夹角时，可以通过

<a id="method-1"></a>
1. 常见的环形问题处理方法：将原数组的数据加上 $2\pi$ 复制一遍拼接在数组尾部（“破环成链”）
<a id="method-2"></a>
2. 补充用 `2 * PI - (angles[r] - angles[l])` 更新答案，其中 `(angles[r] - angles[l])` 为双指针找到的 $>180^\circ$ 的最小差值（注意检查下标越界）



### 代码实现

基于 [2.](#method-2) 的实现：

```cpp
const double PI = acos(-1);
void solve() {
    ll n;
    cin >> n;
    vector<ll> x(n), y(n);
    for (ll i = 0; i < n; i++) cin >> x[i] >> y[i];
    double ans = 0.0;
    for (ll j = 0; j < n; j++) {
        vector<double> angles;
        for (ll i = 0; i < n; i++) {
            if (i == j) continue;
            double dx = x[i] - x[j], dy = y[i] - y[j];
            angles.push_back(atan2(dy, dx));
        }
        sort(angles.begin(), angles.end());
        ll m = angles.size();
        ll r = 0;
        for (ll l = 0; l < m; l++) {
            while (r < m && angles[r] - angles[l] <= PI) r++;
            ans = max(ans, angles[r - 1] - angles[l]);
            if (r < m) ans = max(ans, 2 * PI - (angles[r] - angles[l]));
        }
    }
    ans = ans * 180 / PI;
    cout << ans << '\n';
}
```

基于 [1.](#method-1) 的实现对上述代码 `L16-L22` 做如下修改即可：

```cpp {linenostart=16,hl_lines=[2,3,4,7,9]}
        ll m = angles.size();
        for (ll i = 0; i < m; i++) {
            angles.push_back(angles[i] + 2 * PI);
        }
        ll r = 0;
        for (ll l = 0; l < m; l++) {
            while (angles[r] - angles[l] <= PI) r++;
            ans = max(ans, angles[r - 1] - angles[l]);
        }
```


