22/09/2024 20:58

Tags: #Shortest_Path #graphs #01BFS #Dijkstra #priority_queue

Status: [[BFS]] [[Queue]]

## 01BFS

##### Description

Given a directed graph with N vertices and M edges.

What is the minimum number of edges needed to reverse in order to have at least one path from vertex 1 to vertex N, where the vertices are numbered from 1 to N ?

>[!Answer]
>Add a reverse edge of each original edge in the graph. Give reverse edge a weight=1 and all original edges a weight of 0. Now, the length of the shortest path will give us the answer.

```cpp
#include <bits/stdc++.h>

using namespace std;
#define int long long
#define double long double

vector<vector<pair<int,int>>> g;
vector<int> vis;
vector<int> dist;

int n,m;

void BFS01(int node){
    deque<int> dq;
    dq.push_back(node);
    dist[node] = 0;

    while(!dq.empty()){
        int cur=dq.front();
        dq.pop_front();
        vis[cur]=1;

        for(auto v:g[cur]){ //if weight is 0 added to the front
            if(v.second==0 && !vis[v.first] && dist[v.first]>dist[cur] ){
                dist[v.first]=dist[cur];
                dq.push_front(v.first);
            }
            // if weight is 1 added to the back
            else if(v.second==1 && !vis[v.first] && dist[v.first]>dist[cur]+1){
                dist[v.first]=dist[cur]+1;
                dq.push_back(v.first);
            }
        }
    }
}

void solve(){

    int t;
    cin>>t;
    while(t--){
        cin>>n>>m;
        g.clear();
        g.resize(n+1);
        vis.assign(n+1,0);
        dist.assign(n+1,1e9);

        for(int i=0;i<m;i++){
            int a,b;
            cin>>a>>b;
            g[a].push_back(make_pair(b,0));
            g[b].push_back(make_pair(a,1));
        }

         BFS01(1);
        if (dist[n] == 1e9) {
            cout << -1 << "\n";
        } else {
            cout << dist[n] << "\n";
        }
    }
}
```


# Dijkstra
##### Description

There are _n_ cities and _m_ roads. The capital is located at 1. Your task is to determine the length of the shortest route from the capital to every city.

>[!tip]
>This problem can be solved using Dijkstra's algorithm. Dijkstra's algorithm is a greedy algorithm that finds the shortest path between nodes in a weighted graph.

###### `vector<pair<int, int>>` 
in `priority_queue<pair<int, int>, vector<pair<int, int>>, comp> pq;`
This specifies the underlying container used by the priority queue to store the pairs. By default, the priority queue uses a `vector` as its internal container, which allows for efficient heap operations.

```cpp
  

#include <bits/stdc++.h>
#define int long long
using namespace std;

  
vector<vector<pair<int, int>>> g;
vector<bool> vis;
vector<int> dis;

int n, m;

class comp{  //to make it a min pq, else insert negative and extract negative
    public:
    bool operator()(pair<int, int> p1, pair<int, int> p2){
        return p1.second > p2.second;
    }
};

void dijkstra(){
    dis[1] = 0;
    priority_queue<pair<int, int>, vector<pair<int, int>>, comp> pq;
    pq.push({1, 0});

    while(!pq.empty()){
        auto val = pq.top();
        int node = val.first;
        int curWeight = val.second;
        pq.pop();
        
        if(vis[node]) continue;  //imp condition, else gives TLE
        vis[node] = true;

        for(auto neighNode : g[node]){
            int neigh = neighNode.first;
            int weight = neighNode.second;

            if(dis[neigh] > dis[node] + weight){
                dis[neigh] = dis[node] + weight;
                pq.push(make_pair(neigh, dis[neigh]));
            }
        }
    }
}

void solve(){

    cin >> n >> m;
    g.assign(n+1, vector<pair<int, int>>());
    vis.assign(n+1, false);
    dis.assign(n+1, 1e18);

    for(int i=0; i<m; i++){
        int a, b, c; 
        cin >> a >> b >> c;

        g[a].push_back({b, c});
    }

    dijkstra();

    for(int i=1; i<=n; i++) cout << dis[i] << " ";

}


signed main(){
    ios_base::sync_with_stdio(0);
    cin.tie(0); cout.tie(0);

    solve();

    return 0;

}
```
### Examples

##### Description #thread_BFS #Burn_them_All #important #unsolved

You have given an undirected graph of **N** vertices and **M** edges. Edge weight **d** on edge between nodes **u** and **v** represents that **u** and **v** are connected by a thread of length **d** units.   
You set node **A** on to the fire. It takes to fire 1 unit of time to travel 1 unit of distance via threads.  
Let **T** be the minimum time in which all the threads will be burned out. 

Your task is to find **10T**. We can prove that **10T** will always be an integer number.
##### Input Format

The first line of input contains **N -** the number of nodes in the graph.  
The second line contains **M** - the number of edges in the graph.  
Next **M** lines contain three integers **u**, **v**, **d** - there is a thread between node **u** and **v** of length **d**.  
The last line of input contains **A** - the node on which we set fire.

It’s guaranteed that graph is connected.

##### Output Format

Print the value of **10T**.

##### Constraints

1 ≤ N ≤ 2 x 105  
1 ≤ M ≤ 2 x 105  
1 ≤ u, v ≤ N  
1 ≤ d ≤ 109
```
Sample Input 1
2 
1 
1 2 2 
1

Sample Output 1
20

Sample Input 2
3 
3 
1 2 2 
2 3 2 
1 3 6 
1

Sample Output 2
50

Sample Input 3

3 
3 
1 2 2 
1 3 2 
2 3 1 
1

Sample Output 3
25
```


>[!Note]
>Priority Queue takes the bigger of the first element in a pair<int,int>
----
```cpp
#include<bits/stdc++.h>
using namespace std;
#define int long long

int n,m;
long double ans = 0;
vector<vector<pair<int,int>>> g;
vector<int> visited,dis;

void dijkstra(int st){
    priority_queue<pair<int,int>> pq;
    for(int i=1;i<=n;i++){
        dis[i] = 1e18;
        visited[i] = 0;
    }

    pq.push({-0,st});
    dis[st] = 0;
    
    while(!pq.empty()){
        auto it = pq.top();
        pq.pop();

        if(visited[it.second]) continue;
        visited[it.second] = 1;

        for(auto v:g[it.second]){
        
            if(dis[v.first]>dis[it.second]+v.second){
                dis[v.first] = dis[it.second]+v.second;
                pq.push({-dis[v.first],v.first});
            }
        }
    }

    for(int node=1;node<=n;node++){
        for(auto bt:g[node]){

            if( abs(dis[bt.first]-dis[node]) >= bt.second){
                long double temp = min(dis[bt.first],dis[node])+bt.second;
                ans = max(ans,temp*10);
            }
            else{
                long double time = bt.second+dis[node]+dis[bt.first];
                time = time/2;
                ans = max(ans,time*10);
            }
        }
    }
}
  

signed main(){

    ios_base::sync_with_stdio(false);
    cin.tie(0);cout.tie(0);



        cin>>n>>m;
        g.resize(n+1);
        visited.resize(n+1);
        dis.resize(n+1);

        for(int i=0;i<m;i++){
            int u,v,d;
            cin>>u>>v>>d;
            g[u].push_back({v,d});
            g[v].push_back({u,d});
        }

        int st;
        cin>>st;
        dijkstra(st);
        cout<<(int)ans<<"\n";

    return 0;
}
}```

