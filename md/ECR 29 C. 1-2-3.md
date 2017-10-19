# ECR 29 C. 1-2-3

######time limit per test: 1 second

Ilya is working for the company that constructs robots. Ilya writes programs for entertainment robots, and his current project is "Bob", a new-generation game robot. Ilya's boss wants to know his progress so far. Especially he is interested if Bob is better at playing different games than the previous model, "Alice".

So now Ilya wants to compare his robots' performance in a simple game called "1-2-3". This game is similar to the "Rock-Paper-Scissors" game: both robots secretly choose a number from the set {1, 2, 3} and say it at the same moment. If both robots choose the same number, then it's a draw and no one gets any points. But if chosen numbers are different, then one of the robots gets a point: 3 beats 2, 2 beats 1 and 1 beats 3.

Both robots' programs make them choose their numbers in such a way that their choice in (*i* + 1)-th game depends only on the numbers chosen by them in *i*-th game.

Ilya knows that the robots will play $k$ games, Alice will choose number $a$ in the first game, and Bob will choose $b$ in the first game. He also knows both robots' programs and can tell what each robot will choose depending on their choices in previous game. Ilya doesn't want to wait until robots play all $k$ games, so he asks you to predict the number of points they will have after the final game.

####Input

The first line contains three numbers $k$, $a$, $b$ $(1 ≤ k ≤ 10^{18}, 1 ≤ a,b ≤ 3)$.

Then 3 lines follow, $i$-th of them containing 3 numbers $A_{i,1},A_{i,2},A_{i,3}$, where $A_{i,j}$ represents Alice's choice in the game if Alice chose $i$ in previous game and Bob chose $j$ $(1 ≤ A_{i,j} ≤ 3)$.

Then 3 lines follow, $i$-th of them containing 3 numbers $B_{i,1},B_{i,2},B_{i,3}$, where $B_{i,j}$ represents Bob's choice in the game if Alice chose $i$ in previous game and Bob chose $j$ $(1 ≤ B_{i, j} ≤ 3)$.

####Output

Print two numbers. First of them has to be equal to the number of points Alice will have, and second of them must be Bob's score after $k$ games.

####Sample input

```
10 2 1
1 1 1
1 1 1
1 1 1
2 2 2
2 2 2
2 2 2
```

####Sample output

```
1 9
```

####Sample input

```
8 1 1
2 2 1
3 3 1
3 1 3
1 1 1
2 1 1
1 2 3
```

####Sample output

```
5 2
```

####Sample input

```
5 1 1
1 2 2
2 2 2
2 2 2
1 2 2
2 2 2
2 2 2
```

####Sample output

```
0 0
```

####Note

In the second example game goes like this:

![img](http://espresso.codeforces.com/1e21b6e200707470571d69c9946ace6b56f5279b.png)

The fourth and the seventh game are won by Bob, the first game is draw and the rest are won by Alice.

## Answer

可抽象成状态转换图，由于每个点都有且只有一个出度，9个点，共有9个出度，所以必然存在环，我们只需知道环的长度，即可取模求结果。

```c++
//Author:CookiC
//#include"stdafx.h"
#include<iostream>
#define LL long long
//#pragma warning(disable : 4996)
using namespace std;

LL k,sa,sb;
int a,b;
int A[4][4],B[4][4],N[4][4];

void rec(int a,int b,LL &sa,LL &sb){
	if(a==3&&b==2||a==2&&b==1||a==1&&b==3)
		++sa;
	if(a==2&&b==3||a==1&&b==2||a==3&&b==1)
		++sb;
}

int main(){
//	freopen("test.in","r",stdin);
//	freopen("test.out","w",stdout);
	ios::sync_with_stdio(false);
	
	int i,j;
	cin>>k>>a>>b;
	for(i=1;i<=3;++i)
		for(j=1;j<=3;++j)
			cin>>A[i][j];
	for(i=1;i<=3;++i)
		for(j=1;j<=3;++j)
			cin>>B[i][j];
	
	sa=0;
	sb=0;
	int at,bt;
	for(i=1;!N[a][b]&&k;++i){
		N[a][b]=i;
		rec(a,b,sa,sb);
		at=A[a][b];
		bt=B[a][b];
		a=at;
		b=bt;
		--k;
	}
	
	if(k){
		LL cir=i-N[a][b];
		LL ca=0,cb=0;
		for(i=0;i<cir;++i){
			rec(a,b,ca,cb);
			at=A[a][b];
			bt=B[a][b];
			a=at;
			b=bt;
		}
		sa+=k/cir*ca;
		sb+=k/cir*cb;
		k%=cir;
		while(k){
			rec(a,b,sa,sb);
			at=A[a][b];
			bt=B[a][b];
			a=at;
			b=bt;
			--k;
		}
	}
	cout<<sa<<' '<<sb<<endl;
	return 0;
}
```

