## Codeforces 258D Little Elephant and Broken Sorting

*problem*:

> 数列A，按顺序给出M次交换操作(x, y)，每次操作有50%概率不执行。求最终逆序对数量的期望。

*solution*:

> probability[x, y]表示A[x] > A[y]的概率。对于每个(x, y)对所有i进行更新，并保证probability[x, y] = probability[y, x] = 0.5.

## ZOJ 3509 Island Communication

*problem*:

> 无向图，三种操作：加边，删边，查询两点是否连通。删过的边不会再加进来。$N \lt 500, M \lt 50000$.

*solution*:

> 维护图中不存在环即可保证dfs的复杂度在O(N)。每次加边时如果形成了环，就在环里找到之后最早被删去的边并把他干掉。

## Codeforces 235B Let's Play Osu!

*problem*:

> 连续进行N次操作，每次操作有prob[i]的概率画O，(1 - prob[i])的概率画X。对于操作完得到的序列，连续的x个O可以累加x^2的分数，序列总得分为每段连续的O的得分之和。求期望得分。$N \lt 100000$. 

*solution*:

> dp[i]表示序列的前i项的期望得分，则dp[i] = dp[i - 1] + g[i]，其中g[i]为当前第i位的得分。显然，这一位有prob[i]的概率是一段O的末尾，得分应累加prob[i] * 1。在此基础上，有prob[i] * prob[i - 1]的概率是以OO作为末尾，此时最后一位的价值为4 - 1 = 3，其中有1分已经在prob[i]的部分算过，因此累加得分是prob[i] * prob[i - 1] * 2。以此类推，可以得到$g[i] = prob[i] \times 1 + prob[i] \times prob[i - 1] \times 2 + \ldots + \prod_{j = 1}^{i} prob[j]$。这里g[i]可以从g[i - 1]推过来，总复杂度是O(n)。

## Codeforces 229E Gifts

*problem*:

> M种礼物，每种礼物若干个，有不同的价值，需要在所有礼物中选择N个出来。每选择一组中的s个，对方会在这一组中随机返回s个回来。选的时候采取最优策略，即选择一种取法，尽量使自己拿到最大的N个。如果有多种取法，在其中随机选择一种。问最终能拿到最大的N个礼物的概率。$N, M \lt 1000$.

*solution*:

> 注意到取法必然是这样子的：假设第N大的礼物价值是x，那么每一组当中所有大于x的都是必须取的，把个数记为must[i]。每一组中等于x的礼物是可选的，个数记为optional[i]。所有的optional[i]之和为total，我们需要在其中选择N - Sigma{must[i]}个出来。

> prob[i, j]表示前i组，已经取了j个optional的礼物的概率。很重要的一点是，已经取的optional的数量，会影响到当前这一组礼物是否取optional的概率。如果当前已经取了j个optional，那么当前取optional的概率为p = C[left - 1, total - j - 1] / C[left, total - j]，其中left是从当前组到最后一组还剩下的optional的数量。

> 转移的时候分两种情况：  

> 1. optional[i] = 0:  prob[i, j] = prob[i - 1, j] / C[size, must]
> 2. optional[i] != 0: prob[i, j] = prob[i - 1, j - 1] * p / C[size, must + 1] + prob[i - 1, j] * p0 / C[size, must]

> 其中p0是在prob[i - 1, j]下不取当前组的概率。如果采用顺推的方法，应该可以避免p0 != 1 - p的这个问题，不过prob[i, j]的含义就应该变化为ij状态下，当前组还没取的概率。总复杂度是O(NM).

## POJ 2279 Mr. Young's Picture Permutations

*problem*:

> 给定一个矩阵，其中每一行的前X[i]个格子可以用来填数字。设一共有N个格子合法，现要将1到N填进去，且满足矩阵在横向和纵向都满足单调递增。求方案数。

*solution*:

> 钩子公式。先求出以每个格子作为钩子顶点的钩子长度，也即每个格子右方和下方的累计长度$A[i, j]$，则答案为$\dfrac{N!}{\sum A[i, j]}$。

## SOJ 1898 Tree

*problem*:

> 给定黑白树，每次操作可以交换树上的任意两个点，求把所有黑点连成一个连通块的最少步数。$N < 100$.

*solution*:

> 注意到用$T_i$子树下$j$个黑点的最少步数作为状态来dp是不可行的，因为状态不具有转移性，也就是说，将所有点转移到子节点，再从子节点拿一个交换到根并非最佳策略。但是可以发现在转移的过程中，在当前状态下不动的点可以累加。因此设计状态$dp[i, j]$为在$T_i$子树中，构造$j$个黑点连通块，其中不需要交换的最多的黑点数。于是有

>> $dp[T_i, j] = \max\{dp[T_x, k] + dp[T_i - T_x, j - k]\}$

> 初始状态下对于所有点来说，如果当前点是黑点，则$dp[i, 1] = 1$，否则如果他的后代中有黑点，则$dp[i, 1] = 0$。最后答案为$total - \max\{dp[i, total]\}$，其中$total$为树中所有黑点的个数。

## USACO March 2008 Gold, Land Acquisition

*problem*:

> N块土地，每块的长为$wide[i]$，宽为$height[i]$，如果购买其中的若干块土地，则花费为$\max\{wide[i]\} \times \max\{height[i]\}$。求把所有土地买下来的最小花费。$N \leq 50000$。

*solution*:

> 将所有土地按照$wide$降序排列。显然对于土地$i$来说，如果存在土地$j$满足$wide[i] \lt wide[j]$且$height[i] \lt height[j]$，那么他们肯定可以合并在一起买，可以先把这样的土地给删掉。接下来的DP就很显然了：由于排序后$wide$单调减，$height$单调增，因此土地必然是连续的一段来买。$f[i]$表示购买前i块土地的最小花费，有$f[i] = \max\{f[j] + wide[j + 1] \times height[i]\}$。注意到$wide[j + 1] \times height[i]\}$满足四边形不等式$w[i, j] + w[i + 1, j + 1] \leq w[i + 1, j] + w[i, j + 1]$，因此dp具有决策单调性。用一个栈维护当前决策所覆盖的区间$[start, end]$，每次加入一个新的决策，就判断在栈顶覆盖的$start$位置处当前决策是否更优。如果是，将栈顶弹出；否则对栈顶的区间进行二分，找到最小的当前决策最优的位置$x$，将栈顶区间拆成$[start, x]$并插入新的决策覆盖区间$[x, n]$。总复杂度是$O(NlogN)$。

## USACO February 2009 Gold, Stock

*problem*:

> 给定S只股票在D天内每天的价位，初始手上的钱为M，求D天之后把股票全部套现能拿到的最多现金。$S < 50, D < 10$，答案不超过$5 \times 10^5$。

*solution*:

> 对于每只股票，都可以看作只买一天。如果一只股票持续多天，可以看作在第一天买，第二天卖出，之后在第二天重新买进。将问题转化后，对于每一天来说，问题变成了，每只股票的价格为当前天的价格，获利为下一天的价格与当前的价格差，则问题转化为一个无限背包。复杂度为$O(NMS)$。

## ZOJ 3512 Financial Fraud

*problem*:

> 给定序列A，求单调不减的序列B，满足$|A_1 - B_1| + |A_2 - B_2| + \ldots + |A_n - B_n|$最小。$N \leq 50000$。

*solution*:

> 对于一段连续递减的序列，全部变成中位数是最好的选择。考虑当前已经计算出了序列前$i$位的答案，其中前$i$位被划分成了若干段连续相同的B序列：$(B_1, B_1, \ldots, B_1), \ldots, (B_m, B_m, \ldots, B_m)$。对于第$(i + 1)$位来说，如果他大于$B_m$，则单独划为一组；否则跟前一组$B_m$合并，求出$B_m$这一组控制下的A序列和$A_{i+1}$的中位数作为新的$B_m$，并一直往前合并。实现时需要维护序列的中位数（即前k小）并支持合并操作，因此使用左偏树来维护每一组的B序列。