# ABC 465 D - X to Y

[题目链接](https://atcoder.jp/contests/abc465/tasks/abc465_d)

### 题意简述

初始时有一个非负整数 $x=X$ ，每次操作可将 $x$ 变为 $y$ ，其中 $y$ 满足 $\left \lfloor \dfrac{x}{K} \right \rfloor = y$ 或 $\left \lfloor \dfrac{y}{K} \right \rfloor = x$．

求将 $x$ 变为 $Y$ 的最少操作次数（可以不操作）．

多测，$1\le T\le 2\times 10^5$，$0\le X,Y\le 10^{18}$，$2\le K\le 10^{18}$．


### 解析

构造一棵以 $0$ 为根节点的树，对于树上任意正整数节点 $u$ ，其父节点 $\mathrm{Fa}(u)=\left \lfloor \dfrac{u}{K} \right \rfloor$．

则对 $x$ 进行一次操作相当于将节点 $x$ 移动到其相邻节点 $y$ ，最少操作次数即为节点 $X$ 和 $Y$ 的最短路径 $=\mathrm{Dist}(X,\mathrm{LCA})+\mathrm{Dist}(Y,\mathrm{LCA})$．

容易看出对于任意节点 $u>v$ ，$\mathrm{Depth}(u)\ge\mathrm{Depth}(v)$ ，故只需要每次将数值较大的节点上移直至两点相遇（即到达 $\mathrm{LCA}$），移动次数即为最短路径．


### 参考代码

```cpp
int cnt = 0;
while (x != y) {
    if (x > y) x /= k;
    else y /= k;
    cnt++;
}
```

