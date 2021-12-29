---
title: codeforces
mathjax: true
date: 2019-12-07 21:13:59
tags: 
    - problems
    - cf
categories: 
    - 题目
    - cf
top: 1
---

----------
# [codeforces566C](http://codeforces.com/problemset/problem/566/C)

## **description**
- $n$个点的树，点有点权，边有边权，定义树上两点距离$dis(u,v)=( \sum_{e\in path(u, v)}w_e)^{1.5}$，求带权中心位置
- $n\leq 100000$

## **solution**
- 直接维护显然不行，考虑带权重心能否从$u$移动到$v$
![1.png](https://i.loli.net/2021/08/21/W7hzZPHLNu6BYrK.png)
但是直接比较$u$, $v$答案不弱于原题，先做出带权重心可以在边上某处的假设，设$u$与带权重心距离$x$
$$f(x)=\sum_{i \in A}a_i(d_i+x)^{1.5}+\sum_{i \in B}a_i(d_i+l-x) ^ {1.5}$$
有上述假设后$f$是连续函数，$f''(x)>0$，函数是凸函数，只有一个最低点，
所以$f'(0)>=0$时，$u\rightarrow v$必然不优
$$
\begin{aligned}
f'(0)&=f'(x)|_{x=0}\\
&=1.5[\sum_{i \in A}a_i(d_i+x)^{0.5}-\sum_{i \in B}a_i(d_i+l-x) ^ {0.5}]|_{x=0}\\
&=1.5[\sum_{i\in A}^na_idis(u, i)^{0.5}-\sum_{i\notin A}^na_idis(u, i)^{0.5}]
\end{aligned}
$$
以$u$作为根，记$sum_x=\sum_{i\in subtree(x)}a_id_i^{0.5}$, 满足$f'(0)<0$的$v$即为$u$儿子$sum$的绝对众数，至多一个
至此，做法仍为$O(n^2)$，但移动方向唯一很有启发
考虑点分治，分治层数最多$\log n$，向子树**移动方向唯一**，只需关心点分树的一条链，那么总共只有$O(\log n)$个点需要计算，总复杂度$O(n\log n)$
树上最值的一些问题，一条路是大力维护，还有一条是**对可选点进行比较，不断向更优位置靠近**，像本题是从带权重心向四周答案不断增大的结构。本题中化离散为连续的思想也值得借鉴


----

# [codeforces566E](http://codeforces.com/problemset/problem/566/E)

## **description**
- $n$个点的树，已知与每个点$u$相距不超过2的点集$S_u$，但是$\{S\}$乱序给出，恢复这棵树(答案不唯一任取)
- $n\leq 1000$

## **solution**
- 第一种想法是从叶子开始向上删点，但集合乱序挺恶心
换个角度，试图分析点集性质。

**part1**

- 把$S_x$中的元素提取出来，会发现$x$的爷爷只与$x$父亲相连
![1.png](https://i.loli.net/2021/08/20/HJSWTofKsLh7pFi.png)
根据上图，选择红点与绿点，两集合交集大小为2，在交集元素间连边
通过判断两两集合交集为2，可以连上所有不挂叶子的边。

**part2(接叶子)**

- 考虑叶子$u$，包含$u$的集合中大小最小的必定为$S_u$
根据$S_u$将叶子划成若干子集
每个子集互为兄弟，分别考虑。
为方便，记$adj_u$为$u$和与直接有边相连
若有叶子$u_1, u_2, \cdots, u_p$，记$F = fa_{u_1}$, 容易发现
$S_{u_1}$包括$adj_F$，以及这些叶子
而$adj_u$在非叶节点大于两个时两两不同，可以唯一区分

**part3**

- 还要注意，非叶节点不超过两个还需要特判

------

# [codeforces585C](http://codeforces.com/problemset/problem/585/C)

## **description**

$a=1,b=1$，构造一个AB字符串，遇到Ａ执行$b=a+b$，遇到Ｂ执行$a=a+b$
要求最终$a=x ,b=y$
**data limit** $x,y\leq 1e18$

## **solution**

从终态逆推，容易发现是在执行辗转相减，运用辗转相除求gcd的过程加速辗转相减
注意$\gcd(x,y) > 1$是无解

------

# [codeforces585E](http://codeforces.com/problemset/problem/585/E)

## **description**

> 有n个数，可以从中任意选取一个x，然后可以从剩下的数中选取任意个构成一个集合S，使集合S中所有数的$gcd(S)>1$，且$gcd(gcd(S),x)=1$，求方案数。

## **solution**

先用$tmp[i]$表示$i$的倍数有几个，
对于每个质数$x$，选出$x$的倍数(共$tmp[x]$个)，会对答案贡献$(n-tmp[x])*(2^{tmp[x]}-1)$
此时考虑$prime1\ast prime2$，在枚举$prime1,prime2$时都计算到，需要$ans-=(n-tmp[x])\ast (2^{tmp[x]}-1)$；
类似的，三个质数乘积$p\ast q\ast r$，在$p,q,r$分别加一次，$p\ast q$，$p\ast r$，$q\ast r$分别减一次，因此还要加一遍
若一个质因子$p$出现两遍，要么在$p$中算过，要么$gcd!=1$，因此对答案没有贡献
容易发现，容斥系数恰好为$-mu[x]$
暴枚$i$，$ans=\sum_{i}(-mu[i])\ast(2^{tmp[i]}-1)\ast(n-tmp[i])$

------

# [codeforces773D](http://codeforces.com/problemset/problem/773/D)

## **description**

给一个完全图，边有边权，求以每个点为根的内向树，使所有点到该点的距离和最小，注意，距离定义为两点之间路径边权的最小值
$n\leq 2000$

## **solution**
先要充分挖掘这棵内向树的性质：
如果存在$y-x$和$z-x$两条边，把$z-x$变为$z-y$一定**不会变劣**，因此，**每个点最多一条入边**，也就是说，这是**一条链**
不妨设最优的链由根开始的权值为$w_1,w_2,w_3,\cdots,w_{n-1}$，则有$ans=\sum_{i=1}^{n-1}\min_{j=1}^iw_i$
这样仍旧不好处理，有一步重要的转化：设所有边权值最小为$M$，把每条边减去$M$，答案加上$M(n-1)$，此时，必有一条边**权值为零**，只要途经这条边，后面的值均是零
不妨设链上最靠前的零边为$w_k$，经过画图会发现，$\forall i\leq k-3,w_i>w_{i+1}$，下面简单证一下：
假设存在$w_i\leq w_{i+1}$，那么可以把$e_{i+1}$直接连向$0$边一端，再从零边另一端走出，结果不劣，附图说明：[![BLiOQf.png](https://s1.ax1x.com/2020/11/10/BLiOQf.png)](https://imgchr.com/i/BLiOQf)
然后路径只有两种，要么由根通往$0$边某端点的路径长度，要么允许$2w$的代价瞬移至零边端点，直接对于每个点跑dij，获得$O(n^3)$的复杂度
改成$O(n^2)$就差一步，只需把**$dis_i$初始化成最小出边的两倍**，直接跑dij即可（等于说是**多加入一个源点进行一轮扩展**）


----------------------

# [codeforces1243D](http://codeforces.com/problemset/problem/1243/D)

## **description**

$n$个点的完全无向图，$m$条边边权为1，其余边权为0，求最小生成树
$n,m\leq 10^5$

## **solution**

容易得到答案为补图的联通块个数，运用链表+bfs解决
每次选取一个未分配的点，从这个点 bfs。链表中存储没访问的点
假设当前点是 u ，如果原图上有边 $u \rightarrow v$，就把点 v 标记（$cover[v]=1$）。从链表头遍历，对于$cover[v]=0$的点，塞入队列，并从链表中删除。
注意还要把标记取消!!!
因为每个点和每条边都只会走一次，复杂度是$O(n+m)$ 。

------



# [codeforces1247E](http://codeforces.com/problemset/problem/1247/E)

## **description**

一个n*m的矩阵，有个别位置会有石头，石头受到撞击会沿着方向撞到墙为止（每个石头会占一个格子）。你从左上角出发，只能向右或者向下走，问你走到右下角的方案数。

## **solution**

先统计每个位置右边，下边有几个石头，存在$row$与$col$中
考虑动态规划，$dp[i][j][0]$表示从$(i,j)$出发向右移动至终点的方案数，$dp[i][j][1]$表示从$(i,j)$出发向下移动至终点的方案数。考虑向右移动时，最远可以移到$(i,m-row[i][j+1])$，$$dp[i][j][0]=\sum_{p=j+1}^{m-row[i][j+1]}dp[i][p][1]$$
同理 $$dp[i][j][1]=\sum_{p=i+1}^{n-col[i+1][j]}dp[p][j][0]$$
运用前缀和优化$dp$即可
时间复杂度$O(n^2)$

------



# [codeforces1284D](http://codeforces.com/problemset/problem/1284/D)

## **description**

线段集合$A,B$分别含有$n$条线段，若存在$A_i$与$A_j$相交，但$B_i$与$B_j$不相交，输出no

## **solution**

与[cf1106E](http://codeforces.com/problemset/problem/1106/E)预处理做法相似，类似于扫描线
对于每一段线段：
1.在 $tim=sa_i$ 处插入 $[sb_i,eb_i]$，并加入$insert$标记（即$typ=1$）
2.在 $tim=ea_i+1$ 处删除 $[sb_i,eb_i]$，并加入$remove$标记（即$typ=-1$）
把上述$2*n$个事件按$tim$排序
**attention：当 $tim$ 相等时，先删除区间，再加入新的区间！！！**
这样的话，对于每个时间点$tim$，能够维护哪些$A$线段会覆盖$tim$。显然，这些$A$线段两两相交，因此，对应的$B$线段也必须两两相交。（**核心**）
区间$[a,b]$ 与 $[c,d]$不相交，当且仅当 $b<c$ 或 $d<a$ 。

若 $ea_i < \max(sb)$ 或 $sa_i > \min(eb)$ ，则新加入的线段不满足两两相交
上述信息只需要维护两个$multiset$，一个维护插入的区间左端点$sb$，另一个维护插入的区间的右端点$eb$
上述过程仅仅考虑$A$线段相交,$B$线段不相交，还需要交换$A$,$B$后再来一次


-----
# [codeforces1363F](http://codeforces.com/problemset/problem/1363/F)

## **description**
给出长为n的字符串s与t，一次操作可以把s中的一个字符取出，并放置到原位置之前，试求把s变成t的最小步数

## **solution1**
 考虑动态规划，$dp[i][j]$表示$S$中的$i$个字符与$T$中$j$个字符匹配（其中一个是另一个的后缀）的最小步数
 边界是$dp[i][0]=0$
 考虑转移过程：
 1.丢弃S中的第$i$个字符（**不是下标为i**），之后再用一步匹配该位置，$dp[i][j]=dp[i-1][j]+1$
 2.$s[i]==t[j]$时，无需操作，$dp[i][j]=dp[i-1][j-1]$
 3.$dp[i][j]=dp[i][j-1]$，但是想要如此转移有一个先决条件，**$S$中剩下的字符中$t[j]$的个数必须比$T$中剩下的字符中$t[j]$的个数多**，若在预处理阶段处理$suf[i][j]$表示$j+1--n$中字符$i$的个数，$suf_s[t[j]][i+1]>suf_t[t[j]][j+1]$
 推荐用记忆化实现
 时间复杂度$O(n^2)$
## **solution2**

 联想到$LCS$的求解
 最初感觉$answer=n-LCS(s,t)$，但是马上会有反例：$S=abb,T=bba$,答案应为$2$
 考虑题目限制条件，字符只能从后面移动到前面，对于$S_{i+1 \sim  n}$与$T_{j+1 \sim n}$，一旦S中某个字符比T少，就无法匹配
 $dp[i][j]=\max(dp[i-1][j],dp[i][j-1])$不受影响
 转移$dp[i][j]=(dp[i-1][j-1]+1)*[s[i]=t[j]]$时必须先满足$\forall k, suf[k][i+1]\geq suf[k][j+1]$
 时间复杂度$O(n^2)$ 

------


# [codeforces1373F](http://codeforces.com/problemset/problem/1373/F)

## **description**

有$n$个壶，$n$个桶，第$i$个桶可以往第$i$个壶与第$i+1$个壶里倒水(特殊的，$n$号桶可以往第1个壶内倒水)，问能否灌满所有壶

## **solution**

首先，记1号桶往1号壶内倒入的水量为$S$，如果知道$S$，显然可以再$O(n)$时间内判断是否可行
会发现如果$1$号桶往$1$号壶倒的水过多，可能导致后面的壶倒不满了，此时需要调少该水量 
否则，可以调高该水量
上述两行发现，$S$对答案的影响是单调的，可以二分实现
注意二分上下界不要写错
时间复杂度$O(nlogn)$

------

# [codeforces1086D](http://codeforces.com/problemset/problem/1086/D)

## **description**

$n$个人拍成一排玩石头剪刀布，初始给定每个人的手势。进行$q$次询问，每次询问修改一个人的手势，查询有多少人可能获胜。比赛规则是进行$n-1$轮，每一轮由你指定相邻两人比拼，输者淘汰，若平局，由你指定一人淘汰。

## **solution**

一、手势唯一时，答案为$n$
二、手势种类为两种时，答案为获胜的手势个数
三、手势有三种时，分别考虑每一种，记种类编号为$x$，$x$可以胜过$y$，但不能胜过$z$
要使$x$能获胜，他的左右两侧分别至少出现一个$y$（$y$一定胜过$z$，$x$可以两两对峙）
具体实现用了三个$set$存不同手势，并用树状数组记录三种手势的位置。
记$yl$为最左侧的$y$，$yr$为最右侧的$y$，记$zl$为最左侧的$z$，$zr$为最右侧的$z$
如果$zl<yl$，则$ans-=query(x,yl)-query(x-zl)$
如果$yr<zr$，则$ans-=query(x,zr)-query(z,yr)$

------------------------

# [codeforces1237E](http://codeforces.com/problemset/problem/1237/E)
## **description**
 询问$n$个点的符合以下要求的二叉搜索树有几个：每个点上标$1-n$的数字，互不重复，每个点左儿子的奇偶性与之相异，右儿子的奇偶性与之相同，且只有最后一层不满。
 $n \leq 10 ^ 6$

## **solution**

首先，仅有最后一层不满，必定为满二叉树上加若干条边得到。
其次，考虑$x$是$y$的左儿子，$x\rightarrow r\rightarrow y$，由于奇偶性不同，$size(rson(x))\mod 2=0$
![graph.png](https://i.loli.net/2020/09/12/FGIp8h9Q6iuzxSE.png)
然后，考虑$x$是$y$的右儿子，$y\rightarrow l\rightarrow x$，由于奇偶性相同，$size(lson(x))\mod 2=1$
![graph.png](https://i.loli.net/2020/09/12/cEOIwpgMRaPBxdJ.png)
总结一下，若一个点$x$是左儿子，$size(rson(x))\mod 2=0$，否则$size(lson(x))\mod2=1$
所以，满二叉树的最后一层节点要么是叶子，要么只有左儿子。
然后会有一个朴素的算法：由底到顶考虑满二叉树中的节点，根据上述总结判断，若不满足，答案自增，该点到根的路径上所有点的$size$自增，时间复杂度$O(nlogn)$。容易发现，只用保存$size\mod2$，自增改为$xor 1$即可，会发现答案只有$0$或$1$。
通过~~模拟~~思考发现，遍历完最后一层节点后，倒数第二层$size%2$均为$0$，其余不变
记$a[x]$为在$dep=x$的满二叉树上加几条边才能满足条件，$a[1]=1,a[2]=1,a[3]=2$
对于$i>3$，有$a[i]=a[i-2]+2^{i-2}$，递推即可，当且仅当$n=dep+a[dep]\ $或$\ n=dep+a[dep]+1$时，$ans=1$


------

# [codeforces1237F](http://codeforces.com/problemset/problem/1237/F)
## **description**
一个$n*m$的网格，$ban$掉了若干行与若干列，问摆放任意多张$1\ast 2$的矩形，且每行每列中不存在两个格子属于不同的矩形的方案数，对$998244353$取模
$n\leq 3600$

## **solution**

很好的$dp$计数题。
首先考虑枚举横向的矩形个数$x$，与纵向的矩形个数$y$，则我们需要$x+y+y$个空行，且有$y$组连续的空行，列同理。这个东西似乎不好处理，但如果先考虑有$y$组连续的空行，剩下的$x$行仅仅是一个组合数而已。
定义$dp[x][y]$为考虑前$x$行，取了$y$组连续两个的空行的方案数，转移很方便：
$$dp[x][y]=dp[x-1][y]$$
$$dp[x][y]+=dp[x-2][y-1]*[!ban[x]\ \wedge \ !ban[x-1]\ \wedge x>=2\ \wedge y>=1]$$
再定义$dp_{row}[x][y]$为$y$组连续两个的空行，$x$个空行的方案数：
$$dp_{row}[x][y]=dp[n][y]*\binom{n-ban-y-y}{x}$$
列同理
统计答案时，横向矩形之间可以交换位置，方案数为$x!$
$$ans=\sum_{x=0}^{n/2}\sum_{y=0}^{m/2}dp_{row}[x][y]*dp_{col}[y][x]*x!*y!$$

-----------------------------

# [codeforces1086F](https://codeforces.com/problemset/1086/F)
##**description**
有$n$个着火点，每一秒与已着火的点八连通的点会着火，着火持续$t$秒，求$\sum_x\sum_ytim(x,y)$，其中$tim(x,y)$指$(x,y)$着火的时间，若$(x,y)$未着火则$tim(x,y)=0$
$n\leq50\ \ |x|\leq 1e8$
##**solution**##
首先考虑暴力算法。会发现计算第$x$秒有几个新着火点不太容易，但计算截止第$x$秒全部着火的点相对容易得多，我们记$f(x)$为前$x$秒的着火点数量，边界$f(0)=n$，如果计算单个$x$的函数值，可以把若干个边长为$x+x+1$的正方形做面积并，这样的问题可以用扫描线解决（甚至不必写线段树，直接写$O(n^2)$暴力即可）
考虑计算答案，答案$$ans=(t+1)f(t)-\sum_{i=1}^tf(i)$$
迄今为止，得到了$O(tn^2)$的算法，着重考虑少计算一些$f$值。
$f$值想必会有一点特殊性质，从最特殊的情况着手，考虑$n=1$，这时$f(x)=(2x+1)^2$，是一个二次函数，会不会$f$是一个二次函数？
这句话有一定道理，但存在漏洞，应该是一个分段二次函数，这样考虑该结论的正确性：
每轮变化相当于在图形外表面再**镶上一圈**，**在正方形相交情况不改变时，呈现的就是二次函数**，现在只用找出所有相交点，具体来说是$\lfloor\frac{x_i-x_j+1}{2}\rfloor$和$\lfloor\frac{y_i-y_j+1}{2}\rfloor$，一共有$O(n^2)$个
**$k+1$个不共线的点必能确定唯一的$k$次函数**，由于每一段是二次函数，去每一段的前三对数，运用拉格朗日插值即可得到二次函数，运用平方和公式求部分和即可。
总时间复杂度$O(n^3\log n)-O(n^4)$（根据写法不同有差异）

-----------------------------

# [codeforces1548C](https://codeforces.com/problemset/1548/C)
##**description**
对于所有$1\leq x\leq3n$求出$$\sum_{i=1}^n\binom{3i}{x}$$
$1\leq n \leq 10^6$
##**solution1**##
尝试从$x-1$向$x$递推，记所求为$f_x$
$$f_x=f_{x-1}+\sum_{i=1}^n\binom{3i-1}{x}$$
然后发现有新的式子，再设$$g_x=\sum_{i=1}^n\binom{3i-1}{x}, h_x=\sum_{i=1}^n\binom{3i-2}{x}, d_x=\sum_{i=1}^n\binom{3i-3}{x}$$
则有
$$ f_x=f_{x-1}+g_{x}\\
    g_x=g_{x-1}+h_{x}\\
    h_x=h_{x-1}+d_{x}\\
    d_x=f_x-\binom{3n}{x} $$
消去$d_x$后，发现三个方程线性相关，方程个数还不够
再观察上述定义，$$f_x+g_x+h_x=\sum_{i=1}^{3n}\binom{i}{x}=\binom{3n+1}{x+1}$$
方程个数足够，直接解即可(本题卡常，不要傻乎乎套$cramer's\ rule$)
上述解法关键在于大胆**设出几种式子，寻找递推关系**,并仔细分析**式子之间的关系**(最后的$f+g+h$考验全局意识)

##**solution2**##
> 考虑生成函数，所求即为$$[x^d]\sum_{i=1}^n(1+x)^{3i}$$
补上常数项$1$，$$f_d=[x^d]\sum_{i=0}^n(1+x)^{3i}=[x^d]\frac{1-(1+x)^{3n+3}}{1-(1+x)^3}$$
直接进行大除法，由于分母次数仅有三，除法的复杂度为$O(n)$
该解法指明**正确分析特殊情况的算法复杂度**的关键性，需要结合实际题目选择合适做法，不能够因为常规多项式除法是$O(n^2)$而不往该方向靠拢
**适度使用生成函数**可能可以简化问题，但不能成为呆板的生成函数选手，一定要**结合其他技巧使用**

-----------------------------
# [codeforces1548D1/2](https://codeforces.com/problemset/1548/D2)
##**description**
有$n$个点，保证无三点共线，求有多少个三角形，面积为正整数且内格点数目为奇数。
Easy : $\forall x, y \equiv 0 \mod 2$
$1\leq n\leq6000$

##**solution**##
有面积，内格点的限制，很容易想到$pick$定理
 下述记$S$为面积，$B$为外格点，$I$为内格点，$$2S=B+2I-2$$
 不妨$I=2k+1$，则$2S=4k+B$，那么$B$必为偶数，$2S\equiv B \ (\mod 4)$
一个向量穿过的格点数为$\gcd(x, y)$，$B = \gcd(x3-x1, y3-y1)+\gcd(x2-x1,y2-y1)+\gcd(x3-x2,y3-y2)$ 
先考虑Easy:
各点坐标偶数，$B$的表达式可知$B$必为偶数，且$B$的值只与$x \mod 4, y \mod 4$有关，略加分讨即可
Easy -> Hard，难点主要在:
1.$B$不仅与坐标模4相关，还与具体坐标相关(比如$\gcd(3, 9)=3, \gcd(3,1)=1$)
2.如果直接算面积，叉积完了还有绝对值，而$x\equiv|x| \ \mod 4$不一定成立
抓住$B$偶数的性质继续分析，三条边上点数为奇奇奇或偶偶奇，至少一边为奇数，那一条边上的**两点横纵坐标奇偶性相同**

WLOG，这两点(P, Q)为$ee$，剩余的一个点为$ee/(eo/oe/oo)$。
模4意义下，$x_R-x_P=x_Q-x_P,y_R-y_P =y_R - y_Q$， 因此**$2S$必为偶数**，上面的难点2消除
难点1只需在枚举$R$点之外，多枚举$RP, RQ, PQ$边上的点数mod4
枚举时要注意枚举的两个元素在同一个桶里时答案增多$\binom{c}{2}$

有时限制条件很多，直接做不可行，可以考虑**在状态中增加几维，多枚举一些限制条件**

-----------------------------
# [codeforces1620F](https://codeforces.com/problemset/1620/F)
##**description**
给定一个长度为 $n$ 的排列 $p_i$ , 可以使每个位置加负号或不加，构造任意一组解，使得 $p$ 的最长下降子序列不超过 $2$。
$n \leq 10 ^ 6$

##**solution**##
记长度为 $2$ 的最长下降子序列末尾为 $y$，长度为 $1$ 的最长下降子序列末尾为 $x$
考虑新加入一个数 $z$
1. $z > x$, $(x, y) \rightarrow (z, y)$
2. $z > y$, $(x, y) \rightarrow (x, z)$
3. $z < y$, 不合法
直接 dp 转移是 $O(n ^ 3)$。
考虑一个常见的优化，dp 数组不记 0/1，而是记录满足条件的最值。
在此处即为 $f_{i, x}$ 为 $y$ 的最小值，优化到平方。
还会发现，合法的 $z$ 一定会在结果中的 $x$ 处或 $y$ 处出现。
用一个布尔变量记录 $p_{i-1}$ 的出现位置，另一个布尔变量记录 $p_{i-1}$ 的符号，简单转移即可。
时间复杂度 $O(n)$。

##**reflection**
1. dp 中一个常见的优化，数组不用布尔，而是记录满足条件的最值，使状态蕴含信息增加。

2. dp 时要注意状态的特殊性质，例如本题 $p_{i-1}$ 一定出现在某个位置。

-------------------
# [codeforces1617E](https://codeforces.com/problemset/1617/E)
##**description**
定义操作 $x$ 为选择一个 $k$ 满足 $2 ^ k \geq x$，把 $x$ 变成 $2 ^ k - x$
定义 $x$ 与 $y$ 的距离为最少操作 $x$ 次数使得 $x = y$
给定序列，求 $\max(dis(a_i, a_j))$
$n \leq 10 ^ 5$, $a_i \leq 10 ^ 9$

##**solution**##
首先考虑距离的计算。
在 $x$ 与 $2 ^ k - x$ 之间连边，通过小数据尝试，会发现这是一个树的结构，证明大致如下：
对于一个 $x$ ，它与比他小的数只有一条边，就是选出最小的 $k$ 进行操作，如果 $k' > k$ 满足，$2 ^ {k'} \geq 2 ^ {k + 1} \gt 2x$， 矛盾。
$dis(x, y)$ 即是该树上两点距离。
还可以发现树上 $fa_x$ 的二进制最高位严格小于 $x$，树高只有 $\log n$。
于是可以枚举每个 $a_i$ 到根上的所有点，尝试作为 LCA，在其不同子树中找两个点。

##**reflection**
1. 和为二的次幂的数连边，构成一棵树。
2. 一些与数的性质相关的变换、关系等，可以从图论的角度考虑问题，看看是否可以通过图论手段转化问题。

-------------------