22/09/2024 10:25

Tags: #topological #topo_dfs #topo_bfs #kahn_algorithm

Status:[[DFS]]    [[BFS]] 

# Topological ordering


#### Using DFS
* we add elements to the back of a new topo vector at the end of each dfs function
* and finally reverse is
```cpp
#include <bits/stdc++.h>
using namespace std;

vector<vector<int>> g; // Adjacency list for the graph
vector<int> vis;       // Visited array to keep track of visited nodes
vector<int> topo;      // Vector to store the topological ordering

// Depth First Search (DFS) for topological sorting
void dfs(int node) {
    vis[node] = 1;  // Mark the node as visited
    for (auto v : g[node]) {
        if (!vis[v]) {  // If the adjacent node is not visited
            dfs(v);     // Recur for that node
        }
    }
    topo.push_back(node); // Once done with all adjacent nodes, add this node to topo
}

signed main() {
    int n, m;       // n is the number of nodes, m is the number of edges
    cin >> n >> m;
    g.resize(n + 1);   // Resize the graph to fit n nodes (1-based index)
    vis.assign(n + 1, 0); // Initialize the visited array to 0

    // Read the graph edges
    for (int i = 0; i < m; i++) {
        int a, b;
        cin >> a >> b;
        g[a].push_back(b); // Directed edge from a to b
    }

    // Perform DFS for all unvisited nodes
    for (int i = 1; i <= n; i++) {
        if (!vis[i]) {
            dfs(i);
        }
    }

    // Reverse the topological order because we are using DFS
    reverse(topo.begin(), topo.end());

    // Print the topological ordering
    for (auto v : topo) {
        cout << v << " ";
    }
    cout << endl;
    return 0;
}

```


#### Using BFS (Kahn's Algorithm)

* Here we use the concept of indegree
 >[!Process the Queue]
 >
- Remove a vertex `u` from the queue.
- Add `u` to the topological order.
- For each adjacent vertex `v` (i.e., all vertices that `u` points to), reduce the in-degree of `v` by 1 because we just removed one of its incoming edges.
- If the in-degree of `v` becomes 0, add `v` to the queue.

>[!Tip]
>We use *priority queue* instead of queue if we want lexiographically smallest order
>

##### Description

Among the sequences P that are permutations of (1,2,…,N) and satisfy the condition below, find the lexicographically smallest sequence.

- For each i=1,…,M. Ai​ appears earlier than Bi​​ in P.

If there is no such P, print -1.
##### Input Format

Input is given from Standard Input in the following format: 
N M 
A1​ B1​ 
: 
:
AM BM


```cpp
#include<bits/stdc++.h>
using namespace std;
#define int long long


int n,m;
vector<vector<int>>grid;
vector<int>indegree;
vector<int>toposort;


void kahn(){
    
    priority_queue<int>pq;
    
    for(int i=1;i<=n;i++){
        if(indegree[i]==0){
            pq.push(-i);    //negative because we want to sort in increasing order
        }
    }
    
    while(!pq.empty()){
        
        int curr=-pq.top();
        pq.pop();
        toposort.push_back(curr);
        
        for(auto it:grid[curr]){            
            indegree[it]--;
            if(indegree[it]==0){
               pq.push(-it); 
            }
        }
    }
}
signed main()
{
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    cout.tie(NULL);
   
    cin>>n>>m;
    grid.resize(n+1);
    indegree.assign(n+1,0);  //initialized indegrees
    
    while(m--){        
        int a,b;
        cin>>a>>b;        
        grid[a].push_back(b);
        indegree[b]++;       //every indegree in increased from input
    }
    
    kahn();
       
    if(toposort.size()<n){
        cout<<-1<<"\n";
    }
    else{
        
        for(auto v:toposort){
        cout<<v<<" ";
        }
        cout<<"\n";
    }    
}
```
### Number of Possible Topological Sort 
##### Description

A game has _n_ levels, connected by _m_ teleporters, and your task is to get from level 1 to level _n_. The game has been designed so that there are no directed cycles in the underlying graph. In how many ways can you complete the game?

```cpp
#include <iostream>
#include <vector>
#include <queue>
using namespace std;
int n;
vector<int> edge[100001];
vector<int> backedge[100001];

int main(){
    ios_base::sync_with_stdio(0); cin.tie(0);
    int m; cin >> n >> m;
    int in_degree[n+1], dp[n+1];
    for(int i = 0; i <= n; i++){
        in_degree[i] = 0;
        dp[i] = 0;
    }
    dp[1] = 1;
    for(int i = 0; i < m; i++){
        int a,b; cin >> a >> b;
        edge[a].push_back(b);
        backedge[b].push_back(a);
        in_degree[b]++;
    }
    //uses Kahn's algorithm
    queue<int> q;
    for(int i = 0; i < n; i++) {
        if(in_degree[i] == 0) {
            q.push(i);
        }
    }

    while(!q.empty()) {
        int node = q.front();
        q.pop();
        for(int next : edge[node]) {
            in_degree[next]--;
            if(in_degree[next] == 0) q.push(next);
        }

        for(int prev : backedge[node]) {
            dp[node] = (dp[node] + dp[prev]) % 1000000007;
        }
    }
    cout << dp[n] << endl;
    return 0;
}```

```