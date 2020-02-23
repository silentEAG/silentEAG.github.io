---
title: 矩阵优化DP类问题应用向小结
author: Silent_E
avatar: 'https://cdn.jsdelivr.net/gh/silenteag/cdn@1.3/img/avatar.jpg'
authorLink: https://silenteag.github.io
authorAbout: For the Life, For the World!
authorDesc: For the Life, For the World!
categories: 技术
comments: true
date: 2019-09-26 11:50:07
tags:
 - 矩阵
 - 动态规划
keywords:
description: 重点：应用向。
photos: https://s2.ax1x.com/2020/02/23/33iitS.jpg
---

## 前言

本篇强调**应用**，矩阵的基本知识有所省略。<span class="spoiler">也许会写篇基础向。。。</span>

## 思想及原理

为什么Oier们能够想到用矩阵来加速DP呢？做了一些DP题之后，我们会发现，有时候DP两两状态之间的转移是**定向**的，也就是说，在DP转移的所有阶段中，对于一个固定的状态$f_i$，它只能转移到一个不变的状态集合$\{F_i\}$中，我们转移的方向不会因为阶段的改变而改变。

好，提炼关键信息，我们需要状态的转移，且状态转移的方式不变（不排除某些毒瘤题），并且对于大多数转移，无非就是各个状态之间的带系数运算。当需要转移的次数过多并且状态数较小时，我们就可以考虑用矩阵快速幂来优化转移。

矩阵快速幂的原理及实现就当前置芝士了吧，略略略~

当然，如果你需要用矩阵来优化DP，首先你还是得先推出朴素的DP式子，然后观察DP式子各个状态之间的关系，然后构造一个符合状态转移的矩阵，然后就可以快速幂了。

## 常用套路

- **模板化**，比如用struct封装matrix；个人倾向于把构造的转移矩阵放左边XD。
- **猜想矩阵加速**，对着数据范围YY应该是每个oier都应有的能力。。。
- **自定义矩阵乘法**，我们需要知道对于特定的状态转移，只要它满足[广义矩阵乘法](https://baike.baidu.com/item/%E7%9F%A9%E9%98%B5%E4%B9%98%E6%B3%95/5446029?fr=aladdin)的基本性质，我们同样可以用矩阵来加速，下面有道例题就是这样。
- **预处理掉不合法的转移**，蒟蒻做的矩阵优化题有一半都与状压有关，，，那么我们就可以预处理出哪些状态本身就不合法，然后减少矩阵中的元素（一个矩阵就是$O(n^3)$啊）。
- **设辅助数组来构造转移矩阵**，CSP应该不会考到这种难度吧，，，反正知道有这种毒瘤题就对了。
- **对题目的特殊要求特殊处理**，其实也不算特殊处理吧，就是如何把题目中的所有条件都设计进矩阵里面，做到不重不漏。

想到我再补充。。。

## 例题

#### [牛继电器Cow Relays](https://www.luogu.org/problem/P2886)

给出一张无向连通图，求S到E经过k条边的最短路。

如果你对Floyd算法理解够深的话，这道题那就是很裸的矩阵优化了，但是需要我们重新定义矩阵乘法，而且必须证明符合广义矩阵乘法的所有性质。

#### [可乐](https://www.luogu.org/problem/P3758)

题意略。

有点难度。

难就难在我们如何合理地把题目中的条件设计进矩阵中。

#### [棋盘](https://www.luogu.org/problem/P3977)

题意略。

跟状压有关，减去冗余状态，然后矩阵优化转移。

#### [花园](https://www.luogu.org/problem/P1357)

题意略。

和上一题一样，但状态表示的难度大一点。

这道题做的时候想到了之前[动物园](https://www.luogu.org/problem/P3622)的做法，因为状态之间的转移是连续的，并且很短，自然想到状压，并且每个状态能够转移到的状态也是确定了的，所有想到矩阵加速转移。

所以首先预处理所有合法的转移情况，然后直接上矩阵快速幂，解决。

代码请参看我的[blog](https://www.cnblogs.com/silentEAG/p/11426625.html)。

#### [GXOI/GZOI2019 逼死强迫症](https://www.luogu.org/problem/P5303)

题目大意：在$2\times N$的方格中用$N-1$块$2\times 1$的方砖和$2$块$1\times 1$的方砖填充，且两块$1\times 1$的方块**不能有相邻的边**，求合法方案数。

不得不说，这题真的烦，需要推一大波式子。。。

这道题就需要我们考虑用辅助数组来帮助我们把状态转移设计进矩阵里面。



或许在[这里](https://www.cnblogs.com/silentEAG/p/10927635.html)阅读有更好的体验？



啊，一道计数问题，我开始是这样想的。

如果没有那两块很碍事的砖，我们很容易想到$f[i]=f[i-1]+f[i-2]$，递推走起。

好，现在来看那两块**碍事**的砖。

首先，我们会发现，这两块特别的砖会把整个方格分成三个部分，我们假设左右两部分刚好是完整的（即是个矩形），那么中间的块就有性质了。

仔细推一推就会发现，当这两个特殊的块间隔奇数个块时，这两个块必定在相异的两行，并且中间只有一种方案构成。

同样的，当这两个特殊的块间隔偶数个块时，这两个块必定在相同的一行，并且中间也只有一种方案构成。

又因为不能有相邻的边，于是计算公式就出来了。
$$
ans = 2 * \sum_{i=0}^{n-3}\sum_{j=0}^{i}f[j]*f[i-j]
$$
细细理解下。

用这个大概只能得$20pt$，我们想想怎么优化？看到$N\le 2e+9$的数据范围，当然要往矩阵快速幂上面想咯。

好，~~我不会了~~。

矩阵的推法各有不同吧，我们来一步一步来拆这个式子。

设$g(i)=\sum_{j=0}^{i}f(j)*f(i-j)$。

所以

$\begin{equation}
\begin{aligned}
g(i)&=\sum_{j=0}^{i}f(j)*f(i-j) \\
&=\sum_{j=0}^{i-2}f(j)*f(i-j)+f(i-1)*f(1)+f(i)*f(0)\\
&=\sum_{j=0}^{i-2}f(j)*[f(i-1-j)+f(i-2-j)]+f(i-1)+f(i)\\
&=g(i-2)+g(i-1)+f(i)
\end{aligned}
\end{equation}$

又设$sum(i)=\sum_{j=0}^{i}g(j)$

所以

$\begin{equation}
\begin{aligned}
sum(i)&=\sum_{j=0}^{i}g(j) \\
&=\sum_{j=0}^{i-1}g(j)+g(i) \\
&=sum(i-1)+g(i-2)+g(i-1)+f(i)
\end{aligned}
\end{equation}$

所以~~易推~~得矩阵转移方程：

$\begin{equation}{
\left[ \begin{array}{ccc}
1 & 1 & 0 & 0 & 0\\
1 & 0 & 0 & 0 & 0 \\
1 & 1 & 1 & 1 & 0 \\
0 & 0 & 1 & 0 & 0 \\
1 & 1 & 1 & 1 & 1 \\
\end{array} 
\right ]}\times {
\left[ \begin{array}{ccc}
f(i) \\
f(i-1) \\
g(i) \\
g(i-1) \\
sum(i) \\
\end{array} 
\right ]}={
\left[ \begin{array}{ccc}
f(i+1)\\
f(i)\\
g(i+1)\\
g(i)\\
sum(i+1)
\end{array}
\right ]}
\end{equation}$

于是$O(125logn)$可过。

注意下界处理。

代码：

```cpp
#include<ctime>
#include<queue>
#include<cstdio>
#include<cstdlib>
#include<cstring>
#include<iostream>
#include<algorithm>
#define ll long long
using namespace std;
const int MAX = 100000 + 5;
const int mod = 1e9 + 7;
inline int read(){
    int f = 1, x = 0;char ch;
    do { ch = getchar(); if (ch == '-') f = -1; } while (ch < '0'||ch>'9');
    do {x = x*10+ch-'0'; ch = getchar(); } while (ch >= '0' && ch <= '9'); 
    return f*x;
}

struct sakura {
    ll mar[5][5];
}A;

int t, n;

inline sakura mul(sakura A, sakura B) {
    sakura C;
    memset(C.mar, 0, sizeof (C.mar));
    for (int i = 0;i <= 4; ++i) {
    for (int k = 0;k <= 4; ++k) {       
        for (int j = 0;j <= 4; ++j) {
                C.mar[i][j] = (C.mar[i][j] + (A.mar[i][k] * B.mar[k][j]) % mod ) % mod;
            }
        }
    }
    return C;
}

inline sakura mi(sakura A, int c) {
    sakura B;
    B.mar[0][0] = 1, B.mar[1][0] = 1, B.mar[2][0] = 2, B.mar[3][0] = 1, B.mar[4][0] = 3;
    for (;c;c >>= 1) {
        if (c & 1)  B = mul(A, B);
        A = mul(A, A);
    }
    return B;
}
int main(){
    t = read();
    while (t--) {
        n = read();
        if (n < 3) {
            printf("0\n");
            continue;
        }
        if (n == 3) {
            printf("2\n");
            continue;
        }
        A.mar[0][0] = 1, A.mar[0][1] = 1, A.mar[0][2] = 0, A.mar[0][3] = 0, A.mar[0][4] = 0;
        A.mar[1][0] = 1, A.mar[1][1] = 0, A.mar[1][2] = 0, A.mar[1][3] = 0, A.mar[1][4] = 0;
        A.mar[2][0] = 1, A.mar[2][1] = 1, A.mar[2][2] = 1, A.mar[2][3] = 1, A.mar[2][4] = 0;
        A.mar[3][0] = 0, A.mar[3][1] = 0, A.mar[3][2] = 1, A.mar[3][3] = 0, A.mar[3][4] = 0;
        A.mar[4][0] = 1, A.mar[4][1] = 1, A.mar[4][2] = 1, A.mar[4][3] = 1, A.mar[4][4] = 1;
        sakura ans = mi(A, n - 4);
        ans.mar[4][0] <<= 1;
        ans.mar[4][0] %= mod;
        printf("%d\n", ans.mar[4][0]);
    }
    return 0;
}
```

## 扩展阅读

- [矩阵十大经典题目](https://blog.csdn.net/Ever_glow/article/details/75308250)，这里面的东西很多。
- [yyb的分类](https://www.cnblogs.com/cjyyb/category/1036557.html)，跟着神犇刷题总没错，只是有些题太难了啊。。。。
- [百度百科矩阵乘法](https://baike.baidu.com/item/%E7%9F%A9%E9%98%B5%E4%B9%98%E6%B3%95/5446029?fr=aladdin)，补充基础知识。
- [线性代数的几何实质](https://www.luogu.org/blog/Howershine950644/p-xian-xing-dai-shuo-di-ji-he-shi-zhi#)，众所周知，矩阵的可以看做是一个向量的集合，这篇文章涉及到了矩阵及其运算的几何意义，而且写得浅显易懂，有兴趣的可以去看看，加深对矩阵变换的理解。
- [线性代数的几何实质](https://www.bilibili.com/video/av6731067/?p=7)，上面的视频版。