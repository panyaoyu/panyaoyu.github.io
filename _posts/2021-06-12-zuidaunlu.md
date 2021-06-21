---
layout: post
title: "算法笔记 最短路(Acwing849)"
comments: true
description: "最短路博客"
keywords: "最短路"
---

### [849. Dijkstra求最短路 I - AcWing题库](https://www.acwing.com/problem/content/description/851/)

```cpp
// 朴素dijkstra vector模拟邻接表
#include<bits/stdc++.h>
using namespace std;
typedef pair<int, int> PII;
const int maxn = 1e5 + 5;
const int INF = 0x3f3f3f3f;
int v[500 + 5], d[500 + 5];
vector<PII> g[maxn];
int main(){
    int n, m;
    cin >> n >> m;
    int a, b, c;
    for(int i = 0; i < m;i++){
        scanf("%d%d%d",&a,&b,&c);
        g[a].push_back({b,c});
    }
    memset(d,0x3f,sizeof(d));
    d[1] = 0;
    for(int i =0;i<n;i++){
        int x = 0,m = INF;
        for(int j = 1; j <= n; j++){
            if (!v[j] && d[j] < m){
                x = j;
                m = d[j];
            }
        }
        v[x] = 1;
        for(int i = 0; i < g[x].size(); i++){
            int to = g[x][i].first, edge = g[x][i].second;
            if (d[to] > d[x] + edge)
                d[to] = d[x] + edge;            
        }
    }
    if (d[n]==INF)
        cout << -1 << endl;
    else
        cout << d[n] << endl;
}
```

```cpp
// 朴素dijkstra 数组模拟邻接表
#include<bits/stdc++.h>
using namespace std;
const int maxn = 1e5 + 5;
const int INF = 0x3f3f3f3f;
int v[500 + 5], d[500 + 5];
int e[maxn],w[maxn],h[maxn],ne[maxn], idx;
void add(int a, int b, int c){
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx++;
}
int main(){
    int n, m;
    cin >> n >> m;
    int a, b, c;
    memset(h,-1,sizeof(h));
    for(int i = 0; i < m;i++){
        scanf("%d%d%d",&a,&b,&c);
        add(a,b,c);
    }
    memset(d,0x3f,sizeof(d));
    d[1] = 0;
    for(int i =0;i<n;i++){
        int x = 0,m = INF;
        for(int j = 1; j <= n; j++){
            if (!v[j] && d[j] < m){
                x = j;
                m = d[j];
            }
        }
        v[x] = 1;
        for(int j = h[x]; j!=-1;j=ne[j]){
            int to = e[j],edge = w[j];
            if (d[to] > d[x] + edge)
                d[to] = d[x] + edge;
        }
    }
    if (d[n]==INF)
        cout << -1 << endl;
    else
        cout << d[n] << endl;
}
```

