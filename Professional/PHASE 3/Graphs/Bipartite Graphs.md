22/09/2024 00:07

Tags: #bipartite #graphs #dfs 

Status:[[DFS]] [[Graph Terminologies]]

# Bipartite Graphs

>[!note]
>A bipartite graph is a graph whose vertices can be divided into two disjoint sets, say A and B, such that every edge connects a vertex in set A to a vertex in set B. If the graph is bipartite, we can divide the students into two teams by assigning one team to set A and the other team to set B.

##### Description

There are _n_ students in AlgoZenith Course and _m_ friendships between them. Your task is to divide the students into two teams in such a way that no two students in a team are friends. You can freely choose the sizes of the teams. The size of each team should be positive.

**most important condition is $(3-n)$ as the choice of color**  for two colors 1 and 2

```cpp
#include<bits/stdc++.h>

using namespace std;
#define int long long

int n,m;
vector<vector<int>>adj;
vector<int>visited;
bool is_bipartite=true;

  
void dfs(int node,int color){

     visited[node]=color;

     for(auto it:adj[node])
     {
        if(visited[it]==0)
        {
            dfs(it,3-color);  //important condition
        }
        else if(visited[it]==visited[node])
        {
            is_bipartite=false;
        }
     }
     return;
}

signed main()

{
    ios_base::sync_with_stdio(0);
    cin.tie(0);cout.tie(0);

    cin>>n>>m;
    adj.resize(n+1);
    visited.assign(n+1,0);

    for(int i=0;i<m;i++)
    {
        int u,v;
        cin>>u>>v;
        adj[u].push_back(v);
        adj[v].push_back(u);
    }

    for(int i=1;i<=n;i++)
    {
        if(visited[i]==0)
        {
            dfs(i,1);
        }
    }

    if(is_bipartite)
    cout<<"YES"<<'\n';
    else
    cout<<"NO"<<'\n';

}
```


### Examples