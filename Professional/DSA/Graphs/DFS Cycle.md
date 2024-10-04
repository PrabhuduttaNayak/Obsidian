21/09/2024 23:53

Tags: #dfs_cycle

Status:[[DFS]]

# DFS Cycle

##### Description

Zenithland has _n_ cities and _m_ roads between them. Your task is to check for the existence of a round trip that begins in a city, goes through two or more other cities, and finally returns to the starting city. Every intermediate city on the route has to be distinct.


* here we keep track of parent
* here we also keep 3 numbers ,
*    1: the vertex is completely unvisited.  
	2: the vertex whose some but not all children are visited.  
	3: the vertex whose all the children are visited

```cpp
#include<bits/stdc++.h>

using namespace std;
// int m=1e9+7;
#define ll long long
#define ld long double

int n, m;
vector<vector<int>> adj;
int iscycle = 0;
vector<int> parent;


void dfs(int par, int node, int color[]){

    color[node]=2;
    parent[node]= par;

    for(auto it : adj[node]){
        if(color[it] == 1){
            dfs(node, it, color);
        }
        else if(color[it]==2 && it != parent[node] ){
            iscycle = 1;
        }
    }
}

void solve(){
    cin >> n >> m;

    adj.resize(n+1);
    parent.resize(n+1);

    for(int i =0; i<m; i++){
        int x, y;
        cin >> x >> y;
        adj[x].push_back(y);
        adj[y].push_back(x);
    }

    int color[n+1];
    for(int i=0; i<=n; i++) color[i]=1;

    for(int i =1; i<=n; i++){
        if(color[i]==1){
            dfs(0, i, color);
        }
    }
    if(iscycle) cout << "YES" << '\n';
    else cout << "NO" << '\n';

}
signed main(){
     ios_base::sync_with_stdio(0);cin.tie(0);cout.tie(0);
      
       solve();

}
```


### Examples