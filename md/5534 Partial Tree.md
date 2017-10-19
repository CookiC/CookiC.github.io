# 5534 Partial Tree

###### time limit per test: 1 seconds

In mathematics, and more specifically in graph theory, a tree is an undirected graph in which any two nodes are connected by exactly one path. In other words, any connected graph without simple cycles is a tree.

You find a partial tree on the way home. This tree has $n$ nodes but lacks of $n−1$ edges. You want to complete this tree by adding $n−1$ edges. There must be exactly one path between any two nodes after adding. As you know, there are $n^{n−2}$ ways to complete this tree, and you want to make the completed tree as cool as possible. The coolness of a tree is the sum of coolness of its nodes. The coolness of a node is $f(d)$, where $f$ is a predefined function and $d$ is the degree of this node. What's the maximum coolness of the completed tree?

#### Input

The first line contains an integer $T$ indicating the total number of test cases.
Each test case starts with an integer $n$ in one line,
then one line with $n−1$ integers $f(1),f(2),…,f(n−1)$.

$1≤T≤2015$
$2≤n≤2015$
$0≤f(i)≤10000$
There are at most 10 test cases with $n>100$.

#### Output

Print a single integer: the minimum possible cost to make the list good.

#### Sample input

```
2
3
2 1
4
5 1 4
```

#### Sample output

```
5
19
```

## Answer

由于是个树，可以知道最后所有点的度数和是$2n-2$，但有一个限制条件，每个点至少得有一个度，所以在最初的时候给所有点先分配一个度，使一开始的时候，$ans=f(1)*n$，并使$f(n)=f(n)-f(1),n>1$，之后还剩下$n-2$个度，这就转化成了完全背包问题：有$n-2$种物品，物品$i$的大小为$i$，物品$i$的价值为$f(i)$，每种物品有无数个，放入容量为$n-2$的背包中，求解装入背包的物品最小总价值。

```c++
//Author:CookiC
//#include"stdafx.h"
#include<iostream>
#include<algorithm>
#define maxn 2020
#define maxf 10010
#define NINF 0x80000000
//#pragma warning(disable : 4996)
using namespace std;

int T,N,ans;
int f[maxn],dp[maxf];

int main(){
//	freopen("test.in","r",stdin);
//	freopen("test.out","w",stdout);
	ios::sync_with_stdio(false);
	
	int i,j;
	cin>>T;
	while(T--){
		cin>>N;
		for(i=0;i<N-1;++i)
			cin>>f[i];
		ans=f[0]*N;
		dp[0]=0;
		for(i=1;i<N-1;++i){
			f[i]-=f[0];
			dp[i]=NINF;
		}
		for(i=1;i<N-1;++i)
			for(j=i;j<N-1;++j)
				dp[j]=max(dp[j],dp[j-i]+f[i]);
		ans+=dp[N-2];
		cout<<ans<<endl;
	}
	return 0;
}
```

