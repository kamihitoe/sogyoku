# 洛谷 P17621 [Qaswed12 OI R1] Xor Is Mex

[题目链接](https://www.luogu.com.cn/problem/P17621)


### 第一问

容易看出 $t=1$ 时无解，$t=2$ 时可构造 $a=[1,1]$．

对于 $t\ge3$，可构造 $a=[2,3,0,\dots,0]$，此时 $\bigoplus a_i = \operatorname{mex}(a) = 1$．



### 第二问

需要用到一个经典结论：

记 $F(n) = 1 \oplus 2 \oplus \cdots \oplus n$，则
$$
F(n) = \begin{cases}
n & n \bmod 4 = 0\\
1 & n \bmod 4 = 1\\
n+1 & n \bmod 4 = 2\\
0 & n \bmod 4 = 3
\end{cases}
$$
证明见[此处](/note/tricks/#1n-%e8%bf%9e%e7%bb%ad%e5%bc%82%e6%88%96%e5%92%8c)．

由该结论，可先在序列 $A$ 中放 $t$ 个数：$0, 1, 2, \dots, t-1$，此时 $\operatorname{mex}(A) = t$，$\bigoplus A_i=F(t-1)$，只需再补若干个 $\ne t$ 且异或和 $=t \oplus F(t-1)$ 的数即可．

下面按 $t \bmod 4$ 分类讨论：

| $t \bmod 4$ | $ F(t-1) $ | $ t \oplus F(t-1)$ | 补数         |
| ----------- | -------- | --------------------- | ------------ |
| $ 0 $         | $ 0 $      | $ t $                   | $ 1 $ 和 $ t+1 $ |
| $ 1 $         | $ t-1 $    | $ 1 $                   | $ 1 $          |
| $ 2 $         | $ 1 $      | $t+1$    | $ t+1 $        |
| $ 3 $         | $ t $      | $ 0 $                   | 无        |

特别地，当 $t=1$ 时补数 $=1$，该构造不可行，可特殊构造为 $A=[0,2,3]$；

当 $t=10^6$ 时，$|A|=10^6+2$，不满足 $|A|<10^6$，需特判为无解（此时不存在其它合法构造，因为要使 $\operatorname{mex}(A) = 10^6$ 至少要有 $0, 1, 2, \dots, 10^6-1$ 共 $10^6$ 个数）．


