---
title: atcoder
mathjax: true
date: 2021-12-29 22:04:07
tags:
    - problems
    - atc
categories:
    - 题目
    - atc
top: 2
---
# atcoder

标签（空格分隔）： problems

---

# [agc021b](https://vjudge.net/problem/AtCoder-agc021_b)
## **description**
有 $n$ 个点，在足够大的圆内随机撒一点， $\forall i \in [1, n]$ ，求出该点与 $a_i$ 欧氏距离最小的概率。
$x, y \in [0, 10 ^ 6]$

## **solution**##
本题最重要的想法是 **近似处理**， 在解题中反复运用。
记点集的凸包为 $P$，考虑两种情况：
1.$x$ 在凸包内部，由于 $P$ 很小，概率近似为 $0$；

2.$x$在凸包上，记$P$上与$x$相邻的点 $p, q$，撒点区域可以被近似成$x$为圆心的足够大的圆，$dis(x, p), dis(x, q)$近似为0。  有效区域在$xp,xq$中垂线之间，$p = \frac{\angle PXQ-\frac{\pi}{2}*2}{2\pi}$ 。

利用精度限制做近似处理可能会成为有效手段。

---
# [agc021c](https://vjudge.net/problem/AtCoder-agc021_c)
## **description**
$n*m$网格，填入 $1*2$ 骨牌 $a$ 张, $2*1$ 骨牌 $b$ 张，判定合法并构造。

## **solution**##
先讨论简单无解：
1. $2(a+b) > n * m$
2. $a > n * \lfloor \frac{m}{2} \rfloor $
3. $b > m * \lfloor \frac{n}{2} \rfloor $

除此之外还有无解，从简单情况着手：

**1.$n, m$为偶数**
划成 $\frac{n}{2} * \frac{m}{2}$ 个方块，每块内两横或两竖，该方法失效当且仅当 $a + b = n * m / 2$ 且 $b$ 为奇数，失效时手算发现无解。

无解理由：奇数编号的行染成红色，横骨牌占据偶数个红格，竖骨牌占据奇数个红格，而总红格为偶数，矛盾！

另外的整体方法类似，细节还需注意。

**2.$n$，$m$为奇偶性不同**
方法与 $1$ 相同，只不过提前把多出的一行先覆盖

**3.$n, m$为奇数**
此时情况有变，上述的无解情况反倒有解了，具体见下图
![1.png](https://i.loli.net/2021/08/19/GukELDlrBQfNwPU.png)
全部放置完成后，$3*3$中用上图调整即可。

## **reflection**
本题考验**分类讨论**能力与**复杂问题化归简单情况**的想法，首先排除大部分无解，小范围中在发掘无解必要条件，解决好一定有解的情况。在整体处理困难是，一定要划成小问题考虑。
实际做题不可能总是得到完备的无解理由，有效的分割问题规模更显重要。

---
# [agc021_d](https://vjudge.net/problem/AtCoder-agc021_d)
## **description**
允许改动字符串的$k$个字符，求新串的反串与新串lcs的最大值
$1 \leq k \leq n \leq 300$

## **solution**##
首先要分析 **新串的反串与新串lcs**。

靠感觉的话，新串的最长回文子序列长度(lps)应为所求，试图证明一下：

设 $lcs(s, rev(s))=m$， $lps \leq m$， 只需 $lps \geq m$ 。

则 $\exists a_1 < a_2 < \cdots < a_m, b_1 > b_2 > \cdots > b_m，s[a_i] = s[b_i]$ 。 

1. 若 $a_m < b_m$， $lps = 2m > m$
2. 否则必然存在 $a_w<b_w \wedge a_{w+1}>=b_{w+1}$

(1). $a_{w+1}>b_{w+1}$ : 此时 $a_1, \cdots, a_w, b_w, \cdots, b_1$ 与 $b_m, \cdots, b_{w+1}, a_{w+1}, \cdots, a_m$ 均为回文， 总长度 $2m$， 必得其一。

(2). $a_{w+1}=b_{w+1}$ : 此时 $a_1, \cdots, a_w, a_{w+1},b_w, \cdots, b_1$ 与 $b_m, \cdots, b_{w+2}, a_{w+1}, a_{w+2}, \cdots, a_m$ 均为回文，总长度$2m$，必得其一。
综上, $lps \geq m$, Q.E.D.
之后是简单区间 $dp$ 。

---




