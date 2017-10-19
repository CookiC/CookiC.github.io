# ECR 29 D. Yet Another Array Queries Problem

###### time limit per test: 2 seconds

You are given an array $a$ of size $n$, and $q$ queries to it. There are queries of two types:

- 1 $l_i$ $r_i$ — perform a cyclic shift of the segment $[l_i, r_i]$ to the right. That is, for every $x$ such that $l_i ≤ x < r_i$ new value of $a_{x + 1}$ becomes equal to old value of $a_x$, and new value of $a_{l_i}$ becomes equal to old value of $a_{r_i}$;
- 2 $l_i$ $r_i$ — reverse the segment $[l_i, r_i]$.

There are $m$ important indices in the array $b_1, b_2, ..., b_m$. For each $i$ such that $1 ≤ i ≤ m$ you have to output the number that will have index $b_i$ in the array after all queries are performed.

####Input

The first line contains three integer numbers $n$, $q$ and $m$ $(1 ≤ n, q ≤ 2·10^5, 1 ≤ m ≤ 100)$.

The second line contains $n$ integer numbers $a_1, a_2, ..., a_n$ $(1 ≤ a_i ≤ 10^9)$.

Then $q$ lines follow. $i$-th of them contains three integer numbers $t_i, l_i, r_i$, where $t_i$ is the type of $i$-th query, and $[l_i, r_i]$ is the segment where this query is performed $(1 ≤ t_i ≤ 2, 1 ≤ l_i ≤ r_i ≤ n)$.

The last line contains $m$ integer numbers $b_1, b_2, ..., b_m$ $(1 ≤ b_i ≤ n)$ — important indices of the array.

####Output

Print $m$ numbers, $i$-th of which is equal to the number at index $b_i$ after all queries are done.

####Sample input

```
6 3 5
1 2 3 4 5 6
2 1 3
2 3 6
1 1 6
2 2 1 5 3
```

####Sample output

```
3 3 1 5 2 
```

## Answer

由于$m$的数据很小，我们就可以直接从$b_i$逆推出一开始在$a$中的坐标，时间复杂度是$O(qm)$。

```c++
//Author:CookiC
//#include"stdafx.h"
#include<iostream>
#define maxn 200010
#define maxm 110
//#pragma warning(disable : 4996)
using namespace std;

int n,q,m;
int a[maxn],t[maxn],l[maxn],r[maxn],b[maxm];

int main(){
//	freopen("test.in","r",stdin);
//	freopen("test.out","w",stdout);
	ios::sync_with_stdio(false);
	
	int i,j;
	cin>>n>>q>>m;
	for(i=0;i<n;++i)
		cin>>a[i];
	for(i=0;i<q;++i)
		cin>>t[i]>>l[i]>>r[i];
	for(i=0;i<m;++i)
		cin>>b[i];
	for(i=q-1;i>=0;--i)
		for(j=0;j<m;++j)
			if(l[i]<=b[j]&&b[j]<=r[i])
				if(t[i]==1)
					b[j]=b[j]-1>=l[i]?b[j]-1:r[i];
				else
					b[j]=r[i]+l[i]-b[j];
	
	for(i=0;i<m;++i)
		cout<<a[b[i]-1]<<' ';
	cout<<endl;
	return 0;
}
```

