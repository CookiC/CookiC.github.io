# 6209 The Intersection

###### time limit per test: 3 second

A given coefficient $K$ leads an intersection of two curves $f(x)$ and $g_K(x)$. In the first quadrant, the curve $f$ is a monotone increasing function that $f(x)=\sqrt x$. The curve $g$ is decreasing and $g_K(x)=K/x$.
To calculate the $x$-coordinate of the only intersection in the first quadrant is the following question. For accuracy, we need the nearest rational number to $x$ and its denominator should not be larger than 100000.

#### Input

The first line is an integer $T (1≤T≤100000)$ which is the number of test cases.
For each test case, there is a line containing the integer $K (1≤K≤100000)$, which is the only coefficient.

#### Output

For each test case, output the nearest rational number to $x$. Express the answer in the simplest fraction.

####Sample Input

```
5
1
2
3
4
5
```

####Sample Output

```
1/1
153008/96389
50623/24337
96389/38252
226164/77347
```

## Answer

可由题目推出$x=K^{2/3}$，先计算出结果$x$，之后利用**Stern-Brocot树**，得出答案。（注意精度问题）

```c++
//Author:CookiC
//#include"stdafx.h"
#include<iostream>
#include<iomanip>
#include<cmath>
#define LL long long
#define LD long double
//#pragma warning(disable : 4996)
using namespace std;

void SternBrocot(LL K,LL &A,LL &B){
	LD x=pow(K*K,0.333333333333);
	A=x+0.5;
	B=1;
	if(A*A*A==K*K)
		return;
	LL la=x,lb=1,ra=x+1,rb=1;
	long double c,C=A*A*A,a,b;
	
	x=K*K;
	do{
		a=la+ra;
		b=lb+rb;
		c=a/b;
		c=c*c*c;
		if(abs(C-x)>abs(c-x)){
			A=a;
			B=b;
			C=c;
			if(abs(x-C)<1e-10)
			break;
		}
		if(x<c){
			ra=a;
			rb=b;
		}
		else{
			la=a;
			lb=b;
		}
	}while(lb+rb<=1e5);
}

int T;
LL K;

int main(){
//	freopen("test.in","r",stdin);
//	freopen("test.out","w",stdout);
	
	LL a,b;
	scanf("%d",&T);
	while(T--){
		scanf("%I64d",&K);
		SternBrocot(K,a,b);
		printf("%I64d/%I64d\n",a,b);
	}
	return 0;
}
```

