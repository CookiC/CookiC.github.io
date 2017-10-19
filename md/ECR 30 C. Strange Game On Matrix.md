# ECR 30 C. Strange Game On Matrix

######time limit per test: 1 second

Ivan is playing a strange game.

He has a matrix $a$ with $n$ rows and $m$ columns. Each element of the matrix is equal to either 0 or 1. Rows and columns are 1-indexed. Ivan can replace any number of ones in this matrix with zeroes. After that, his score in the game will be calculated as follows:

1. Initially Ivan's score is 0;
2. In each column, Ivan will find the topmost 1 (that is, if the current column is $j$, then he will find minimum $i$ such that $a_{i,j} = 1$). If there are no 1's in the column, this column is skipped;
3. Ivan will look at the next $min(k, n - i + 1)$ elements in this column (starting from the element he found) and count the number of 1's among these elements. This number will be added to his score.

Of course, Ivan wants to maximize his score in this strange game. Also he doesn't want to change many elements, so he will replace the minimum possible number of ones with zeroes. Help him to determine the maximum possible score he can get and the minimum possible number of replacements required to achieve that score.

####Input

The first line contains three integer numbers $n, m$ and $k (1 ≤ k ≤ n ≤ 100, 1 ≤ m ≤ 100)$.

Then *n* lines follow, *i*-th of them contains *m* integer numbers — the elements of *i*-th row of matrix *a*. Each number is either 0 or 1.

####Output

Print two numbers: the maximum possible score Ivan can get and the minimum number of replacements required to get this score.

####Sample input

```
4 3 2
0 1 0
1 0 1
0 1 0
1 1 1
```

####Sample output

```
4 1
```

####Sample input

```
3 2 1
1 0
0 1
0 0
```

####Sample output

```
2 0
```

####Note

In the first example Ivan will replace the element $a_{1, 2}$.

## Answer

two pointers.

```c++
//Author:CookiC
//#include"stdafx.h"
#include<iostream>
#define maxn 110
//#pragma warning(disable : 4996)
using namespace std;

int n,m,k;
bool a[maxn][maxn];
int dp[maxn][maxn];

int main(){
//	freopen("test.in","r",stdin);
//	freopen("test.out","w",stdout);
	ios::sync_with_stdio(false);
	
	char c;
	int i,j;
	cin>>n>>m>>k;
	for(i=0;i<n;++i)
		for(j=0;j<m;++j)
			cin>>a[i][j];
	
	int cnt,last,maxl,minc,ansl=0,ansc=0;
	for(j=0;j<m;++j){
		cnt=0;
		for(i=0;i<k&&i<n;++i)
			if(a[i][j])
				++cnt;
		i=0;
		maxl=cnt;
		minc=0;
		last=0;
		while(i<n){
			if(a[i][j]){
				++last;
				--cnt;
			}
			if(i+k<n&&a[i+k][j])
				++cnt;
			if(cnt>maxl){
				maxl=cnt;
				minc=last;
			}
			++i;
		}
		ansl+=maxl;
		ansc+=minc;
	}

	cout<<ansl<<' '<<ansc<<endl;
	return 0;
}
```

