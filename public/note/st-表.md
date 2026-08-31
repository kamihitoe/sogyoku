# ST 表 (Sparse Table)
ST 表（Sparse Table，稀疏表）用于解决可重复贡献问题[^1]

[^1]: 即满足幂等律的区间查询问题，如区间最值、区间 GCD、区间按位与、区间按位或等

令 $f(i,j)$ 表示区间 $[i,i+2^j-1]$ 的最大值，则有：
$$
f(i,j)=\begin{cases}
a_i, & j = 0 \\
\max(f(i,j-1),f(i+2^{j-1},j-1)), & j > 0
\end{cases}
$$

对于每个询问 $[l,r]$，分成两部分求最大值：$[l,l+2^s-1]$ 与 $[r-2^s+1,r]$，其中 $s=\left\lfloor\log_2(r-l+1)\right\rfloor$．

