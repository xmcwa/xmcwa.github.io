---
title: 数论笔记
description: 
slug: number-theory
date: 2023-09-17
image: cover.jpg
categories:
    - programming
    - algorithm
tags:
    - Number Theory
    - Math
---

## 知识点

### 数论函数

数论函数是定义域为正整数的函数。

### 积性函数

称函数f为积性函数，当且仅当 $\\forall x,y\\in\\mathbb N^*,x~\\bot~ y$ 都有 $f(xy)=f(x)f(y)$。

称函数f为完全积性函数，当且仅当 $\\forall x,y\\in\\mathbb N^*$ 都有 $f(xy)=f(x)f(y)$。

显然，若有积性函数 $f$，则 $f(1)=1$ 一定成立。

#### 例子

+ 单位函数 $\\varepsilon(n)=[n=1]$
+ 恒等函数 $id_k(n)=n^k$，$id_1(n)$ 简记为 $id(n)$
+ 常数函数 $1(n)=1$
+ 除数函数 $\\sigma_k(n)=\\sum_{d\\mid n}d^k$，$\\sigma_0(n)$ 简记为 $d(n)$，$\\sigma_1(n)$ 简记为 $\\sigma(n)$
+ 欧拉函数 $\\varphi(n)=\\sum_{i=1}^n[gcd(i,n)=1]$
+ 莫比乌斯函数 $\\mu(n)=\\begin{cases}1&n=1\\\\0&\\exists d>1,d^2\\mid n\\\\(-1)^{\\omega(n)}&\\text{otherwise}\\end{cases}$

### Dirichlet 卷积

数论函数 $f$ 与 $g$ 的 Dirichlet 卷积定义为：
$$h(n)=\\sum_{d\\mid n}f(d)g(\\frac nd)$$
记作 $h=f\\ast g$

#### 性质

交换律：$f\\ast g=g\\ast f$，显然。

结合律：$(f\\ast g)\\ast h=f\\ast(g\\ast h)$

证明：
$$
\\begin{aligned}
&((f\\ast g)\\ast h)(n)\\\\
=&\\sum_{td_3=n}\\left(\\sum_{d_1d_2=t}f(d_1)g(d_2)\\right)h(d_3)\\\\
=&\\sum_{d_1d_2d_3=n}f(d_1)g(d_2)h(d_3)\\\\
=&\\sum_{d_1t=n}f(d_1)\\left(\\sum_{d_1d_2=t}g(d_2)h(d_3)\\right)\\\\
=&(f\\ast(g\\ast h))(n)
\\end{aligned}
$$

单位元：$\\varepsilon$ 是 Dirichlet 卷积的单位元，即 $\\forall f,f\\ast\\varepsilon=\\varepsilon\\ast f=f$。

逆元：$\\forall f\\not\\equiv0$，都存在函数 $f^{-1}$ 使得 $f\\ast f^{-1}=\\varepsilon$，构造如下：

$$
f(n)=\\begin{cases}
\\displaystyle\\frac{1}{f(1)}&n=1\\\\\\\\
\\displaystyle\\frac{-1}{f(1)}\\sum_{d\\mid n\\text{且}d<n}f(\\frac nd)f^{-1}(d)&n>1
\\end{cases}
$$

### 莫比乌斯反演

莫比乌斯函数满足等式 $\\sum_{d\\mid n}\\mu(d)=[n=1]$，设 $n$ 有 $k$ 个本质不同的质因子，证明如下：
$$
\\sum_{d\\mid n}\\mu(d)=\\sum_{i=0}^k\\binom ki\\cdot(-1)^i=[k=0]
$$

实际上，还有如下反演原理：
$$
f(n)=\\sum_{d\\mid n}g(d)\\Leftrightarrow g(n)=\\sum_{d\\mid n}f(d)\\mu(\\frac nd)
$$
证明：
$$
\\begin{aligned}
\\sum_{d\\mid n}f(d)\\mu(\\frac nd)&=\\sum_{d\\mid n}\\sum_{i\\mid d}g(i)\\mu(\\frac nd)\\\\
&=\\sum_{i\\mid n}g(i)\\sum_{d\\mid\\frac ni}\\mu(\\frac n{id})\\\\
&=\\sum_{i\\mid n}g(i)[n/i=1]\\\\
&=g(n)
\\end{aligned}
$$

#### $\\varphi$ 函数

对于一个正整数 $n$，我们考虑 $[0,1)$ 之内所有分母为 $n$ 的有理数，以 $n=12$ 为例：
$$
\\frac {0}{12},\\frac {1}{12},\\frac {2}{12},\\frac {3}{12},\\frac {4}{12},\\frac {5}{12},
\\frac {6}{12},\\frac {7}{12},\\frac {8}{12},\\frac {9}{12},\\frac {10}{12},\\frac {11}{12}
$$
经化简得到：
$$
\\frac{0}{1},\\frac{1}{12},\\frac{1}{6},\\frac{1}{4},\\frac{1}{3},\\frac{5}{12},
\\frac{1}{2},\\frac{7}{12},\\frac{2}{3},\\frac{3}{4},\\frac{5}{6},\\frac{11}{12}
$$
我们根据分母将以上分数分组：
$$
\\frac{0}{1};~\\frac{1}{2};~\\frac{1}{3},\\frac{2}{3};~\\frac{1}{4},\\frac{3}{4};~
\\frac{1}{6},\\frac{1}{5};~\\frac{1}{12},\\frac{5}{12},\\frac{7}{12},\\frac{11}{12};
$$
我们发现，对于每个 $d\\mid n$，$d$ 都出现在了分母上，并且对于分母为 $d$ 的一组，全部 $\\varphi(d)$ 个与 $d$ 互质的数都在分子上出现。从而
$$
\\sum_{d\\mid n}\\varphi(d)=n
$$
即 $\\varphi\\ast1=id$。

### 杜教筛

杜教筛可以用来求解积性函数前缀和问题，我们考虑如下式子：
$$
\\begin{aligned}
\\sum_{i=1}^n(f\\ast g)(i)&=\\sum_{i=1}^n\\sum_{d\\mid i}g(d)f(\\frac id)\\\\
&=\\sum_{d=1}^n\\sum_{i=1}^{\\lfloor\\frac nd\\rfloor}g(d)f(i)\\\\
&=\\sum_{d=1}^ng(d)S(\\lfloor\\frac nd\\rfloor)
\\end{aligned}
$$
因此不难得到 $g(1)S(n)=\\sum_{i=1}^n(f\\ast g)(i)-\\sum_{i=2}^ng(i)S(\\lfloor\\frac ni\\rfloor)$。如果 $g$ 的前缀和和 $f\\ast g$ 前缀和都可以快速得到，那么我们可以使用整除分块计算后面一部分。

#### 时间复杂度

设计算 $S(n)$ 的复杂度为 $T(n)$，则
$$
\\begin{aligned}
T(n)&=\\sum_{i=1}^nT(\\lfloor\\frac ni\\rfloor)\\\\
T(n)&=O(\\sqrt n)+\\sum_{i=2}^{\\sqrt n}T(\\lfloor\\frac ni\\rfloor)\\\\
T(\\lfloor\\frac ni\\rfloor)&=O(\\sqrt \\frac ni)+\\sum_{j=2}^{\\sqrt \\frac ni}T(\\lfloor\\frac n{ij}\\rfloor)=O(\\sqrt \\frac ni)\\\\
T(n)&=O(\\sqrt n)+\\sum_{i=2}^{\\sqrt n}T(\\lfloor\\frac ni\\rfloor)=\\sum_{i=2}^{\\sqrt n}T(\\lfloor\\frac ni\\rfloor)\\\\&=O\\left(\\int_0^{\\sqrt n}\\sqrt\\frac nx\\mathrm dx\\right)=O(n^{\\frac 34})\\\\
\\end{aligned}
$$
进一步优化上述做法，可以考虑预先线性筛出一部分 $S(n)$，不难得知此时 $T(n)=O(k+\\frac n{\\sqrt k})$，取 $k=O(n^{\\frac 23})$ 可得最小值 $T(n)=O(n^{\\frac 23})$。

### PN 筛

类似于杜教筛，PN 筛也可以用来求一些积性函数的前缀和。

对于数 $n$，设 $n$ 的质因数分解为 $n=p_1^{\\alpha_1}p_2^{\\alpha_2}\\cdots p_k^{\\alpha_k}$，若 $\\forall i,\\alpha_i>1$，则 $n$ 为 PN。不难发现，所有 PN 都可以被表示成 $a^2b^3$ 的形式，具体的，质因数分解后，把所有幂次是奇数的分给 $b^3$ 一个，剩下全都分给 $a^2$。

现在我们考虑 PN 的个数，我们枚举所有的 PN，设 $n=a^2b^3$，先枚举 $a$，则 PN 的个数为
$$
\\sum_{a=1}^{\\sqrt n}\\sqrt[3]{\\frac n{a^2}}=O\\left(\\int_1^{\\sqrt n}\\sqrt[3]{\\frac n{x^2}}\\mathrm dx\\right)=O\\left(x^{\\frac 13}n^{\\frac 13}\\bigg|_{0}^{\\sqrt n}\\right)=O(\\sqrt n)
$$

对于我们要求前缀和的函数 $f$，构造积性函数 $g$ 使得对于所有质数 $p$，有 $f(p)=g(p)$，然后构造 $h$ 满足 $f=g\\ast h$。由 Dirichlet 卷积定义可知，$f(p)=g(1)h(p)+g(p)h(1)$，$f(p)=g(p)+h(p)$，因此 $h(p)=0$，因此 $h$ 只在 $1$ 和 PN 处有有效值，下面我们对 $\\sum f$ 做如下变换。
$$
\\begin{aligned}
\\sum_{i=1}^nf(i)&=\\sum_{i=1}^n\\sum_{d\\mid i}h(d)g(\\frac id)\\\\
&=\\sum_{d=1}^n\\sum_{i=1}^{\\lfloor\\frac nd\\rfloor}h(d)g(i)\\\\
&=\\sum_{d=1}^nh(d)G(\\lfloor\\frac nd\\rfloor)\\\\
&=\\sum_{\\stackrel{d=1}{d\\text{ is PN}}}^nh(d)G(\\lfloor\\frac nd\\rfloor)
\\end{aligned}
$$
因为 $h$ 最多仅有 $O(\\sqrt n)$ 个有效取值，因此可以搜索所有 PN，计算 $h(d)$ 的值。

## 例题

### 【模板】杜教筛

#### 题意

求 $\\left(\\sum_{i=1}^n\\varphi(i)\\right)\\bmod 998244353, n\\le10^{10}$。

#### 解法

考虑杜教筛，构造 $g=1$，则 $f\\ast g=id$，$S(n)=\\frac 12n(n+1)-\\sum_{i=2}^nS(n)$

### GCD Extreme (hard)

#### 题意

求 $\\sum_{i=1}^n\\sum_{j=i+1}^n \\gcd(i,j),n\\le2\\times 10^{11}$

#### 解法

常见套路
$$
\\begin{aligned}
\\sum_{i=1}^n\\sum_{j=i+1}^n \\gcd(i,j)
&=\\sum_{i=1}^n\\sum_{j=i+1}^n\\sum_{d\\mid (i,j)}d[gcd(i,j)=d]\\\\
&=\\sum_{d=1}^nd\\sum_{d\\mid i}^n\\sum_{j=i+1,d\\mid j}^n[gcd(i,j)=d]\\\\
&=\\sum_{d=1}^nd\\sum_{i=1}^{\\lfloor\\frac nd\\rfloor}\\sum_{j=1}^{i-1}[gcd(i,j)=1]\\\\
&=\\sum_{d=1}^nd\\sum_{i=1}^{\\lfloor\\frac nd\\rfloor}\\varphi(i)\\\\
\\end{aligned}
$$

后面的下次更新