---
layout: post
title: "每日算法20200416 集合栈计算机(The Setstack Computer-UVA12096)"
comments: true
description: "记一道算法题"
keywords: "UVA"
---

### UVA12096

[题目链接https://vjudge.net/problem/UVA-12096](https://vjudge.net/problem/UVA-12096)

或许题目也可以叫套娃(->_->)

一道把STL运用到机制的题目，来自于紫书第五章。用到了`<map>``<set>``<vector>``<algorithm>`

题目本身不难，用比较笨的方法也能做，但是用了STL后整体代码非常简洁好看。

`set_union`和`set_intersection`这两个方法以及`inserter`这个对象以前都没有接触过，说明自己见识过的东西还是太少了，还是要加强学习啊。

以下是AC代码

```cpp
#include<iostream>

#include<stack>

#include<set>

#include<map>

#include<vector>

#include<algorithm>

#define ALL(x) x.begin(),x.end()

#define INS(x) inserter(x,x.begin())

using namespace std;
typedef set<int> Set;
int n,m;
stack<int> s;
map<Set,int> IDcache;
vector<Set> Setcache;

int ID(Set x){
	if(IDcache.count(x))
		return IDcache[x];
	Setcache.push_back(x);
	return IDcache[x] = IDcache[x] = Setcache.size()-1;
}

int main(){
	cin>>n;
	string op;
	for(int i=0;i<n;i++){
		cin>>m;
		for(int j=0;j<m;j++){
			cin>>op;
			if(op[0]=='P')
				s.push(ID(Set()));
			else if(op[0]=='D')
				s.push(s.top());
			else{
				Set x1 = Setcache[s.top()];s.pop();
				Set x2 = Setcache[s.top()];s.pop();
				Set x;
				if(op[0]=='U') set_union(ALL(x1),ALL(x2),INS(x));
				if(op[0]=='I') set_intersection(ALL(x1),ALL(x2),INS(x));
				if(op[0]=='A'){
					x = x2;
					x.insert(ID(x1));
				}
				s.push(ID(x));
			} 
			cout<<Setcache[s.top()].size()<<endl;
		}
		cout<<"***"<<endl; 
	}
	return 0;
} 
```

