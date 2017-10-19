#MSU Round 2 B. Ordering Pizza

######time limit per test: 2 seconds

It's another Start[c]up finals, and that means there is pizza to order for the onsite contestants. There are only 2 types of pizza (obviously not, but let's just pretend for the sake of the problem), and all pizzas contain exactly $S$ slices.

It is known that the *i*-th contestant will eat $s_i​$ slices of pizza, and gain $a_i​$ happiness for each slice of type 1 pizza they eat, and $b_i​$ happiness for each slice of type 2 pizza they eat. We can order any number of type 1 and type 2 pizzas, but we want to buy the minimum possible number of pizzas for all of the contestants to be able to eat their required number of slices. Given that restriction, what is the maximum possible total happiness that can be achieved?

####Input

The first line of input will contain integers $N$ and $S (1 ≤ N ≤ 10^5, 1 ≤ S ≤ 10^5)$, the number of contestants and the number of slices per pizza, respectively. *N* lines follow.

The $i$-th such line contains integers $s_i, a_i, and b_i (1 ≤ s_i ≤ 10^5, 1 ≤ a_i ≤ 10^5, 1 ≤ b_i ≤ 10^5)$, the number of slices the $i$-th contestant will eat, the happiness they will gain from each type 1 slice they eat, and the happiness they will gain from each type 2 slice they eat, respectively.

####Output

Print the maximum total happiness that can be achieved.

####Sample input

```
3 12
3 5 7
4 6 7
5 9 5
```

####Sample output

```
84
```

####Sample input

```
6 10
7 4 7
5 8 8
12 5 8
6 11 6
3 3 7
5 9 6
```

####Sample output

```
314
```

## Answer

简单的贪心题，分情况讨论。

```c++
//Author:CookiC
//#include"stdafx.h"
#include<iostream>
#include<vector>
#include<algorithm>
#define maxn 100010
#define LL long long
//#pragma warning(disable : 4996)
using namespace std;

int N,S;
vector<int> ai,bi;
LL s[maxn],a[maxn],b[maxn];

bool cmpA(const int i,const int j){
	return a[i]-b[i]<a[j]-b[j];
}

bool cmpB(const int i,const int j){
	return b[i]-a[i]<b[j]-a[j];
}

int main(){
//	freopen("test.in","r",stdin);
//	freopen("test.out","w",stdout);
	ios::sync_with_stdio(false);
	
	int i,j;
	LL A=0,B=0,C=0,ans=0;
	cin>>N>>S;
	for(i=0;i<N;++i){
		cin>>s[i]>>a[i]>>b[i];
		if(a[i]>b[i]){
			A+=s[i];
			ans+=s[i]*a[i];
			ai.push_back(i);
		}
		else if(a[i]<b[i]){
			B+=s[i];
			ans+=s[i]*b[i];
			bi.push_back(i);
		}
		else{
			C+=s[i];
			ans+=s[i]*a[i];
		}
	}

	A%=S;
	B%=S;
	if(A+B+C<=S){
		LL suma,sumb,t;
		suma=sumb=ans;
		sort(ai.begin(),ai.end(),cmpA);
		sort(bi.begin(),bi.end(),cmpB);
		for(i=0;A;++i){
			j=ai[i];
			t=min(A,s[j]);
			suma-=t*(a[j]-b[j]);
			A-=t;
		}
		for(i=0;B;++i){
			j=bi[i];
			t=min(B,s[j]);
			sumb-=t*(b[j]-a[j]);
			B-=t;
		}
		ans=max(suma,sumb);
	}
	cout<<ans<<endl;
	return 0;
}
```

