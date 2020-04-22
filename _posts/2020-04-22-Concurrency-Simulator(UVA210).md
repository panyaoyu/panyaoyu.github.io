---
layout: post
title: "每日算法20200422 并行程序模拟(Symmetry-UVA210)"
comments: true
description: "记一道算法题"
keywords: "UVA"
---

### UVA210

[UVA210https://vjudge.net/problem/UVA-210](https://vjudge.net/problem/UVA-210)

有的时候vector不如普通数组。

比如这道题中，笔者本来使用的是`vector<string> a[]`，而紫书给出的代码中使用的是`char**`。本来思考的是`vector<string>`相当于三维数组可以直接记忆是第几组指令中的第几条，比较方便。但是后来发现，其实还是需要一个标志来表示执行到了第几条指令。并且`vector<string> a[]`在每次开始的时候清空实在是太难受了。于是改成了与书中一致的二维数组。

另外吐槽一下这道题的Input实例，明明题中要求第一行给出总的问题个数，但是Input示例中没给，导致找了好久。

做这题的主要目的是练习`deque`。

以下是AC代码

[测试用例在https://udebug.com/UVa/210](https://udebug.com/UVa/210)

```cpp
#include<iostream>

#include<deque>

#include<queue>

#include<cctype>

#include<cstring>

using namespace std;

int c[5];
int ip[1000];
char prog[1000][10];
int var[26]; 
int n,m,Q;
deque<int> readyQ;
queue<int> blockQ;
bool locked;
void run(int pid){
	int q = Q;
	while(q>0){
		char *p = prog[ip[pid]];
		switch(p[2]){
			case '=':
				var[p[0]-'a'] = isdigit(p[5]) ? (p[4]-'0')*10+p[5]-'0':p[4]-'0';
				q -= c[0];
				break;
			case 'i':
				cout<<pid+1<<": "<<var[p[6]-'a']<<endl;
				q -= c[1];
				break;
			case 'c':
				if(locked){
					blockQ.push(pid);
					return;
				}
				locked = true;
				q -= c[2];
				break;
			case 'l':
				locked = false;
				if(!blockQ.empty()){
					int tmp = blockQ.front();
					blockQ.pop();
					readyQ.push_front(tmp);
				}
				q -= c[3];
				break;
			case 'd':
				return;
		}
		ip[pid]++;
	}
	readyQ.push_back(pid);
}
int main(){
//	freopen("data.out","w",stdout); 
	string s;
	cin>>m;
	while(m--){	
		memset(var, 0, sizeof(var));
		cin>>n;
		for(int i=0;i<5;i++)
			cin>>c[i];
		cin>>Q;
		getchar();
		int line = 0;
		for(int i=0;i<n;i++){
			fgets(prog[line++],10,stdin);
			ip[i] = line - 1;
			while(prog[line - 1][2] != 'd')		
				fgets(prog[line++], 10, stdin);
			readyQ.push_back(i);
		}
		locked = false;
		while(!readyQ.empty()){
			int pid = readyQ.front();
			readyQ.pop_front();
			run(pid);
		}
		if(m)
			cout<<endl;
	}
	return 0;
} 
```



