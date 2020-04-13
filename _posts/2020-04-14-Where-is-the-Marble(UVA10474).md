---
layout: post
title: "每日算法20200414 大理石在哪儿(Where is the Marble?-UVA10474)"
comments: true
description: "记一道算法题"
keywords: "UVA"
---

### UVA10474

一道简单的排序题目。是紫书上的一道例题，但是紫书上给出的答案有一点小问题。

下面是紫书的代码，直接提交会有error。原因是`if(a[p] == x) printf("%d found at %d\n", x, p+1);`此处数组中所有元素都小于要找的值时，会返回元素的个数n，会发生越界。

```cpp
#include<cstdio>
#include<algorithm>
using namespace std;

const int maxn = 10000;

int main() {
  int n, q, x, a[maxn], kase = 0;
  while(scanf("%d%d", &n, &q) == 2 && n) {
    printf("CASE# %d:\n", ++kase);
    for(int i = 0; i < n; i++) scanf("%d", &a[i]);
    sort(a, a+n);
    while(q--) {
      scanf("%d", &x);
      int p = lower_bound(a, a+n, x) - a; 
      if(a[p] == x) printf("%d found at %d\n", x, p+1);
      else printf("%d not found\n", x);
    }
  }
  return 0;
}
```



以下是本人AC代码

```cpp
#include<iostream>
#include<algorithm>
using namespace std;
int m,n;
int a[10000];
int main(){
//	freopen("data.out","w",stdout);
	int tmp;
	int tt = 1;
	while(cin>>m>>n){
		if(m==0)
			break;
		printf("CASE# %d:\n",tt++);
		for(int i=0;i<m;i++)
			cin>>a[i];
		sort(a,a+m);
		for(int i=0;i<n;i++){	
			cin>>tmp;
			int temp = lower_bound(a,a+m,tmp)-a;
			if(temp<m&&a[temp]==tmp)
				printf("%d found at %d\n",tmp,temp+1);
			else
				printf("%d not found\n",tmp);
		}
	}
	return 0;
} 
```

