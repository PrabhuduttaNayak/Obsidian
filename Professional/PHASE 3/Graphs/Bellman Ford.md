14/10/2024 23:49

Tags:

Status:

# Bellman Ford







### Examples
##### Description

You have given a graph _G_ with _n_ nodes and _m_ edges. Each edge has an integer weight associated with. The weight of an edge may negative, positive or zero. **If the graph contains a cycle with total weight > 0, print -1. Otherwise, find the weight of the highest weighted path from node 1 to** _**n**_**.**

##### Input Format

The first input line has two integers _n_ and _m_: the number of nodes and edges. The nodes are numbered 1, 2, …, _n_.  
Then, there are _m_ lines describing the edges. Each line has three integers _a_, _b_ and _x_: the edge starts at node _a_, ends at node _b_, and weight of the edge is _x_. All edges are unidirectional edges.  
You can assume that it is possible to get from node 1 to node _n_.

##### Output Format

Print the answer on a new line.

##### Constraints

1 ≤ 2500 ≤ _n_  
1 ≤ 5000 ≤ _m_  
1 ≤ _a_, _b_ ≤ _n_  
−109 ≤ _x_ ≤ 109

##### Sample Input 1

4 5 
1 2 3 
2 4 -1
1 3 -2 
3 4 7 
1 4 4

##### Sample Output 1

5
# Solution Approach

![coin](https://maang.in/img/coding/up.svg)

Construct a new graph with weights multiplies by -1. Now check for the existence of a negative cycle in this graph. If it does not exist, find the shortest distance from 1 to n. Note that since weights can be negative, use Bellman-Ford Algorithm.
```cpp
#include <bits/stdc++.h>

using namespace std;

#define int long long

#define F first

#define S second

#define ii pair<int, int>

  

int n, m;

vector<vector<int>> g;

vector<int> dist;

  

void solve()

{

    cin >> n >> m;

    g.clear();

    dist.clear();

    dist.resize(n + 1, 1e18);

  

    for (int i = 0; i < m; i++)

    {

        int x, y, w;

        cin >> x >> y >> w;

        g.push_back({{x, y, -w}});

    }

  

    dist[1] = 0;

    for (int i = 0; i < n; i++)

    {

        for (auto edge : g)

        {

            int x = edge[0];

            int y = edge[1];

            int w = edge[2];

            if (dist[x] != 1e15 && dist[y] > dist[x] + w)

            {

                dist[y] = dist[x] + w;

            }

        }

    }

    bool hasCycle = 0;

    for (auto edge : g)

    {

        int x = edge[0];

        int y = edge[1];

        int w = edge[2];

        if (dist[y] > dist[x] + w)

        {

            hasCycle = 1;

            break;

        }

    }

  

    if (hasCycle)

    {

        cout << -1 << '\n';

    }

    else

    {

        cout << -dist[n] << '\n';

    }

}

  

signed main()

{

    ios_base::sync_with_stdio(0);

    cin.tie(0);

    cout.tie(0);

    solve();

    return 0;

}
```