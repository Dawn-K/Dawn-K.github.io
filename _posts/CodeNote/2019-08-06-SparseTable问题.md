---
layout: post
title : 「CodeNote」 SparseTable问题
date: 2019-08-06
tags: [CodeNote]
categories: [CodeNote]
---
# SparseTable

## RMQ问题

> RMQ (Range Minimum/Maximum Query)问题是指：**对于长度为n的数列A，回答若干询问RMQ(A,i,j)(i,j<=n)，返回数列A中下标在i,j里的最小(大）值**，也就是说，RMQ问题是指求区间最值的问题。

## 算法

- 朴素（即搜索），```O(n)-O(qn)``` online。

- 线段树，```O(n)-O(qlogn)``` online。(**这个稍后补充**)
- ST（实质是**动态规划**），```O(nlogn)-O(q)``` online。ST算法（```Sparse Table```），以求最大值为例，设```d[i,j]```表示```[i,i+2^j-1]```这个区间内的最大值，那么在询问到[a,b]区间的最大值时答案就是```max(d[a,k], d[b-2^k+1,k])```，其中k是满足```2^k<=b-a+1```(即长度)的最大的k,即```k=[ln(b-a+1)/ln(2)]```。d的求法可以用动态规划，```d[i, j]=max(d[i, j-1],d[i+2^(j-1), j-1])```。
- **RMQ标准算法：先规约成LCA（```Lowest Common Ancestor```），再规约成约束RMQ，```O(n)-O(q)``` online。**

## ST 算法

动态规划:

我们定义状态如下:```dp[i][j]``` 表示```[i,i+2^j-1]``` 区间内的最大值

定义状态转移方程如下: ```dp[i][j] = max(dp[i][j-1],dp[i + 1<<(j-1)][j-1])``` 即将长度为```2^j```的区间拆分成两个长度为```2^(j-1)``` 的区间,长区间的最大值也正是两个短区间内的最大值中的较大者.

### 初始化

```c 
// 与上文说的一样
int dpmax[50000+100][20];
int dpmin[50000+100][20];
int a[50000+100];
void rmq_init(){
    for(int i=1;i<=n;++i){ // 针对区间长度为1情况的预处理
        dpmax[i][0]=dpmin[i][0]=a[i]; 
    }
    for(int j=1;(1<<j)<=n;++j){ // 复杂度 O(nlogn)
        for(int i=1;i+(1<<j)-1<=n;++i){
            dpmax[i][j]=max(dpmax[i][j-1],dpmax[i+(1<<(j-1))][j-1]);
            dpmin[i][j]=min(dpmin[i][j-1],dpmin[i+(1<<(j-1))][j-1]);
        }
    }
}
```

### 查询

查询的话,直接查表即可,但是由于查询的区间可能不是二的幂次,所以需要先求 ```k = ceil(log(r-l+1))``` ,即选取区间长度```myLen =  2^k``` ,显然```myLen```一定为二的幂次,并且一定不小于```(r-l+1)/2``` ,也就是说,**如果我们以左端点为起点,选取一个长为myLen的区间,再以右端点为终点,选取一个长度为myLen的区间,这两个区间一定能覆盖查询区间**.

```c
// POJ 3264
int rmq_q(int l,int r){
    int k=0;
    int len=r-l+1;
    while((1<<(k+1))<=len){
            ++k;
    }
    int ans1 = max(dpmax[l][k],dpmax[r-(1<<k)+1][k]); // 区间最大值
    int ans2 = min(dpmin[l][k],dpmin[r-(1<<k)+1][k]); // 区间最小值
    return ans1-ans2;
}
```

## LCA 问题(最近公共祖先)

### 介绍

> 对于**有根树**T的两个结点u、v，最近公共祖先LCA(T,u,v)表示一个结点x，满足x是u、v的祖先且x的深度尽可能大。
>
> 另一种理解方式是把T理解为一个**无向无环图**，而LCA(T,u,v)即u到v的最短路上深度最小的点。

LCA 问题有两种解法,一种是**在线**算法,转RMQ,然后用ST算法求解,预处理复杂度O(nlogn),单次查询O(1);另一种是tarjan**离线**算法,复杂度是O(n).在此我们先只介绍转RMQ算法的方式.

### 思路

我们发现了一个最近公共祖先的性质:如果对于有根树从根节点开始深度优先遍历,而且把经过的节点(无论曾经是否经过过),都记录下来写入vs[]数组,记u第一次在vs中出现的位置是p,v第一次在vs中出现的位置是q,那么**LCA(u,v)就一定是vs[]数组在[p,q]区间(假设p>q,否则反过来)内的深度最小的点!** 而vs[]数组的大小显然应该是O(n)级别的,因为一个节点最多经过两次(叶子节点只经过一次),所以我们用ST 以O(nlogn)的复杂度,维护一下vs[]数组,就可以在O(1)时间内应对LCA的查询了.

###  洛谷p3379

```c
// 洛谷p3379
// 目前此写法(scanf,printf,手写邻接表)能在不开O2优化下通过洛谷p3379
// 还有带权LCA,也是也可以用这种方法计算,而且(应该是)和树的根无关,参考poj 1986  和 hdu 2586
#include<bits/stdc++.h>
using namespace std;
#define MAXN 1000100
#define pb push_back
int vs[MAXN<<1]; // 存储欧拉序,大概是原序列的2倍长度
int depth[MAXN<<1]; // 和vs数组中的元素一一对应,表示他们的深度(即使一个节点多次加入vs,其depth[]对应的都是同一个值,)
int id[MAXN]; // 可以理解为反向映射,id[u]返回节点u在vs数组中第一次出现的下标
// log(1e5) < 20
int dp[MAXN<<1][20];
int head[MAXN],nxt[MAXN],to[MAXN]; // 邻接表,此题写vector会超时
vector<int> v[MAXN];
int dfsCnt=0;
int n,m,q,root;
int cnt=0;
void addEdge(int x,int y) {
    cnt++;
    nxt[cnt]=head[x];
    head[x]=cnt;
    to[cnt]=y;
}

void dfs(int u,int fa,int dep) { // 构建vs,depth,id数组,以备之后查询使用,看似复杂,实则总共时间复杂度为O(n)
    id[u]=dfsCnt; // 仅在第一次访问节点时更新
    vs[dfsCnt]=u;
    depth[dfsCnt]=dep;
    ++dfsCnt;
    for(int i=head[u]; i!=-1; i=nxt[i]) {
        int v = to[i];
        if(v==fa) // 树上无环,只要不走回头路不可能出现无限递归
            continue;
        dfs(v,u,dep+1);
        vs[dfsCnt]=u; // 回溯,将当前节点再次加入vs数组
        depth[dfsCnt]=dep;
        ++dfsCnt;
    }
}
void rmq_init(int NN) {
    for(int i=0; i<=NN; ++i) {
        dp[i][0]=i;
        // 看似违反直觉,实际上就是这样,这个i指的是vs数组的下标,同时也是dep数组的下标,则depth[] 下标在[i,i]范围内最小的深度的节点的下标自然是i(就这一个节点嘛)
        // 重点是要理解depth数组的下标的内涵是dfs序的时间戳
    }
    for(int j=1; (1<<j)<=NN; ++j) {
        for(int i=1; i+(1<<j)-1<=NN; ++i) {
            int a = dp[i][j-1];
            int b = dp[i+(1<<(j-1))][j-1];
            if(depth[a]<=depth[b]) { // dp数组存的只是下标,而判断需要借助depth数组
                dp[i][j]=a;
            } else {
                dp[i][j]=b;
            }
        }
    }
}
// 此处的dp有些魔改 dp[i][j]意义实际上是 **vs数组中**,编号在[i,i+2^j-1]范围内深度最小的元素的编号
int rmq_query(int l,int r) { // 查询的是vs数组,切记切记
    int k = (int)(log(r-l+1)/log(2)); //  比之前的模拟法求k要快一些,近乎是O(1)
    int a = dp[l][k];
    int b = dp[r-(1<<k)+1][k];
    return depth[a]<=depth[b]?a:b;
}

int query(int u,int v) {
    int x = id[u]; // u 第一次在vs中出现的下标
    int y = id[v]; // v 第一次在vs中出现的下标
    if(x<y) {
        return vs[rmq_query(x,y)];
    } else {
        return vs[rmq_query(y,x)];
    }
}
void Build_dfs() {
    dfsCnt=1;
    memset(vs,0,sizeof(vs));
    memset(id,0,sizeof(id));
    memset(depth,0,sizeof(depth));
    dfs(root,-1,0);
    rmq_init(dfsCnt-1);
}
int main() {
    ios::sync_with_stdio(0);
    scanf("%d%d%d",&n,&q,&root);
    m=n-1;
    memset(head,-1,sizeof(head));
    for(int i=0; i<m; ++i) {
        int a,b;
        scanf("%d%d",&a,&b);
        addEdge(a,b);
        addEdge(b,a);
    }
    Build_dfs();
    for(int i=0; i<q; ++i) {
        int a,b;
        scanf("%d%d",&a,&b);
        printf("%d\n",query(a,b));
    }
    return 0;
}
```

### HDU2586

```c
// HDU 2586
// 模板题
#include<bits/stdc++.h>
using namespace std;
#define MAXN 500100
#define pb push_back
#define ll long long
int vs[MAXN<<1];
int depth[MAXN<<1];
int id[MAXN];
int dp[MAXN<<1][20];
int head[MAXN],nxt[MAXN],to[MAXN],vis[MAXN],w[MAXN];
ll sum[MAXN]; // 用以保存从根节点到此节点的路径的长度之和,类似前缀和的思想
int dfsCnt=0;
int n,m,q,root;
int cnt=0;
void addEdge(int x,int y,int weight) {
    cnt++;
    nxt[cnt]=head[x];
    head[x]=cnt;
    w[cnt]=weight;
    to[cnt]=y;
}

void dfs(int u,int fa,int dep) { // 构建vs,depth,id数组,以备之后查询使用,看似复杂,实则总共时间复杂度为O(n)
    id[u]=dfsCnt; // 仅在第一次访问节点时更新
    vs[dfsCnt]=u;
    depth[dfsCnt]=dep;
    ++dfsCnt;
    for(int i=head[u]; i!=-1; i=nxt[i]) {
        int v = to[i];
        if(v==fa)
            continue;
        if(!vis[v]) {
            sum[v]=sum[u]+w[i]; // 维护前缀和
            vis[v]=1;
        }
        dfs(v,u,dep+1);
        vs[dfsCnt]=u; // 回溯,将当前节点再次加入vs数组
        depth[dfsCnt]=dep;
        ++dfsCnt;
    }
}
void rmq_init(int NN) {
    for(int i=0; i<=NN; ++i) {
        dp[i][0]=i;
        // 看似违反直觉,实际上就是这样,这个i指的是vs数组的下标,同时也是dep数组的下标,
        // 重点是要理解depth数组的下标的内涵是dfs序的时间戳
    }
    for(int j=1; (1<<j)<=NN; ++j) {
        for(int i=1; i+(1<<j)-1<=NN; ++i) {
            int a = dp[i][j-1];
            int b = dp[i+(1<<(j-1))][j-1];
            if(depth[a]<=depth[b]) {
                dp[i][j]=a;
            } else {
                dp[i][j]=b;
            }
        }
    }
}

int rmq_query(int l,int r) { 
    int k = (int)(log(r-l+1)/log(2)); 
    int a = dp[l][k];
    int b = dp[r-(1<<k)+1][k];
    return depth[a]<=depth[b]?a:b;
}

int query(int u,int v) {
    int x = id[u]; // u 第一次在vs中出现的下标
    int y = id[v]; // v 第一次在vs中出现的下标
    int lca;
    if(x<y) {
        lca =  rmq_query(x,y);
    } else {
        lca =  rmq_query(y,x);
    }
    return sum[u]+sum[v]-2*sum[vs[lca]]; // 此处这里不是返回lca了,而是返回u经lca再到v的路径长度,运用前缀和的思想, u到v的距离 == 根到u的距离 + 根到v的距离 - 2*根到lca的距离
}
void Build_dfs() {
    dfsCnt=1;
    memset(vs,0,sizeof(vs));
    memset(id,0,sizeof(id));
    memset(depth,0,sizeof(depth));
    dfs(root,-1,0);
    rmq_init(dfsCnt-1);
}
int main() {
    ios::sync_with_stdio(0);
    int T;
    scanf("%d",&T);
    while(T--) {
        scanf("%d%d",&n,&q);
        root=1;
        m=n-1;
        cnt=0;
        memset(head,-1,sizeof(head));
        memset(vis,0,sizeof(vis));
        sum[1]=0;
        vis[1]=1;
        for(int i=0; i<m; ++i) {
            int a,b,weight;
            scanf("%d%d%d",&a,&b,&weight);
            addEdge(a,b,weight);
            addEdge(b,a,weight);
        }
        Build_dfs();
        for(int i=0; i<q; ++i) {
            int a,b;
            scanf("%d%d",&a,&b);
            printf("%lld\n",query(a,b));
        }
    }
    return 0;
}
```

