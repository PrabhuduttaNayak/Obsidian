21/09/2024 17:47

Tags: #DFS

Status:

# DFS

Boiler Plate for dfs
```cpp
#include <bits/stdc++.h>

using namespace std;
#define int long long
#define double long double

vector<vector<int> > g;
vector<int> vis;

void dfs(int node, int comp){
    
    vis[node]=comp;
    for( auto v: g[node]){
        if(!vis[v]){
            dfs(v,comp);
        }
    }
}

void solve(){
    int n,m,q;
    cin>>n>>m>>q;
    g.resize(n+1);

    for(int i=0;i<m;i++){
        int a,b;
        cin>>a>>b;
        g[a].push_back(b); //for undirected graph
        g[b].push_back(a);

    }

    vis.assign(n+1,0); //it assigns size and ini val to vector

    int comp=0;
    for( int i=1; i<n+1;i++){
        if(!vis[i]){
            comp++;
            dfs(i,comp);
        }
    }
} 

signed main(){
ios_base::sync_with_stdio(false);
cin.tie(0); cout.tie(0);

solve();

    return 0;

}
```

### Key Concepts:

- **Graph Representation**: A graph can be represented using an adjacency list, where each node points to a list of its neighbors.
- **Recursion or Stack**: DFS can be implemented using recursion (implicitly using the call stack) or iteratively using an explicit stack.
- **Visited Array**: To prevent cycles and repeated exploration of nodes, we maintain a boolean array to track whether a node has been visited.

### Iterative DFS Using a Stack #stack_dfs

If we don’t want to use recursion, we can implement DFS iteratively using an explicit stack data structure.
```cpp
void dfsIterative(int start, vector<bool>& visited, vector<vector<int>>& adjList) {
    stack<int> s;
    s.push(start);
    
    while (!s.empty()) {
        int node = s.top();
        s.pop();
        
        if (!visited[node]) {
            visited[node] = true;
            cout << node << " ";  // Print the node
            
            // Push all unvisited neighbors onto the stack
            for (int neighbor : adjList[node]) {
                if (!visited[neighbor]) {
                    s.push(neighbor);
                }
            }
        }
    }
}

```

### Time and Space Complexity:
>[!info]
>- **Time Complexity**: O(V + E), where `V` is the number of vertices (nodes) and `E` is the number of edges in the graph. Each node and edge is processed once.
> 
>- **Space Complexity**: O(V), due to the space required to store the visited array and the call stack (in the recursive version) or the explicit stack (in the iterative version).



### Examples
