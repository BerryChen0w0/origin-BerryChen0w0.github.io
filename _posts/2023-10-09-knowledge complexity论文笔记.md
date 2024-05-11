这一篇是knowledge complexity论文的笔记，比较草率，没有整理。

# 补充知识

## Quadratic redisue/quadratic nonresidue 二次剩余

给定整数$n$，对一个整数$q$，若$q$和某个完全平方数模$n$同余，即存在整数$x$满足$x^2\equiv q\mod n$，则称$q$是模$n$的**二次剩余(quadratic residue)**。否则，是**二次非剩余(quadratic nonresidue)**。

## QRP(Quadratic Residuosity Problem) 二次剩余问题

给定整数$a$和$N$，判定$a$是否是$N$的二次剩余。其中$N=p_1p_2$是两个未知素数的乘积，$a$是一个并不显然为二次非剩余数的整数。

# 文章

## chap1

文章之所以产生，思想是从NP出发，希望给多项式时间证明系统一个更general更natural的定义。

文中证明系统仍然是V有多项式时间算力，P有无穷算力

不考虑包含非确定多项式时间算法(non-deterministic poly time algorithm)和概率多项式时间算法(probabilistic poly time algorithm)的语言【因为它们中很少有interaction操作】，而考虑有交互式证明系统的语言。不过文章并没有给出一个具体的这样的语言，而是说按照本文对interactive proof system的定义【注：实际上是本文的早期版本对interactive proof system的定义。我现在看的这篇是1989年发表的，之前84年有一版投稿未发表的，85年有一版发表了的（这个历史也值得搞搞清楚）】，[GMW]文章的graph nonisomorphism语言就含有一个交互式证明系统；[BS]论文中的Auther-Merlin证明系统本身就是交互式证明系统；[GS]的论文甚至证明了一个语言含有interactive proof system当且仅当它含有Auther-Merlin proof system。

## chap2 interactive proof systems

这里要给相对正式的定义。【下面两行是之前的笔记。后面是之后补充的较为完整的笔记】

> Verifier变成概率多项式时间的了。

> Arther-Merlin Game是interactive proof system的子集，它要求V每次发送随机串s（0/1串）给P，而interactive proof system的V可以使用一个多项式时间函数f，每次发送f(s)给P。

### 2.1 interactive Turing machines and protocols

#### interactive Turing machine(ITM)

是一个图灵机，它有一个只读输入带(read-only input tape)，一个工作带(work tape)，一个随机带(random tape)，一个只读通信带(read-only communication tape)，一个只写通信带(write-only communication tape)。【后面用RO/WO表示read/write only，comm.表示communication】random tape包含无限长的随机比特，只能从左往右scan。称ITM读取random tape上的下一位随机比特为flips a coin（抛硬币）

#### interactive protocol

是一个有序ITM对A,B【原文中是A,B，实际上A是证明者，B是验证者，为表述更清晰，后面在无歧义的情况下我都用P,V来代替】。P和V共用一个input tape，它们的RO/WO comm. tape分别是对方的WO/RO comm. tape。P拥有无限的计算能力，V只有poly(input的长度)的计算时间。

两个ITM交替运行，V先运行。每次P/V运行时，先用它的input,work,comm.,random tape做一些计算，然后把一个string写到它的WO comm. tape上。然后换另一位运行（自己被deactive，另一位被active）。P/V的第i条消息(i-th message)就是它在第i次激活时(i-th active stage)写在自己的WO comm.tape上的string。任意一方可以停止协议的计算过程(the computation of the protocol)，只要它在自己的某个active stage不写任何message到WO comm.tape即可。最后V输入accept/reject（分别表示接受/拒绝input）结束协议。V的运算时间是它所有激活阶段的运算时间之和。

### 2.2 interactive proof systems

令$L$是$\{0,1\}^*$上的语言，$(A,B)$是interactive protocol。称$(A,B)$是$L$的interactive proof system：若

(1) 对任意$k$，对足够长的$x\in L$作为$(A,B)$的输入，B停机且以至少$1-|x|^{-k}$的概率接受【任意的k，那就是说不接受的概率是可忽略函数呗？】

(2) 对任意$k$，对足够长的$x\notin L$，对任意ITM $A'$，$x$作为$(A',B)$的输入，B至多以$|x|^{-k}$的概率接受【就是说接受的概率是可忽略函数？】

> remark 1.错误的概率$|x|^{-k}$可以减得更小，例如可以小于$2^{-|x|}$，只要重复多次这个协议然后以多数次数的结果决定是否接受。

#### 还剩一些，下次再整吧【IP的定义，以及Arthur-Merlin game】

## chap3 zero-knowledge

这一章给出各种形式化定义，最终形式化地定义zero-knowledge

### 3.1 indistinguashability of random variables

#### U={U(x)}, V={V(x)}

> 定义在3.1 indistinguashability of random variables里【6/23下面】

是两组随机变量(family of random variables)。这里$x\in L$，$L\subseteq\{0,1\}^*$是一个语言(language)。所有随机变量$U(x),V(x)$也都在$\{0,1\}^*$的范围内取值

> 理解：是一个函数，把一个字符串$x\in \{0,1\}^*$映射到一个随机变量$U(x)$。每个x对应一个U(x)，U(x)是一个随机变量；U是所有x对应的U(x)组成的随机变量的集合。

希望实现的是：当$|x|$足够大时，$U(x)$和$V(x)$两个随机变量的分布几乎一致【把U(x)或V(x)的一个随机采样(random sample)交给一个"judge"，judge输出0或1，分别表示判定这个random sample是来自U(x)还是来自V(x)。则U(x)和V(x)不可区分(U(x) is "replaceable" by V(x))就是说，任意一个judge的判定(0或1)的分布都和采样的分布不相关(uncorrelated)

#### Statistical Indistinguashability

令$L\subset \{0,1\}^*$是语言，两组随机变量$\{U(x)\}, \{V(x)\}$在$L$上statistically indistinguashable：若对于任意常数$c>0$和任意足够长的$x\in L$，有：
$$
\sum_{\alpha\in\{0,1\}^*}|P(U(x)=\alpha)-P(V(x)=\alpha)| < |x|^{-c}
$$

> 理解：
>
> U(x)和V(x)都在$\{0,1\}^*$中取值，左边是对每一个可能取值，两个随机变量的概率的差值。
>
> 注意U(x)和V(x)是跟x有关的。每一个x，都对应两个随机变量U(x)和V(x)。上式要求对长度达到一定阈值的任意x（它们分别对应不同的(U(x),V(x))对）都成立

**例子（192页example 2）**：U(x)对所有长度为|x|的字符串赋相同的取值概率【不妨记$l=|x|$。这个U(x)定义就是说，共有$2^l$个长度为$l$的串，所以U(x)对每个长度为$l$的串取值概率是$2^{-l}$，而长度不等于$l$的串取值概率是0】。V(x)对所有长度为$|x|$的字符串赋概率值$2^{-l}$，但$\overbrace{0..0}^{l个}$和$\overbrace{1..1}^{l个}$除外，它们分别赋概率0和$2^{-l+1}$。则U(x)和V(x)就是在$\{0,1\}^*$上statistical indistinguashable的

> $$
> \begin{align}
> 2^{-l}+2^{-l+1} &\stackrel{?}{<} l^{-c} \\
> \frac{3}{2^l} &\stackrel{?}{<} \frac{1}{l^c}
> \end{align}
> $$
>
> 当$l$足够大时，一定是成立的（左边指数级，右边多项式级）

#### poly-size family of circuit

一组布尔电路$C=\{C_x\}$，其中每个布尔电路$C_x$，有一个布尔输出。存在某个常数$e>0$，使得任意$C_x\in C$至多有$|x|^e$个门(gate)

#### poly-bounded families of random variables

对给定常数$d>0$，所有随机变量$U(x)\in U$满足：$P(U(x)=\alpha)\neq 0$当且仅当$|\alpha|=|x|^d$

#### P(U,C,x)

令$U=\{U(x)\}$是poly-bounded family of random variables，$C=\{C_x\}$是poly-size family of circuit。$P(U,C,x)$表示当输入一个服从$U(x)$分布的随机字符串时，$C(x)$输出1的概率。【假定U(x)赋正概率值的那些字符串的长度（即$|x|^d$）等于$C_x$的输入的数量】

#### Computational Indistinguashability

令$L\subset \{0,1\}^*$是语言，两组poly-bounded随机变量$\{U(x)\}, \{V(x)\}$在$L$上computationally indistinguashable：若对于任意poly-size电路组$C=\{C_x\}$，任意常数$c>0$和任意足够长的$x\in L$，有：
$$
|P(U,C,x)-P(V,C,x)|<|x|^{-c}
$$
后面称indistinguashable意思就是computational indist。【作者认为这里的comp. indist.定义是具有一般性的】

> 注(P193 remark 2)：这里comp.indist.定义只输入一个样本（P(U,C,x)函数是只输入一个随机样本的）。事实上按照这里的comp.indist.定义，满足comp.indist.当且仅当：当$C_x$接收了poly(|X|)【我用poly(x)表示关于x的多项式量级】这么多的输入字符串（每个字符串都按照U(x)的分布独立选取），其输出1的概率(propability)，与接收同样多的按照V(x)分布独立选取的字符串时，输出1的概率，几乎一致(be close to)【我理解文中的be close to应该就是差别是可忽略函数那种级别的意思吧。其实前面的stastical indist.和comp.indist.定义都要求差别小于|X|的多项式量级，其实也就是差别是可忽略函数的意思吧】

### 3.2 approximability of random variables随机变量的可逼近性/可近似性

现在形式化这样一个问题：一个随机变量U是基本上容易生成的(essentially easy to generate)。也就是说，存在一个高效的算法，它能够以和U这个分布indist.的分布随机输出字符串。

#### M(x)

令M是概率图灵机，且它输入$x$时必定停机(with propability 1)。定义$M(x)$是这样的随机变量：$p(M(x)=\alpha)=p(M输入x时输出\alpha)$

#### perfectly approximable

令$L\subset \{0,1\}^*$是语言，一组随机变量$U=\{U(x)\}$在$L$上perfectly approximable：若存在在期望多项式时间(running in expected poly. time)内运行的概率图灵机$M$，对任意$x\in L$，$M(x)=U(x)$。

#### statistically/computationally approximable

令$L\subset \{0,1\}^*$是语言，一组随机变量$U=\{U(x)\}$在$L$上statistically/computationally approximable：若存在在期望多项式时间(running in expected poly. time)内运行的概率图灵机$M$，随机变量组$\{M(x)\}$和$\{U(x)\}$在$L$上stat.indist./comp.indist.。

后面称approximable(我简记为appro.)意思就是comp.appro.

### 3.3 zero-knowledge protocols and proof systems

首先解决“cheating verifier" $B'$。它可以不遵守协议。

#### cheating verifier B'

令(A,B)为interactive protocol，B'是这样的ITM：输入x，另外有一条额外的input tape $H$。$H$的长度限定为poly(|X|)的。A和B'交互时，A只能看到输入x，而B'能看到(x,H)。我们假设B‘和A交互时的总计算用时不超过poly(|x|)

#### view of B'

（view of B'就是B'能看到的所有东西。）令$\sigma$和$\rho$分别是A和B'的random tape上的string（应该是只包括已经使用过的那部分随机比特）。假设A和B'进行了n轮交互，第i轮中A和B'交互的message是$b_i,a_i$。则$(\rho,b_1,a_1,...,b_n,a_n)$是在输入x和H时B'的view(view of B')。定义$View_{A,B'}(x,H)$为随机变量，其可能的取值就是这个view。【这个view中有随机值$\rho$，所以即使对相同的x,H，view也可能不同。这个随机变量的可能取值应该是在x,H输入所有下可能的view】

方便起见，考虑每个view都是$\{0,1\}^*$中的string且长度恰好为$|x|^c$，这里$c>0$是某定值。

#### perfect/statistical/computational zero-knowledge

令$L\subset \{0,1\}^*$是语言，$(A,B)$是protocol。B'如上定义。称$(A,B)$在$L$上对B'是perfect/stat./comp. zk的：若随机变量组$View_{A,B'}$在$L'$上是perfect/stat./comp. appro.的
$$
L'=\{(x,H)|x\in L, |H|=|x|^c\}
$$
称$(A,B)$在$L$上是perfect/stat./comp. zk的：如果(A,B)在L上对任意多项式时间ITM B'都是perfect/stat./comp. zk的。

此后称zero-knowledge就指comp.zk【comp.zk是上述定义中最general的】

#### perfect/stat./comp. zk proof system

称(A,B)是L上的perfecr/stat./comp. zk proof system：若(A,B)是L的interactive proof system，且(A,B)是L上的zk protocol

此后称zk proof system就指comp.zkp system【同前，comp.是上述定义中最general的】

### 3.4 ....

### 3.5 examples of zk languages

BPP中的所有语言都有perfect zkp system【BPP类！了解一下！】

> BPP复杂性类：https://zh.wikipedia.org/zh-hans/BPP_(%E8%A4%87%E9%9B%9C%E5%BA%A6)

二次剩余语言QR的perfect zkp system。。。

## chap4 the quadratic residuosity problem

【论文中二次剩余的定义】

**Fact1**. 令$x\in N,y\in Z_x^*$，$y$是模$x$的二次剩余 **iff.**  对$x$的任意素因子$p$，$y$是模$p$的二次剩余

# 思考

1. statistical indist和computational indist区别是什么？
