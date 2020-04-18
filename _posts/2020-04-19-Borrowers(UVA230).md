---
layout: post
title: "每日算法20200419 图书管理系统(Borrowers-UVA230)"
comments: true
description: "记一道算法题"
keywords: "UVA"
---

### UVA230

[题目链接https://vjudge.net/problem/UVA-230](https://vjudge.net/problem/UVA-230)

这道题花了非常多的工夫。。。一开始看上去挺简单的，但是越做越发现不对劲。。。

最后花了很大劲才AC。

主要思路是这样的。

1.创建一个书的结构体(变量包括标题和作者，其中重载<号，否则无法使用`sort`和`set`)，把所有的书籍先读进去一个`vector`然后排好序，用下标作为其ID,建立一个从title到ID的map映射，否则无法找到对应的书籍。

2.用创建两个`set`，第一个`set`(ret)保存已经还回来书的信息，另一个`set`(books)保存图书馆中有的书的信息。通过遍历ret一个个输出，通过在books中查找可以找到书籍的上一本。

笔者水平有限，遇到一个问题，在下面的代码中这一小段。笔者一开始`library[it1]`处写的是`library[*it2]`笔者水平有限，觉得两者是一样的，但是事实上后者会WA。笔者百思不得其解，希望有高手能告诉我原因。

```cpp
set<int>::iterator it2 = books.find(it1);
				if(it2 == books.begin()){
					printf("Put \"%s\" first\n",library[it1].title.c_str());
				}
				else{
					printf("Put \"%s\" after \"%s\"\n",library[it1].title.c_str(),library[*--it2].title.c_str());
```

以下是AC代码

[这里有很多测试样例](https://udebug.com/UVa/230)

```cpp
#include<iostream>

#include<set>

#include<sstream>

#include<map>

#include<vector>

#include<algorithm>

using namespace std;

struct book{
	string title;
	string author;
	book(string s1,string s2):title(s1),author(s2){}
	bool operator < (const book& b){
		return author<b.author||(author==b.author&&title<b.title);
	}
};
map<string,int> Map;
set<int> books,ret;
vector<book> library; 
int main(){
//	freopen("data.out","w",stdout);
	string s;
	while(getline(cin,s)&&s[0]!='E'){
		int p = s.find("by");
		string title = s.substr(1,p-3);
		string author = s.substr(p+3);
		library.push_back(book(title,author));
	}
	sort(library.begin(),library.end());
	int len = library.size();
	for(int i=0;i<len;i++){
		Map[library[i].title] = i;
		books.insert(i);
	}
	while(getline(cin,s)&&s[0]!='E'){
		if(s[0]=='S'){		
			for(int it1 : ret){
				set<int>::iterator it2 = books.find(it1);
				if(it2 == books.begin()){
					printf("Put \"%s\" first\n",library[it1].title.c_str());
				}
				else{
					printf("Put \"%s\" after \"%s\"\n",library[it1].title.c_str(),library[*--it2].title.c_str());
				}
			}
			cout<<"END"<<endl;
			ret.clear();
		}
		else{
			string title = s.substr(8, s.size()-8-1);
			if(s[0]=='B')
				books.erase(Map[title]);
			else{
				books.insert(Map[title]);
				ret.insert(Map[title]);
			}
		}
	}
	return 0;
}
```

