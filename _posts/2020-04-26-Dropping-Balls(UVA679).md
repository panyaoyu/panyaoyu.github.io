---
layout: post
title: "每日算法20200426 小球下落(Dropping Balls-UVA679)"
comments: true
description: "记一道算法题"
keywords: "UVA"
---

[UVA679](https://vjudge.net/problem/UVA-679)

二叉树的一条初级题目。很有意思的题。

得到正确答案很简单，用数组模拟二叉树即可，问题是如何不超时。

如何给多少个小球就模拟多少次一定会超时。甚至使用一个状态数组来判断每个节点的开关状态都会超时，因此这题看上去简单，但是想要AC还是要有一定的技巧的。

紫书给出的方法是用小球的id直接判断小球会下落到左子树还是右子树，这样无论有多少数目的小球都只需要模拟一次。

以下是AC代码

[测试用例https://udebug.com/UVa/12657](https://udebug.com/UVa/679)

```cpp
#include<iostream>

using namespace std;

int n,m,d;
int main(){
//	freopen("data.in","r",stdin);
//	freopen("data.out","w",stdout);
	scanf("%d",&n);
	while(n--){
		scanf("%d%d",&d,&m);
		int pos = 1;
		for(int i=0;i<d-1;i++){
			if(m&1){
				m = (m+1)/2;
				pos = 2*pos;
			}
			else{
				m = m/2;
				pos = pos*2+1;
			}
		}
		cout<<pos<<endl;		
	}
	return 0;
} 
```

