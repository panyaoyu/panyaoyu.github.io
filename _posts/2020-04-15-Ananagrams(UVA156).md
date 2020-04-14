---
layout: post
title: "每日算法20200415 反片语(Ananagrams-UVA156)"
comments: true
description: "记一道算法题"
keywords: "UVA"
---

### UVA156

也是一道很简单的题目，同样是因为有些启发才记下这道题目。

1.以前很少用到map，但是从这道题看map在处理某些字符串类型的题目时还是非常有用的。

2.紫书的代码中repr函数定义如下`string repr(const string& s)`，一开始没看懂，const可以理解，为了防止传进去的字符串被修改，但是为什么函数中没有对s进行修改但是传参时加了引用符号`&`呢？

在网上查了一下，如果不加入引用符号，传参方式是按值传入，相当于`new`了一个新的和`s`内容一样`string`。string本身是一个类，创建对象时需要调用构造函数和析构函数，这就增加了额外的时间和空间开销，如果直接把s的引用传进去就可以省下这个开销，虽然实际中这个节省的开销可以忽略不计(起码对于笔者目前的水平来说)，但是这是一种极客精神的体现，笔者十分有感触，因此在这里记下这个看上去难度不大的题目。

以下是笔者AC代码

```cpp
#include<iostream>

#include<map>

#include<cctype>

#include<algorithm>

#include<vector> 

using namespace std;

string s;
map<string,int> dict;
vector<string> tmp;
vector<string> ans;
string repr(const string& s){
	string res = s;
	int len = res.length();
	for(int i=0;i<len;i++){
		res[i] = tolower(res[i]);
	}
	sort(res.begin(),res.end());
	return res;
}
int main(){
	while(cin>>s){
		if(s=="#")
			break;
		tmp.push_back(s);
		if(!dict[repr(s)])
			dict[repr(s)] = 0;
		dict[repr(s)]++;
	}
	for(int i=0;i<tmp.size();i++){
		if(dict[repr(tmp[i])]==1)
			ans.push_back(tmp[i]);
	}
	sort(ans.begin(),ans.end());
	for(vector<string>::iterator it=ans.begin();it<ans.end();it++)
		cout<<*it<<endl;
	return 0; 
} 
```

