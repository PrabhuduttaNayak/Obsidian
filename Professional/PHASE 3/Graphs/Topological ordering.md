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

---
##### Description

Given an array _A_ of _N_ positive integers, find the maximum of bitwise ANDs of all subsequences of _A_ with length equal to _X_.

##### Input Format

The first line of the input contains a single integer _T_ denoting the number of test cases, _(1<=T<=100)_.

The first line of each test case contains two space-separated integer _N_, _X_, _(2<=N<=100000), (1<=X<=N)_.

The second line contains _N_ space-separated integers A1,A2,…,AN, _(1<=Ai<=10^9)_

```cpp
#include <bits/stdc++.h>
using namespace std;

signed main()
{
   ios_base::sync_with_stdio(false);cin.tie(0);cout.tie(0);

   int testCase;
   cin>>testCase;
   while(testCase--){
       int n,x;
       cin>>n>>x;
       vector < int > arr(n);
       for(int i=0;i<n;i++)
           cin>>arr[i];
       int ans = 0;
       for(int i=29;i>=0;i--){
           vector < int > elementBitSet;
           for(auto v:arr){
               if(v&(1LL<<i))
                   elementBitSet.push_back(v);
           }
           if(elementBitSet.size()>=x){
               ans+=(1LL<<i);      //if all elements have that bit as 1 then direclty add 1 to it
               arr = elementBitSet; //check that same array again
           }
       }
       cout<<ans<<"\n";
   }

}```
#### Hint 2

Take all the numbers with highestBit set if the count is >=X. Add 1<<(highestBit) in your answer. Now your problem reduces to the array of elements with highestBit set and highestBit-1 be the next bit to be considered. If the subsequence of length X is not present then calculate the answer for the same array with highestBit-1 as the new highestBit possible.



##### Example #unsolved 
##### Description

A game has _n_ levels, connected by _m_ teleporters, and your task is to get from level 1 to level _n_. The game has been designed so that there are no directed cycles in the underlying graph. In how many ways can you complete the game?

##### Input Format

The first input line has two integers _n_ and _m_: the number of levels and teleporters. The levels are numbered 1, 2, …, _n_.  
After this, there are _m_ lines describing the teleporters. Each line has two integers _a_ and _b_: there is a teleporter from level _a_ to level _b_.

##### Output Format

Print one integer: the number of ways you can complete the game. Since the result may be large, print it modulo 109+7.

##### Constraints

1 ≤ _n_ ≤ 105  
1 ≤ _m_ ≤ 2 x 105  
1 ≤ _a_, _b_ ≤ _n_

##### Sample Input 1

4 5 
1 2
2 4 
1 3 
3 4 
1 4

##### Sample Output 1

3


# Solution Approach

[[DP]] #DP
One useful property of directed acyclic graphs is, as the name suggests, that no cycles exist. If we consider each node in the graph as a state, we can perform dynamic programming on the graph if we process the states in an order that guarantees for every edge u → v that u is processed before v. Fortunately, this is the exact definition of a topological sort!

Let dp[v] denote the number of paths reaching v. We can see, dp[v]= sum of all dp[u], s.t. u → v edge exists.  
With an exception of dp[1], or the starting node, having a value of 1. We process the nodes topologically so dp[u] will already have been computed before dp[v]
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