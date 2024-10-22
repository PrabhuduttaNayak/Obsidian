{20/10/2024 14:14

Tags: [[Union Find DSU]] [[00_DSA]] [[Trees]] [[Recursion]]

Status:

# MST


### Solved Using #Kruskal's Algorithm or #Prim's Algorithm


Here’s how you can use the **Disjoint Set Union (DSU)** (or **Union-Find**) data structure to solve **Kruskal’s Minimum Spanning Tree (MST)** problem. This approach uses the DSU to detect cycles while adding edges in increasing order of their weights.

### Steps of Kruskal's Algorithm:

1. Sort all edges by their weights.
2. Pick the smallest edge and check if adding this edge forms a cycle using DSU.
    - If no cycle is formed, add this edge to the MST.
    - If a cycle is formed, discard the edge.
3. Repeat until we have added n−1n-1n−1 edges (where nnn is the number of vertices in the graph).

### Code
```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

struct UnionFind {
    int n, set_size, *parent, *rank;

    UnionFind(int a) {
        n = set_size = a;
        parent = new int[n+1];
        rank = new int[n+1];
        for(int i = 1; i <= n; ++i) {
            parent[i] = i;
            rank[i] = 1;
        }
    }

    int find(int x) {
        if (x != parent[x])
            parent[x] = find(parent[x]); // Path compression
        return parent[x];
    }

    void merge(int x, int y) {
        int xroot = find(x), yroot = find(y);
        if (xroot != yroot) {
            if (rank[xroot] >= rank[yroot]) {
                parent[yroot] = xroot;
                rank[xroot] += rank[yroot];
            } else {
                parent[xroot] = yroot;
                rank[yroot] += rank[xroot];
            }
            set_size--;
        }
    }
};

// Edge structure
struct Edge {
    int u, v, weight;
    Edge(int u, int v, int w) : u(u), v(v), weight(w) {}
};

// Comparator function to sort edges by weight
bool compare(Edge a, Edge b) {
    return a.weight < b.weight;
}

// Kruskal's algorithm function
int kruskal(int n, vector<Edge> &edges) {
    UnionFind dsu(n);  // Create a DSU instance for 'n' nodes
    sort(edges.begin(), edges.end(), compare);  // Sort edges by weight

    int mst_weight = 0;  // To store total weight of the MST
    vector<Edge> mst_edges;  // To store the edges in the MST

    for (Edge &edge : edges) {
        // Check if adding the edge forms a cycle
        if (dsu.find(edge.u) != dsu.find(edge.v)) {
            dsu.merge(edge.u, edge.v);
            mst_edges.push_back(edge);
            mst_weight += edge.weight;

            // If we've added n-1 edges, the MST is complete
            if (mst_edges.size() == n - 1) {
                break;
            }
        }
    }

    cout << "Edges in the MST:\n";
    for (Edge &e : mst_edges) {
        cout << e.u << " -- " << e.v << " == " << e.weight << "\n";
    }

    return mst_weight;
}

int main() {
    int n = 5;  // Number of vertices
    vector<Edge> edges;

    // Add edges (u, v, weight)
    edges.push_back(Edge(1, 2, 10));
    edges.push_back(Edge(1, 3, 5));
    edges.push_back(Edge(2, 3, 8));
    edges.push_back(Edge(2, 4, 7));
    edges.push_back(Edge(3, 4, 6));
    edges.push_back(Edge(3, 5, 12));
    edges.push_back(Edge(4, 5, 3));

    int mst_weight = kruskal(n, edges);
    cout << "Total weight of MST: " << mst_weight << endl;

    return 0;
}

```

---

### Gotham Rises #gotham_Rises
The city consists of _N_ blocks. The mayor also has to ensure that every block has access to at least one clinic. The connectivity is established by repairing the existing roads.

The cost to build a new clinic is _C_ and to repair a road is _r_. 

Since you are working for the Mayor and want to help the Batman, your job is to find the minimum cost such that for every block in the city. 

- Either there is a clinic in the block 
- Or the block has a path via repaired roads to a block with a clinic.
##### Input Format

First-line comprises T - the number of test cases.

For each test case, the first line comprises four space-separated integers N R c r - number of blocks, number of roads, the cost for building a clinic, the cost for repairing a road. 

Next R lines comprise 2 space-separated integers _x, y_ - denoting road connection(undirected) between block _x_ and block _y._

##### Output Format

Output the minimum cost for each test case on a new line.

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef long double ld;
#define F first
#define S second
const ll Z = 1e9 + 7;
const int N = 1e5 + 7;

struct unionfind
{
    int n,cmp;
    vector<int> rank;
    vector<int> par;
    unionfind(){};
    unionfind(int a)
    {
        n=cmp=a;
        rank.resize(n+1,1);
        par.resize(n+1);
        for(int i=1;i<=n;i++)
        {
            par[i]=i;
        }
    }

    int find(int x)
    {
        if(x==par[x])return x;
        else return par[x]=find(par[x]);
    }

    void merge(int x, int y)
    {
        int xroot=find(x);
        int yroot=find(y);
        if(xroot!=yroot)
        {
            if(rank[xroot]<=rank[yroot])
            {
                par[xroot]=yroot;
                rank[yroot]+=rank[xroot];
            }
            else
            {
                par[yroot]=xroot;
                rank[xroot]+=rank[yroot];
            }
            cmp--;
        }
    }
};

void solve()
{
    int n,m;
    ll r,c;
    cin >> n >> m >> c >> r;
    unionfind uf(n+1);
    vector<pair<ll,pair<int,int>>> edge;
    for(int i=0;i<m;i++)
    {
        int x,y;
        cin >> x >> y;
        edge.push_back({r,{x,y}});
    }
    for(int i=1;i<=n;i++)
    {
        edge.push_back({c,{i,n+1}});
    }

    sort(edge.begin(),edge.end());
    ll ans=0;
    for(int i=0;i<edge.size();i++)
    {
        int a=edge[i].S.F;
        int b=edge[i].S.S;
        ll c=edge[i].F;
        if(uf.find(a)!=uf.find(b))
        {
            ans+=c;
            uf.merge(a,b);
        }
    }
    cout << ans << endl;
}

int main()
{
ios_base::sync_with_stdio(0);
cin.tie(0);cout.tie(0);
int t; cin>>t; while(t--)
solve();
return 0;
}
```