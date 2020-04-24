---
layout: post
title: "每日算法20200423 矩阵链乘(Matrix Chain Multiplication-UVA422)"
comments: true
description: "记一道算法题"
keywords: "UVA"
---

[UVA442](https://vjudge.net/problem/UVA-442)

这道题难度不大，但是有一个点以前没注意过，因此记录下来。

```cpp
struct Matrix{
	int row,col;
	Matrix(int row=0,int col=0):row(row),col(col){}
} ;
Matrix m[26];
```

在编写这一段代码时，一开始笔者在构造函数时没有给默认值，因此编译一直不过，百思不得其解。后来看到紫书上的解答，构造函数中给出了默认值，恍然大悟。因为在声明数组的时候会自动进行初始化，如果没有默认初始值的话，无法初始化数组肯定会报错。

以下是AC代码

[测试用例https://udebug.com/UVa/442](https://udebug.com/UVa/442)

```cpp
#include<iostream>

#include<stack>

#include<cctype>

using namespace std;

int N;
struct Matrix{
	int row,col;
	Matrix(int row=0,int col=0):row(row),col(col){}
} ;
Matrix m[26];
int main(){
	cin>>N;
	char c;
	int row,col;
	for(int i=0;i<N;i++){
		cin>>c>>row>>col;
		m[c-'A'].row = row;
		m[c-'A'].col = col;
	}
	string s;
	while(cin>>s){
		stack<Matrix> st;
		int sum = 0;
		int len = s.length();
		int flag = 0;
		for(int i=0;i<len;i++){
			if(isalpha(s[i]))
				st.push(m[s[i]-'A']);
			if(s[i]==')'){
				Matrix a = st.top();
				st.pop();
				Matrix b = st.top();
				st.pop();				
				if(a.row!=b.col){
					flag = 1;
					break;
				}
				st.push(Matrix(b.row,a.col));
				sum += b.row*b.col*a.col;
			}
		} 
		if(flag)
			cout<<"error"<<endl;
		else
			cout<<sum<<endl;
	}  
	return 0;
}
```

