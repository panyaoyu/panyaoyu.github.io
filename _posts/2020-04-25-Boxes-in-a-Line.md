---
layout: post
title: "每日算法20200425 移动盒子(Boxes in a line-UVA12657)"
comments: true
description: "记一道算法题"
keywords: "UVA"
---

[UVA12657](https://vjudge.net/problem/UVA-12657)

这题要是自己写的话非常难调。

和单链表一样，本体用两个数组表示双向链表的连接顺序。值得学习。

此外，本题适用了内联函数，

```cpp
inline void link(int L, int R) {
  	right_[L] = R; 
  	left_[R] = L;
}
```

内联函数可以减少调用函数产生的开销，因此以后遇到简单的函数可以写成内联的形式。遗憾的是内联函数中不能含有复杂的控制语句(while，switch等)，也不能有递归调用。

我比较了一下使用内联函数70msAC，不使用180msAC，没有想到效率差了这么多。

以下是AC代码

[测试用例https://udebug.com/UVa/12657](https://udebug.com/UVa/12657)

```cpp
#include<algorithm>

#include<iostream>

using namespace std;

int n, left_[100010], right_[100010];

inline void link(int L, int R) {
  	right_[L] = R; 
    left_[R] = L;
}

int main() {
//	freopen("data.out","w",stdout);
  	int m, kase = 0;
  	while(cin>>n>>m) {
    for(int i = 1; i <= n; i++) {
      	left_[i] = i-1;
      	right_[i] = (i+1) % (n+1);
    }
    right_[0] = 1; left_[0] = n;
    int op, X, Y, inv = 0;
    while(m--) {
      	cin>>op;
      	if(op == 4) inv = !inv;
      	else {
	        cin>>X>>Y;
	        if(op == 3 && right_[Y] == X) 
				swap(X, Y);
	        if(op != 3 && inv) 
				op = 3 - op;
	        int LX = left_[X], RX = right_[X], LY = left_[Y], RY = right_[Y];
	        if(op == 1) {
				if(X == left_[Y])
					continue;	
	          	link(LX, RX); 
				link(LY, X); 
				link(X, Y);
	        }
	        else if(op == 2) {
	        	if(X == right_[Y])
					continue;
	          	link(LX, RX);
				link(Y, X);
				link(X, RY);
	        }
	        else if(op == 3){
			        if(right_[X] == Y) { 
					  	link(LX, Y); 
						link(Y, X); 
						link(X, RY); 
					}
		          	else { 
					  	link(LX, Y); 
						link(Y, RX); 
						link(LY, X); 
						link(X, RY); 
					}
	        }
      	}
    }
	    int k = 0;
	    long long ans = 0;
	    for(int i = 1; i <= n; i++){
	      	k = right_[k];
	      	if(i&1) ans += k;
	    }
	    if(inv && !(n&1)) 
			ans = (long long)n*(n+1)/2 - ans;
		cout<<"Case "<<++kase<<": "<<ans<<endl;
  	}
  return 0;
}
```

