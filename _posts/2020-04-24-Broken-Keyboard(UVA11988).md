---
layout: post
title: "每日算法20200423 破损的键盘(Broken Keyboard-UVA11988)"
comments: true
description: "记一道算法题"
keywords: "UVA"
---

[UVA11988](https://vjudge.net/problem/UVA-11988)

以前写过链表，大概是以下这样

```cpp
struct Node{
  int value,
  Node * next;
};
```

这题中的链表写法比较巧妙，很有意思。他并没有新建任何结构体，只是用一个数组来保存某位置上下一个元素的位置。

这种写法插入的写法很简洁。这种模拟链表的方式值得记住。

```cpp
next[i] = next[cur];
next[cur] = i;
cur = i;
```

以下是AC代码

使用next作为数组名不知为何会报错，因此改成了next_，不影响结果。

[测试用例https://udebug.com/UVa/11988](https://udebug.com/UVa/11988)

```cpp
#include<iostream>

#include<cstring>

using namespace std;

int next_[100000+5];
int main(){
//	freopen("data.out","w",stdout);
	string s ;
	while(cin>>s){
		memset(next_,0,sizeof(next_));
		int len = s.length();
		int cur = 0;
		int end = 0;
		next_[0] = 0; 
		s = " "+s;
		for(int i=1;i<=len;i++){
			if (s[i]=='[')
				cur = 0;
			else if (s[i]==']')
				cur = end;
			else{
				next_[i] = next_[cur];
				next_[cur] = i;
				if(cur==end)
					end = i;
				cur = i;
			}
		}
		for(int i=next_[0];i;i=next_[i]){
			cout<<s[i];
		}
		cout<<endl;
	}
	return 0;
}
```

