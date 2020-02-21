---
title: 期望DP小结
author: Silent_E
avatar: https://cdn.jsdelivr.net/gh/silenteag/cdn@1.3/img/avatar.jpg
authorLink: https://silenteag.github.io
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
categories: 技术
date: 2018-12-12 22:16:01
comments: true
tags: 

 - 动态规划
 - 概率问题
keywords: Sakura
description: 这是一篇测试稿
photos: https://cdn.jsdelivr.net/gh/silenteag/cdn@1.2/img/background.jpg
---

这是一篇测试稿！！！

---



> 喵帕斯~



## 资源分享

- [26 个比较概率大小的问题](http://www.matrix67.com/blog/archives/6665#more-6665)

- [数论小白都能看懂的数学期望讲解](https://45475.blog.luogu.org/mathematical-expectation)

## 概念

$PS$：不需要知道太多概念，能拿来用就行了。

- **定义**
  **样本**（$\omega$）：**一次**随机试验产生的**一个**结果。
  **样本空间**（$\Omega$）：**一个**随机试验的**所有**可能的结果的全体，即$\Omega=\{\omega\}$。
  **事件**（$A$）：某一类结果，即$A\subset\Omega$。
  **基本事件**（$s$）：各个互斥的事件即为基本事件。

我们借助**样本空间S**来定义概率。样本空间是**基本事件**的集合。

- **概率论公理**
  样本空间$S$的概率分布$Pr\{\}$是一个从$S$的事件到实数的映射，它满足以下公理：
  1. **非负性**：对于任意事件$A$，$Pr\{A\}\geqslant 0$。
  2. **正则性**：$Pr\{S\}=1$。
  3. **可列可加性**：对于两个互斥事件$A$与$B$，有$Pr\{A\cup B\}=Pr\{A\}+Pr\{B\}$。更一般地，对于任意有限或无限事件序列$A_1,A_2,...,$若其两两互斥，则有：

$$
Pr\{\bigcup_iA_i\}=\sum Pr\{A_i\}
$$

- **期望**

简单理解，期望的意义就是**概率**的**加权平均数**。

假设某随机试验$X$共有$n$种互斥的事件可能发生，其中第$i$个事件发生的概率为$P_i$，价值为$X_i$，则这个随机试验的期望是$E(X)=\sum P_iX_i$。

期望也可以从**频率**的角度来理解，我们知道如果不断重复某个随机试验，某个事件发生的频率会趋近于其概率，而将发生过所有事件的价值取平均值，这个值就会趋近于这个随机试验的期望。

## 期望的线性性质

$$
E(X+Y)=E(X)+E(Y)....(1)
$$

$$
E(aX)=aE(X)..............(2)
$$

$$
E(XY)=E(X)E(Y)..........(3)
$$

证明如下

**(1)式**:
$$
\begin{align}
E(X+Y) =& \sum_{i,j}(X_i+Y_j)P_iQ_j\\
=& \sum_{i, j} X_{i} P_{i} Q_{j}+\sum_{i, j} Y_{j} P_{i} Q_{j}\\
=& \sum_{i} X_{i} P_{i} \sum_{j} Q_{j}+\sum_{j} Y_{j} Q_{j} \sum_{i} P_{i}\\
=& \sum_{i} X_{i} P_{i}+\sum_{i} Y_{j} Q_{j}\\
=& E(X)+E(Y)
\end{align}
$$
**(2)式**:
$$
\begin{align}
E(aX) =& \sum_iaX_iP_i \\
=& a\sum_iX_iP_i \\
=& aE(X)
\end{align}
$$
**(3)式**:
$$
\begin{align}
E(XY)=& \sum_i\sum_jX_iY_jP_iQ_j \\
=&(\sum_iX_iP_i)(\sum_jY_jQ_j) \\
=&E(X)E(Y)
\end{align}
$$
用数学归纳法可推广到多个。

## 期望DP

### 定义

所求结果为某事件的期望的动态规划。

实际上这类动态规划并不是一个新的类型，它都是在原有的动态规划的基础上，将所求的值改成了**概率**和**期望**的相关值，换句话说，这类问题的难度其实还是在动态规划的原型上，概率和期望只是表象。

现在大多数期望题就是手动找公式或者$DP$推出即可，只要处理好边界，然后写好方程，就行了。与常规的求解不同，数学期望经常逆向推出，但不全是。它的代码量很短，但思维难度明显较高。

比如数学期望的$f[x]$一般表示到了$x$这一状态还差多少，最后答案是$f[0]$。

总之，该推公式的还是推，该在图上跑的还是在图上跑，只是注意状态的设计。

## 练手题目

### 入门：

#### [过河](https://www.luogu.org/problemnew/show/UVA12230)

少见的连续型随机变量题。

最坏情况下，过一条河需要 $3*L/v$ 的时间；最好的情况下，过一条河需要 $L/v$ 的时间，又因为船的位置随机，所以过河时间**线性分布**，于是期望取个平均值 $2*L/v$ 就行了。

#### [掷骰子](https://www.luogu.org/problemnew/show/SP1026)

考虑集合中选取一个不同于已选的新数的概率，所以可以设计$DP$方程：$f[i]$表示取了$i$种数后还需取的期望值。

取了$i$种数，当前取的是新数的概率是$\frac{n-i}{n}$，当前取的是集合中出现过的数的概率是$\frac{i}{n}$。所以可得到$DP$转移方程$f[i]=\frac{n-i}{n}f[i+1]+\frac{i}{n}f[i]+1$。

进一步化简，得$f[i]=f[i+1]+\frac{n}{n-i}$。

所以$f[0]=\sum_{i=1}^n\frac{n}{i}$。

#### [换教室](https://www.luogu.org/problem/P1850)

~~夭寿啦，NOIP考期望啦~~。

容易对每节课的选择划分阶段，根据期望的线性性质，可以相加得到总期望。

设$f[i][j][0/1]$表示前$i$节课提交了$j$次申请，当前是否申请时的期望最小体力值。那么只会从$i-1$转移过来。

方程见代码。（懒癌晚期。。。）

```cpp
#include<cstdio>
#include<cstdlib>
#include<algorithm>
#define db double
#define Re register
const int N = 2000 + 5;
const db INF = 1e18;
using std :: max;
using std :: min;
inline int read() {
	int f = 1, x = 0; char ch;
	do { ch = getchar(); if (ch == '-') f = -1; } while (ch < '0' || ch > '9');
	do {x = (x << 3) + (x << 1) + ch - '0'; ch = getchar(); } while (ch >= '0' && ch <= '9'); 
	return f * x;
}
inline void init(), dp(), ouot();
signed main() { init(), dp(), ouot(); }
inline void hand_in() {
	freopen("classroom.in", "r", stdin);
	freopen("classroom.out", "w", stdout);
}
int n, m, v, e, c[N], d[N];
db k[N], f[N][N][2], dis[305][305], ans = INF;
inline void init() {
	hand_in();
	n = read(), m = read(), v = read(), e = read();
	for (Re int i = 1;i <= n; ++i) c[i] = read();
	for (Re int i = 1;i <= n; ++i) d[i] = read();
	for (Re int i = 1;i <= n; ++i) scanf("%lf", k + i);
	for (Re int i = 1;i <= v; ++i) {
		for (Re int j = 1;j <= v; ++j) {
			if (i != j) dis[i][j] = INF;
		}
	}
	for (Re int i = 1, x, y, w;i <= e; ++i) {
		x = read(), y = read(), w = read();
		dis[x][y] = min(dis[x][y], (db)w);
		dis[y][x] = dis[x][y];
	}
	for (Re int s = 1;s <= v; ++s) {
		for (Re int i = 1;i <= v; ++i) {
			for (Re int j = 1;j <= v; ++j) {
				dis[i][j] = min(dis[i][j], dis[i][s] + dis[s][j]);
			}
		}
	}
}
inline void dp() {
	for (Re int i = 0;i <= n; ++i) {
		for (Re int j = 0;j <= m; ++j) {
			f[i][j][0] = f[i][j][1] = INF;
		}
	}
	f[1][0][0] = f[1][1][1] = 0;
	for (Re int i = 2, lim;i <= n; ++i) {
		lim = min(i, m);
		f[i][0][0] = f[i - 1][0][0] + dis[c[i - 1]][c[i]];
		for (Re int j = 1;j <= lim; ++j) {
			f[i][j][0] = min(f[i][j][0], f[i - 1][j][0] + dis[c[i - 1]][c[i]]);
			f[i][j][0] = min(f[i][j][0], f[i - 1][j][1] + k[i - 1] * dis[d[i - 1]][c[i]] + (1.0 - k[i - 1]) * dis[c[i - 1]][c[i]]);
			f[i][j][1] = min(f[i][j][1], f[i - 1][j - 1][0] + k[i] * dis[c[i - 1]][d[i]] + (1.0 - k[i]) * dis[c[i - 1]][c[i]]);
			f[i][j][1] = min(f[i][j][1], f[i - 1][j - 1][1] + k[i] * (k[i - 1] * dis[d[i - 1]][d[i]] + (1.0 - k[i - 1]) * dis[c[i - 1]][d[i]]) + (1.0 - k[i]) * (k[i - 1] * dis[d[i - 1]][c[i]] + (1.0 - k[i - 1]) * dis[c[i - 1]][c[i]]));
		}
	}
}
inline void ouot() {
	for (int i = 0;i <= m; ++i) ans = min(ans, min(f[n][i][0], f[n][i][1]));
	printf("%.2lf", ans);
	exit(0);
}
```



### 进阶：

#### [亚瑟王](https://www.luogu.org/problemnew/show/P3239)

思考后，转化成$ans=\sum dp[i]*d[i]$，其中,$dp[i]$表示第$i$张卡在$r$轮中打出的概率。

考虑如何计算$dp[i]$。

由于每一轮打出一张卡后，该轮结束，所以每张卡在$r$轮中被打出的概率与在它前面有多少张卡被打出有关。

设$f[i][j]$表示在$r$轮中，前$i$张卡被打出了$j$张的概率，在此情况下，第$i+1$张牌在$r$轮中有$(1-p[i+1])^{r-j}$的概率未被打出，再用$1$减去，就得到了打出的概率。所以枚举$k$得到：$dp[i]=\sum f[i-1][k]*(1-(1-p[i])^{r-k})$。

现在考虑如何计算$f[i][j]$。

递推。当前的第$i$张卡选或不选。
选：$f[i][j]+=f[i-1][j-1]*(1-(1-p[i])^{r-j+1})$。$(j\neq 0)$
不选：$f[i][j]+=f[i-1][j]*((1-p[i])^{r-j})$。$(i\neq j)$

边界：
$f[1][1]=dp[1]=1-(1-p[1])^r$
$f[1][0]=(1-p[1])^r$

本题得到解决。（注意精度！）

```cpp
#include<cmath>
#include<ctime>
#include<queue>
#include<cstdio>
#include<cstdlib>
#include<cstring>
#include<iostream>
#include<algorithm>
#define debug() puts("FBI WARNING!")
#define ll long long
using namespace std;
const int MAX = 220 + 5;
const int P = 1e5 + 7;

inline int read(){ 
    int f = 1, x = 0;char ch;
    do { ch = getchar(); if (ch == '-') f = -1; } while (ch < '0'||ch>'9');
    do {x = x*10+ch-'0'; ch = getchar(); } while (ch >= '0' && ch <= '9'); 
    return f * x;
}
int t, n, r;
double p[MAX], d[MAX], ans, f[MAX][MAX], dp[MAX], pw[MAX][MAX];
inline double mi(double a, int b) {
    double res = 1.0;
    while (b) {
        if (b & 1) {
            res *= a;
        }
        a *= a;
        b >>= 1;
    }
    return res;
}
int main(){
    t = read();
    while (t--) {
        memset(dp, 0, sizeof (dp));
        memset(f, 0, sizeof (f));
         ans = 0;
        n = read(), r = read();
        for (int i = 1;i <= n; ++i) scanf("%lf %lf", &p[i], &d[i]);
        f[1][1] = dp[1] = 1.0 - mi(1.0 - p[1], r);		
        f[1][0] = mi(1.0 - p[1], r);
        
        for (int i = 2;i <= n; ++i) {
            for (int j = 0;j <= min(i, r); ++j) {
                
                if (j) {
                    f[i][j] += f[i - 1][j - 1] * (1.0 - mi(1.0 - p[i], r - j + 1));
                }
                if (i != j) {
                    f[i][j] += f[i - 1][j] * mi(1.0 - p[i], r - j);
                }
            }
        }
        
        for (int i = 2;i <= n; ++i) {
            for (int k = 0;k <= min(i - 1, r); ++k) {
                dp[i] += f[i - 1][k] * (1.0 - mi(1.0 - p[i], r - k));
            }
        }
        for (int i = 1;i <= n; ++i) {
            ans += dp[i] * d[i];
        }
        printf("%.10lf\n", ans);
    }
    return 0;
}
```

#### [概率充电器](https://www.luogu.org/problemnew/show/P4284)

$WA$了无数遍。。。有一个坑人的小细节。。。

思考后，可转换成$ans=\sum (1-res[i])$。其中，$res$表示节点$i$未被充电的概率。

强制把这棵树转化为有根树，我们可以发现，对与任意非根节点，它能否被点亮取决于它的子节点以及它的父亲。想到树形$DP$。

我们设$f[i]$，$g[i]$分别表示$i$不被它的子节点点亮的概率，$i$不被它父亲点亮的概率。

所以$res[i]=f[i]\times g[i]$。

现在思考如何求$f[i]$。

第一遍$dfs$遍历时，直接 $f[i]=(1-p[i])\times \prod_{son\in i}\lgroup1-(1-f[son]\times val(i,son))\rgroup$，无需过多的解释。

由于根节点无父节点，所以$g[root]=1$，即$res[root]=f[root]$现在，思考如何求非根节点的$g[i]$。

做到这里，我先前有个抽风的想法，以为直接可以$g[v]=ans[u]+(1-ans[u])\times (1-val(u,v))$($v$是$u$的子节点)，当$WA$到怀疑人生时才想到这是错误的。。。想想为什么？

请注意，$g[i]$的状态定义是：$i$不被它**父亲**点亮的概率，所以不管子节点鸟事。。。即**默认为$v$不带电**，所以我们需要把$ans[u]$除去先前乘进$f[i]$中的$v$子树中不带电的概率。

即$P=\frac{g[u]\times f[u]}{1-(1-f[v]\times val(u,v))}$，$g[v]=P+(1-P)\times (1-val(u,v))$。

本题得到$O(n)$解决。

```cpp
#include<cmath>
#include<ctime>
#include<queue>
#include<cstdio>
#include<cstdlib>
#include<cstring>
#include<iostream>
#include<algorithm>
using namespace std;
const int MAX = 500000 + 5;
inline int read(){ 
    int f = 1, x = 0;char ch;
    do { ch = getchar(); if (ch == '-') f = -1; } while (ch < '0'||ch>'9');
    do {x = x*10+ch-'0'; ch = getchar(); } while (ch >= '0' && ch <= '9'); 
    return f * x;
}
int n, m;
double p[MAX], ans[MAX], res, f[MAX], g[MAX], h[MAX];

struct sakura {
    int to, nxt;
    double p;
}sak[MAX << 1]; int head[MAX], cnt;

inline void add(int x, int y, double p) {
    ++cnt;
    sak[cnt].to = y, sak[cnt].nxt = head[x], sak[cnt].p = p, head[x] = cnt;
}

inline void dfs_1(int u, int fa) {
    f[u] = 1.0 - p[u];
    g[u] = 1.0;
    for (int i = head[u];i;i = sak[i].nxt) {
        int v = sak[i].to;
        double ps = sak[i].p;
        if (v == fa) continue;
        dfs_1(v, u);
//        f[u] *= (1.0 - (1.0 - f[v]) * ps);
        f[u] *= (f[v] + (1.0 - f[v]) * (1.0 - ps));
    }
}

inline void dfs_2(int u, int fa) {
	for (int i = head[u];i;i = sak[i].nxt) {
        int v = sak[i].to;
        double ps = sak[i].p;
        if (v == fa) continue;
        double h;
        if (f[v] + (1.0 - f[v]) * (1.0 - ps)) {
     	  h = (g[u] * f[u]) / (f[v] + (1.0 - f[v]) * (1.0 - ps));        	
        }
		else h = 0.0;
        g[v] = h + (1.0 - h) * (1.0 - ps);
        dfs_2(v, u);
    }
}
int main(){
    n = read();
    for (int i = 1;i < n; ++i) {
    	int a = read(), b = read();
    	double c; scanf("%lf", &c);
    	add(a, b, 0.01 * c);
    	add(b, a, 0.01 * c);
    }
    for (int i = 1;i <= n; ++i) 
    	scanf("%lf", &p[i]), p[i] *= 0.01;
    dfs_1(1, 0);
    dfs_2(1, 0);
    g[1] = 1;
    for (int i = 1;i <= n; ++i) {
    	ans[i] = f[i] * g[i];
    }
    for (int i = 1;i <= n; ++i) 
    	res += (1.0 - ans[i]);
    printf("%.6lf", res);
    return 0;
}
```

#### [奖励关](https://www.luogu.org/problemnew/show/P2473)

刚拿到这题。

？？？

$Woc$！怎么做？

好在看了眼数据范围，，，哦，，，状压套个期望啊。。。

倒着找，就行了。

```cpp
#include<cstdio>
#include<cstdlib>
#include<cstring>
#include<iostream>
#include<algorithm>
#define Re register
using namespace std;
const int MAX = 100 + 5;
inline int read(){ 
    int f = 1, x = 0;char ch;
    do { ch = getchar(); if (ch == '-') f = -1; } while (ch < '0'||ch>'9');
    do {x = x*10+ch-'0'; ch = getchar(); } while (ch >= '0' && ch <= '9'); 
    return f * x;
}
int k, n, miko[MAX];
double f[MAX][1 << 15], a[MAX], ans;
int main(){
    k = read(), n = read();
    for (int i = 1;i <= n; ++i) {
    	scanf("%lf", &a[i]);
    	int x;
    	while (x = read()) {
    		miko[i] = miko[i] | (1 << (x - 1));    		
    	}
    }
    for (int i = k;i >= 1; --i) {
    	for (int j = 0;j < (1 << n); ++j) {
    		for (int l = 1;l <= n; ++l) {
    			if ((j & miko[l]) == miko[l]) {
    				f[i][j] += max(f[i + 1][j | (1 << (l - 1))] + a[l], f[i + 1][j]);
    			}
    			else {
    				f[i][j] += f[i + 1][j];
    			}
    		}
    		f[i][j] /= n;
    	}
    }
    printf("%.6lf\n", f[1][0]); 
}
```

#### [迷失游乐园](https://www.luogu.org/problemnew/show/P2081)

**马马马？？？**

是树的情况貌似和前面的看脸充电器差不多？？？搞一下。

我们设从一个节点$i$的子节点传上来的期望值为$f[i]$，从父节点传下来的期望值为$g[i]$，令$u$为$v$的父节点，所以可得$f[u]=\frac{\sum f[v]+w(u,v)}{son[u]}$，然后以此更新$g[v]$：$g[v]=w(u,v)+\frac{f[u]\times son[u]-w(u,v)-f[v]+g[u]}{son[u]-1+1}$。

具体含义就$YY$一下吧，懒癌晚期。。。

$About$ $1h$ $later...$ $have$ $50$ $Pt$ $...$

好了，现在处理环的情况！  

$Another$ $1h$ $later...$ 

$Woc$！环怎么处理啊！

偷偷瞟了眼题解，稍微有了点思路。

首先，从基环树环上节点的子节点传上来的更新是一样的，这点没有任何怀疑。主要是$g[i]$的更新。

对于环上的点，我们默认为它们的父亲即为环上与它相邻的两个节点。所以它分别有一半的概率走到与它相邻的两个节点上。  

所以，我们强迫它先顺序走一遍，再逆序走一遍，最后两个期望值相加再除以$2$，即可得到$g[x]$。

$g[i]=\sum P_j\times (w(j-1,j)+f[j]\times \frac{son[j]}{son[j]+1})$

其中,$P_j$表示为走到环上节点$j$的概率。

然后环上的$g[x]$更新了，就可以对于环上每个节点所形成的子树进行遍历更新了。

$g[v]=w(u,v)+\frac{fat[u]\times g[u]+f[u]\times son[u]-f[v]-w(u,v)}{fat[u]+son[u]-1}$

其中，$fat[x]$表示$x$父节点的个数。

找环简单说一下，直接开栈记录就行了。

再吐槽一下，这题我改了一下午加一晚上，还是红彤彤的，后来一气之下全员$double$，，，然后，，，它就过了。。。

这题必须代码。

```cpp
#include<cstdio>
#include<cstdlib>
#include<cstring>
#include<iostream>
#include<algorithm>
#define Re register
#define C(x) circle[x] 
#define T(x) tag[x]
const int MAX = 100000 + 5;
inline int read(){ 
    int f = 1, x = 0;char ch;
    do { ch = getchar(); if (ch == '-') f = -1; } while (ch < '0'||ch>'9');
    do {x = x*10+ch-'0'; ch = getchar(); } while (ch >= '0' && ch <= '9'); 
    return f * x;
}
struct sakura { int  to, nxt, w; }sak[MAX << 1]; int head[MAX], cnt;
double f[MAX], ans, g[MAX], res, son[MAX], P, dis[25][25], fat[MAX];
int stack[MAX], top, vis[MAX], circle[MAX], count, num, st, nex[MAX], pre[MAX], tag[MAX];
bool find = 0;
inline void add(int x, int y, int w) { ++cnt; sak[cnt].to = y, sak[cnt].nxt = head[x], sak[cnt].w = w, head[x] = cnt; } 

/* 找环 */ 
inline void pre_dfs_1(int u, int fa) {
    if (find) return;
    vis[u] = 1, stack[++top] = u;
    for (int i = head[u];i;i = sak[i].nxt) {
        int v = sak[i].to;
        if (v == fa) continue;
        if (vis[v]) {
            while (stack[top] != v) { circle[++count] = stack[top--]; }
            circle[++count] = stack[top];
            find = 1;
            return;
        }
        else {
            pre_dfs_1(v, u);    
            stack[--top], vis[v] = 0;
        }
    }
}

/* 找距离 */ 
bool first = 1;
inline void pre_dfs_2(int u, int fa, double w) {
    if (u == st && !first) {
        dis[T(u)][T(fa)] = dis[T(fa)][T(u)] = w;
        return;
    }
    first = 0;
    for (int i = head[u];i;i = sak[i].nxt) {
        int v = sak[i].to;
        double s = sak[i].w;
        if (v == fa || !vis[v]) continue;
        dis[T(u)][T(v)] = dis[T(v)][T(u)] = s;
        pre_dfs_2(v, u, s);
    }
}

/* douwn */
inline void dfs(int u, int fa) {
    for (int i = head[u];i;i = sak[i].nxt) {
        int v = sak[i].to;
        double w = sak[i].w;
        if (v == fa || vis[v]) continue;
        dfs(v, u);
        son[u] ++;
        f[u] += (1.0 * f[v] + w);
    }
    if (son[u]) f[u] /= son[u];
}

/* up */
inline void re_dfs(int u, int fa, double w) {
   g[u] = w;
    if(fat[fa] + son[fa] > 1)
        g[u] += (fat[fa] * g[fa] + son[fa] * f[fa] - f[u] - w) / (fat[fa] + son[fa] - 1);
    for (int i = head[u];i;i = sak[i].nxt) {
        int v = sak[i].to;
        double w = sak[i].w;
        if (v == fa || vis[v]) continue;
        re_dfs(v, u, w);
    }
}

/* m = n-1 */
inline void dfs_tree(int u, int fa) {
    for (int i = head[u];i;i = sak[i].nxt) {
        int v = sak[i].to;
        double w = sak[i].w;
        if (v == fa) continue;
        if (u != st) 
            g[v] = w + (f[u] * son[u] - w - f[v] + g[u]) / son[u]; 
        else {
            if (son[u] == 1) {
                g[v] = w;
            }
            else {
                g[v] = w + (f[u] * son[u] - w - f[v] + g[u]) / (son[u] - 1);
            }
        }
        dfs_tree(v, u);
    }
} 

int n, m; 
int main(){
    n = read(), m =  read();
    for (Re int i = 1;i <= m; ++i) {
        int x = read(), y = read(), z = read();
        add(x, y, z), add(y, x, z);
    }
    if (m != n) {
    	/* 树的情况比较好转移 */
    	st = 1;
        dfs(st, 0);
        dfs_tree(st, 0);
        for (int i = 1;i <= n; ++i) {
            if (i == st) {
                res += (f[i] * son[i] + g[i]) / son[i];
            }
            else {
                res += (f[i] * son[i] + g[i]) / (son[i] + 1);
            }

        }
        printf("%.5lf", res / (1.0 * n));       
        return 0;
    }
    else {
    	/* 找环 */
        pre_dfs_1(1, 0);
        
        /* 标记 & 映射 */
        memset(vis, 0, sizeof (vis));
        for (Re int i = 1;i <= count; ++i) vis[circle[i]] = 1, tag[circle[i]] = i;
        
        /* 找距离 */ 
        st = circle[1];
        pre_dfs_2(circle[1], 0, 0);
		
		/* 对于每个环上的点down下去 */
        for (Re int i = 1;i <= count; ++i) dfs(circle[i], 0);

        for (int i = 1;i <= n; ++i) {
			if (vis[i]) {
				fat[i] = 2.0;
				//在环上父亲数为 2 
			}
			else {
				fat[i] = 1.0;
				//不在环上父亲数为 1 
			}
        }

		/* 处理信息 */
        for (int i = 1;i <= count; ++i) {
            nex[circle[i]] = circle[i + 1];
            pre[circle[i]] = circle[i - 1];
        }
		pre[circle[1]] = circle[count];
		nex[circle[count]] = circle[1];

		/* 更新环上的g[x] */
        for (Re int i = 1;i <= count; ++i) {
            int nows = C(i);       	
            /* 正序来一遍 */
            P = 1.0;
            for (Re int j = nex[nows];j != nows; j = nex[j]) {
                double w = dis[tag[pre[j]]][tag[j]];
                if (nex[j] == nows) 
                     g[nows] += P * (w + f[j]);
                else 
                    g[nows] += P * (w + f[j] * son[j] / (son[j] + 1));
                P /= (son[j] + 1);
            }
            /* 逆序来一遍 */
            P = 1.0;            
            for (Re int j = pre[nows];j != nows; j = pre[j]) {
                double w = dis[tag[nex[j]]][tag[j]];
                if (pre[j] == nows) 
                    g[nows] += P * (w + f[j]);
                else 
                    g[nows] += P * (w + f[j] * son[j] / (son[j] + 1));
                P /= (son[j] + 1);
            }
            /* 除 2 */
            g[nows] /= 2.0;
        }
        
        /* 更新非环节点g[x] */
        for (int i = 1;i <= count; ++i) {
            for (int j = head[C(i)];j;j = sak[j].nxt) {
                if (!vis[sak[j].to]) re_dfs(sak[j].to, circle[i], sak[j].w);
            }
        }
        
        /* 统计答案 */
        for (int i = 1;i <= n; ++i) 
            res += ((g[i] * fat[i]) + f[i] * son[i]) / (fat[i] + son[i]);
        printf("%.5lf", res / (1.0 * n));
        return 0;       
    }
}
```

## 一点小总结

概率$DP$怎么说呢，真的还是以推$DP$式子为主，但其中有些和其它$DP$不一样，比如，概率作为状态时一般是反着来的，还有就是当一个状态的概率不好表示时，想想去表示它相反的概率，抑或者用容斥原理把概率给硬搞出来，期望同理。

还有就是在$OI$的运用中，大多数题目都是离散型变量，很少连续型的，注意一下它俩的区别。