---
layout: post
title: "每日算法20200415 木块问题(The blocks problem-UVA101)"
comments: true
description: "记一道算法题"
keywords: "UVA"
---

### UVA101

一道模拟类型的题目，题目本身并不难，但是给了笔者一些启发。

1.使用vector的resize()方法可以用来弹出尾部的n个元素，不用一个个pop_back()弹出。

2.在开始写代码时不妨先想想如何把过程整理成简单的部分。自己写代码时候时把四种情况各写成一个函数，不仅有很多的重复代码，而且还很容易出错，而紫书上模拟部分的代码只用了两个函数而且十分简洁，很有启发。

以下是本人AC代码

[一个很长的测试用例 https://udebug.com/UVa/101](https://udebug.com/UVa/101)

```cpp
#include<iostream>

#include<vector>

using namespace std;

vector<int> pile[30];
int n;
int o1,o2;
string s1,s2;
int posa,posb,ha,hb;

void clear_above(int pos,int h){
	for(int i=h+1;i<pile[pos].size();i++){
		int tmp = pile[pos][i];
		pile[tmp].push_back(tmp);
	}
	pile[pos].resize(h+1);
} 
void pile_onto(int posa,int ha,int posb){
	for(int i=ha;i<pile[posa].size();i++){
		pile[posb].push_back(pile[posa][i]);
	}
	pile[posa].resize(ha);
}
void find_pos(int obj1,int obj2){
	for(int i=0;i<n;i++)
		for(int j=0;j<pile[i].size();j++){
			if(pile[i][j]==obj1){
				posa = i;
				ha = j;
			}
			if(pile[i][j]==obj2){
				posb = i;
				hb = j;
			}
		}
}
void print(){
	for(int i=0;i<n;i++){
		cout<<i<<":";
		for(int j=0;j<pile[i].size();j++){
			cout<<" "<<pile[i][j];
		}
		cout<<endl;
	}
}
int main(){
	cin>>n;
	for(int i=0;i<n;i++){
		pile[i].push_back(i);
	}
	while(cin>>s1&&s1!="quit"&&cin>>o1>>s2>>o2){
		find_pos(o1,o2);
		if(posa==posb)
			continue;
		if(s2=="onto")
			clear_above(posb,hb);
		if(s1=="move")
			clear_above(posa,ha);
		pile_onto(posa,ha,posb);
//		print();
	}
	print();
	return 0;
} 
```

