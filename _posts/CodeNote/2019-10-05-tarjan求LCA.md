---
layout: post
title : 「CodeNote」 tarjan求LCA
date: 2019-10-05
tags: [CodeNote]
categories: [CodeNote]
---

# tarjan求LCA

## 介绍

第三种求LCA的方法,离线的.

但是实现时如果用STL,就还不如在线的算法快(已在洛谷实验过).

## 算法

算法是这样的,首先建图,然后将查询用邻接表的方式存下.

然后从根节点进行dfs,然后每访问到一个节点,就标记.在访问了一个节点`u`的过程中,将它的子节点都dfs一下.然后用并查集将u,v合并(u作为父亲)

然后处理完节点u的所有子节点之后,开始进行处理u的查询,如果对于询问`(u,v)`(在此我们不考虑顺序),如果v已经被标记过,则LCA(u,v)就是Find(v).

在此不再证明.

最后按照询问依次输出答案即可.此算法较为繁琐,细节多,所以要仔细些.

## 模板

```c
#include<bits/stdc++.h>
#define ll long long
#define pii pair<int,int>
#define eps 1e-6
#define inf 0x3f3f3f3f
#define infll (ll)(1e18)
#define maxn 500000+100
using namespace std;
int n,q,rt;
struct Edge {
    int u,v,id,next;
    Edge(int _u=0,int _v=0,int _id=-1) {
        u=_u,v=_v,id=_id;
    }
} edge[2*maxn],qusetion[2*maxn];
int head[maxn],que_head[maxn],ans[maxn];
int cnt,que_cnt;
int f[maxn];
bool vis[maxn];
bool que_vis[maxn];
int Find(int x) {
    if(x!=f[x]) {
        f[x]=Find(f[x]);
    }
    return f[x];
}
void Mix(int x,int y) {
    int fx=f[x];
    int fy=f[y];
    if(fx!=fy) {
        f[fx]=fy;
    }
}
void init() {
    cnt=0;
    que_cnt=0;
    for(int i=0; i<=n; ++i) {
        head[i]=-1;
        que_head[i]=-1;
        f[i]=i;
        vis[i]=0;
    }
    for(int i=0; i<=q; ++i) {
        que_vis[i]=0;
        ans[i]=-1;
    }
}
void add(int u,int v) {
    edge[cnt].u=u;
    edge[cnt].v=v;
    edge[cnt].next=head[u];
    head[u]=cnt;
    ++cnt;
}
void que_add(int u,int v,int id) {
    qusetion[que_cnt].u=u;
    qusetion[que_cnt].v=v;
    qusetion[que_cnt].id=id;
    qusetion[que_cnt].next=que_head[u];
    que_head[u]=que_cnt; // 之前这里写错了
    ++que_cnt;
}
void tarjan_lca(int u) {
    vis[u]=1;
    for(int i=head[u]; i+1; i=edge[i].next) {
        int v=edge[i].v;
        if(vis[v])
            continue;
        tarjan_lca(v);
        Mix(v,u);
    }
    for(int i=que_head[u]; i+1; i=qusetion[i].next) {
        int id=qusetion[i].id;
        if(!que_vis[id]) {
            int v=qusetion[i].v;
            if(vis[v]) {
                ans[id]=Find(v);
                que_vis[id]=1;
            }
        }
    }
}

int main() {
    while(scanf("%d%d%d",&n,&q,&rt)!=EOF) {
        init();
        for(int i=1; i<n; ++i) {
            int u,v;
            scanf("%d%d",&u,&v);
            add(u,v);
            add(v,u);
        }
        for(int i=0; i<q; ++i) {
            int u,v;
            scanf("%d%d",&u,&v);
            que_add(u,v,i);
            que_add(v,u,i);
        }
        tarjan_lca(rt);
        for(int i=0; i<q; ++i) {
            printf("%d\n",ans[i]);
        }
    }
    return 0;
}

```

