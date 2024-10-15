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
#####   Description

You have a 2-D array of size **N x M**. Consider connected **0**s (which share a common edge) as one single component and **1**s as walls. Replace **0**s with the size of the connected component but if the size of the component is one, then leave it with **0**.

##### Input Format

The first line contains a single integer **t**, the number of test cases.  
For each test case, the first line contains two integers **N** and **M** and then there are N lines containing M 0s and 1s, representing a N x M binary matrix.

##### Output Format

For each test case, print the final matrix after replacing all the 0s accordingly.

##### Constraints

1 ≤ Sum of (N x M) over all test cases ≤ 2 x 105  
0 ≤ Ai ≤ 1

##### Sample Input 1

2 
2 2
0 1 
1 0 
6 5 
1 0 0 1 0 
0 1 0 0 0 
0 0 1 1 0 
0 1 1 0 1 
1 1 1 1 1 
0 1 0 0 0

##### Sample Output 1

0 1 
1 0 
1 7 7 1 7 
4 1 7 7 7 
4 4 1 1 7 
4 1 1 0 1 
1 1 1 1 1
0 1 3 3 3

```cpp
#include <bits/stdc++.h>
using namespace std;

  
#define pii pair<int, int>
#define mp make_pair

void bfs(vector<vector<int>> &arr, vector<vector<int>> &vis, int i, int j)

{

    int dx[] = {0, 0, 1, -1};

    int dy[] = {1, -1, 0, 0};

    int n = arr.size(), m = arr[0].size();

  

    vis[i][j] = 2;

    int sz = 1;

  

    queue<pii> q;

    q.push(mp(i, j));

    while (!q.empty())

    {

        int x = q.front().first, y = q.front().second;

        q.pop();

        for (int k = 0; k < 4; k++)

        {

            int nx = x + dx[k], ny = y + dy[k];

            if ((nx >= 0 && nx < n && ny >= 0 && ny < m) && vis[nx][ny] == 0)

            {

                vis[nx][ny] = 2, sz++;

                q.push(mp(nx, ny));

            }

        }

    }

  

    vis[i][j] = 1; //for single zeros

    if (sz == 1)

        return;

    arr[i][j] = sz;

  

    q.push(mp(i, j));

    while (!q.empty())

    {

        int x = q.front().first, y = q.front().second;

        q.pop();

        for (int k = 0; k < 4; k++)

        {

            int nx = x + dx[k], ny = y + dy[k];

            if ((nx >= 0 && nx < n && ny >= 0 && ny < m) && vis[nx][ny] == 2)

            {

                vis[nx][ny] = 1, arr[nx][ny] = sz;

                q.push(mp(nx, ny));

            }

        }

    }

}

  

void solve()

{

    int n, m;

    cin >> n >> m;

    vector<vector<int>> arr(n, vector<int>(m));

    for (int i = 0; i < n; i++)

        for (int j = 0; j < m; j++)

            cin >> arr[i][j];

  

    // <0 -> not visited, 1 -> completed, 2 -> visited but not completed>

    vector<vector<int>> vis(n, vector<int>(m));

    for (int i = 0; i < n; i++)

        for (int j = 0; j < m; j++)

            vis[i][j] = arr[i][j];

  

    for (int i = 0; i < n; i++)

        for (int j = 0; j < m; j++)

            if (vis[i][j] == 0)

                bfs(arr, vis, i, j);

    for (int i = 0; i < n; i++)

    {

        for (int j = 0; j < m; j++)

            cout << arr[i][j] << " ";

        cout << "\n";

    }

}

```