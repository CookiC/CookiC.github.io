# ECR 30 B. Balanced Substring

######time limit per test: 1second

You are given a string $s$ consisting only of characters 0 and 1. A substring $[l, r]$ of $s$ is a string $s_ls_{l+ 1}s_{l+2}... s_r$, and its length equals to $r-l+1$. A substring is called balanced if the number of zeroes (0) equals to the number of ones in this substring.

You have to determine the length of the longest balanced substring of $s$.

####Input

The first line contains $n (1 ≤ n ≤ 100000)$ — the number of characters in $s$.

The second line contains a string $s$ consisting of exactly $n$ characters. Only characters 0 and 1 can appear in $s$.

####Output

If there is no non-empty balanced substring in *s*, print 0. Otherwise, print the length of the longest balanced substring.

####Sample input

```
8
11010111
```

####Sample output

```
4
```

####Sample input

```
3
111
```

####Sample output

```
0
```

####Note

In the first example you can choose the substring [3, 6]. It is balanced, and its length is 4. Choosing the substring [2, 5] is also possible.

In the second example it's impossible to find a non-empty balanced substring.

## Answer

用DP计算$[1,i]$的1与0的数量差$a_i$，并记录下值不同的$a_i$出现的最后一个位置，由于当$a_i=a_j$时，区间$[a_{i+1},a_j]$的1与0的数量必定是相等的，所以对于任意$a_i$，只要查询和$a_i$值相等的最后一个$a_j$出现的位置即是从$i+1$开始的满足条件的最长子串的位置。

```c++
//Author:CookiC
//#include"stdafx.h"
#include<iostream>
#include<map>
#define LL long long
#define maxn 100010
//#pragma warning(disable : 4996)
using namespace std;

int n;
int a[maxn];
char s[maxn];
map<int,int> last;

int main(){
//	freopen("test.in","r",stdin);
//	freopen("test.out","w",stdout);
	ios::sync_with_stdio(false);
	
	int i,j;
	cin>>n>>s+1;
	a[0]=0;
	for(i=1;i<=n;++i){
		a[i]=a[i-1]+(s[i]=='1'?1:-1);
		last[a[i]]=i;
	}
	
	int ans=0;
	for(i=0;i<n;++i)
		ans=max(ans,last[a[i]]-i);
	cout<<ans<<endl;
	return 0;
}
```

