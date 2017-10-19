#2017-2018 ACM-ICPC NEERC H. Load Testing

######time limit per test: 1second

Polycarp plans to conduct a load testing of its new project Fakebook. He already agreed with his friends that at certain points in time they will send requests to Fakebook. The load testing will last $n$ minutes and in the $i$-th minute friends will send $a_i$ requests.

Polycarp plans to test Fakebook under a special kind of load. In case the information about Fakebook gets into the mass media, Polycarp hopes for a monotone increase of the load, followed by a monotone decrease of the interest to the service. Polycarp wants to test this form of load.

Your task is to determine how many requests Polycarp must add so that before some moment the load on the server strictly increases and after that moment strictly decreases. Both the increasing part and the decreasing part can be empty (i. e. absent). The decrease should immediately follow the increase. In particular, the load with two equal neigbouring values is unacceptable.

For example, if the load is described with one of the arrays [1, 2, 8, 4, 3], [1, 3, 5] or [10], then such load satisfies Polycarp (in each of the cases there is an increasing part, immediately followed with a decreasing part). If the load is described with one of the arrays [1, 2, 2, 1], [2, 1, 2] or [10, 10], then such load does not satisfy Polycarp.

Help Polycarp to make the minimum number of additional requests, so that the resulting load satisfies Polycarp. He can make any number of additional requests at any minute from 1 to $n$.

####Input

The first line contains a single integer $n$ $(1 ≤ n ≤ 100 000)$ — the duration of the load testing.

The second line contains $n$ integers $a_1$, $a_2$, ...,$ a_n$ $(1 ≤ a_i ≤ 10^9)$, where $a_i$ is the number of requests from friends in the $i$-th minute of the load testing.

####Output

Print the minimum number of additional requests from Polycarp that would make the load strictly increasing in the beginning and then strictly decreasing afterwards.

####Sample input

```
5
1 4 3 2 5
```

####Sample output

```
6
```

####Sample input

```
5
1 2 2 2 1
```

####Sample output

```
1
```

####Sample input

```
7
10 20 40 50 70 90 30
```

####Sample output

```
0
```

####Note

In the first example Polycarp must make two additional requests in the third minute and four additional requests in the fourth minute. So the resulting load will look like: [1, 4, 5, 6, 5]. In total, Polycarp will make 6 additional requests.

In the second example it is enough to make one additional request in the third minute, so the answer is 1.

In the third example the load already satisfies all conditions described in the statement, so the answer is 0.

## Answer

假设左右数字的高度为$h_l$和$h_r$，且$h_l<h_r$，现在我们向中间拓展，如果我们先拓展$h_r$，那么$h_{r-1}$必须大于等于$h_r+1$，而$h_{l+1}$也一定大于等于$h_l+1$；如果我们先拓展$h_l$，那么$h_{l+1}$必须大于等于$h_l+1$，而$h_{r-1}$不一定大于等于$h_r+1$，因为$h_r$可能为顶点。那么我们可以得出贪心策略，每次都从较小的一边想中心拓展。

```c++
//Author:CookiC
//#include"stdafx.h"
#include<iostream>
#define LL long long
#define maxn 100010
//#pragma warning(disable : 4996)
using namespace std;

int n;
LL ans;
LL a[maxn];

int main(){
//	freopen("test.in","r",stdin);
//	freopen("test.out","w",stdout);
	ios::sync_with_stdio(false);
	
	int i;
	LL t;
	cin>>n;
	for(i=0;i<n;++i)
		cin>>a[i];
	
	int L=0,R=n-1;
	ans=0;
	while(L<R){
		while(L<R&&a[L]<=a[R]){
			t=max(a[L]+1,a[L+1]);
			ans+=t-a[L+1];
			a[++L]=t;
		}
		while(L<R&&a[L]>a[R]){
			t=max(a[R]+1,a[R-1]);
			ans+=t-a[R-1];
			a[--R]=t;
		}
	}
	cout<<ans<<endl;
	return 0;
}
```

