21/09/2024 19:50

Tags: #bfs #Knight #girth

Status:

# BFS
##### Description

You are given an _N_×_N_ chessboard and a knight with starting position _(Sx, Sy)_. You are given a final position _(Fx, Fy)_. You have to find the minimum number of moves required to reach the final position. 
Here [[Coding Concepts#Knight move]] is used

```cpp
#include <bits/stdc++.h>
using namespace std;
using state = pair<int, int>;
vector<vector<bool> > vis;
vector<vector<int> > dis;

const int dx[] = {2, 1, -1, -2, -2, -1, 1, 2};
const int dy[] = {-1, -2, -2, -1, 1, 2, 2, 1};

inline bool check(int x, int y, int n)
{
    if (x >= 0 && x < n && y < n && y >= 0)
    {
        return 1;
    }
    return 0;
}

vector<state> neighbours(state node, int n)
{
    vector<state> neigh;
    for (int i = 0; i < 8; i++)
    {
        int ndx = dx[i] + node.first;
        int ndy = dy[i] + node.second;
        if (check(ndx, ndy, n))
        {
            neigh.push_back({ndx, ndy});
        }
    }
    return neigh;
}

void bfs(int n, state st_node, state en_node)
{

	//always initialize here the vectors:
    vis.assign(n, vector<bool>(n, 0));
    dis.assign(n, vector<int>(n, -1));

    queue<state> q;
    vis[st_node.first][st_node.second] = 1; //always mark the 1st visited node here
    dis[st_node.first][st_node.second] = 0;
    
    q.push(st_node);
    while (!q.empty())
    {
        state node = q.front();
        q.pop();

        if (node == en_node) break; //base case for final node

        for (auto v : neighbours(node, n))
        {
            if (!vis[v.first][v.second])
            {
                vis[v.first][v.second] = 1;
                dis[v.first][v.second] = 1 + dis[node.first][node.second];
                q.push(v);
            }
        }
    }
}

int KnightWalk(int n, int sx, int sy, int fx, int fy)
{
    state st, end;
    st = {sx-1, sy-1};
    end = {fx-1, fy-1};
    bfs(n, st,end);

    return dis[fx-1][fy-1];
}

int main()
{
    ios_base::sync_with_stdio(0);
    cin.tie(NULL);
    cout.tie(NULL);

    int test_case;
    cin >> test_case;

    while (test_case--)
    {
        int N, Sx, Sy, Fx, Fy;
        cin >> N >> Sx >> Sy >> Fx >> Fy;

        cout << KnightWalk(N, Sx, Sy, Fx, Fy) << "\n";
    }
}
```

##### Description #shortest_cycle

Given an undirected graph, your task is to determine its girth, i.e., the length of its shortest cycle.

>[!Note] What does a cycle through node 1 look like?
> In any cycle through node 1, there exists two nodes u and v on that cycle such that there is a path from 1 to u and 1 to v, and there is an edge between u and v. The length of this cycle is dist(1,u)+dist(1,v)+1.

```cpp
for (auto v : g[u]) {
                if (dist[v] == INF) {
                    dist[v] = dist[u] + 1;
                    parent[v] = u;
                    q.push(v);
                } else if (parent[u] != v) {
                    // Found a cycle
                    girth = min(girth, dist[u] + dist[v] + 1);
                }
            }
```
### Examples