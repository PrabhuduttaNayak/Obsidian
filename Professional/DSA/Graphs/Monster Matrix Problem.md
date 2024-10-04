22/09/2024 00:20

Tags: #Multisource_BFS #bfs #unsolved 

Status: [[BFS]]

# Monster Matrix Problem

##### Description

You and some monsters are in a matrix. When taking a step to some direction in the matrix, each monster may simultaneously take one as well. Your goal is to reach one of the boundary squares without ever sharing a square with a monster.  
Your task is to find out if your goal is possible, and if it is, print the _shortest_ _length of_ _the_ _path_ that you can follow. Your plan has to work in any situation; even if the monsters know your path beforehand.

##### Input Format

The first input line has two integers _n_ and _m_: the height and width of the matrix.  
After this, there are _n_ lines of _m_ characters describing the matrix. Each character is ‘.’ (floor), ‘#’ (wall), ‘A’ (start), or ‘M’ (monster). There is exactly one ‘A’ in the input.

##### Output Format

First, print "YES" if your goal is possible, and "NO" otherwise.  
If your goal is possible, also print the length of the shortest path that you'll follow.

##### Constraints

1 ≤ _n, m_ ≤ 1000

```
5 8
########
#M..A..#
#.#.M#.#
#M#..#..
#.######
```
----
YES
5

```
3 3
###
#A#
#M.
```
no

----
```
1 3
##A
```
YES
0
### CODE
```cpp
#include <bits/stdc++.h>
using namespace std;

#define ll long long int

const int N = 1010;

int mod = 1e9 + 7;

int dx[4] = {-1, 1, 0, 0};
int dy[4] = {0, 0, -1, 1};

bool grid[N][N];
int distA[N][N];
queue<pair<int, int>> monsterOcc, AOcc;
pair<int, int> par[N][N];
int distMon[N][N];

signed main()
{
    ios_base::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);

    int n, m;
    cin >> n >> m;

    memset(grid, false, sizeof(grid));

    memset(distMon, -1, sizeof(distMon));
    memset(distA, -1, sizeof(distA));

    for (int i = 0; i < n; i++)
    {
        string s;
        cin >> s;
        for (int j = 0; j < m; j++)
        {
            grid[i][j] = true;
            if (s[j] == '#')
                grid[i][j] = false;
            else if (s[j] == 'M')
            {
                distMon[i][j] = 0;
                monsterOcc.push({i, j});
            }
            else if (s[j] == 'A')
            {
                distA[i][j] = 0;
                AOcc.push({i, j});
                par[i][j] = {-1, -1};
            }
        }
    }

    while (!monsterOcc.empty())
    {
        auto it = monsterOcc.front();
        monsterOcc.pop();
        int x = it.first, y = it.second;

        for (int i = 0; i < 4; i++)
        {
            int xx = x + dx[i], yy = y + dy[i];
            if (xx < 0 || xx >= n || y < 0 || yy >= m)
                continue;
            if (grid[xx][yy] && distMon[xx][yy] == -1)
            {
                distMon[xx][yy] = distMon[x][y] + 1;
                monsterOcc.push({xx, yy});
            }
        }
    }

    while (!AOcc.empty())
    {
        auto it = AOcc.front();
        AOcc.pop();
        int x = it.first, y = it.second;

        for (int i = 0; i < 4; i++)
        {
            int xx = x + dx[i], yy = y + dy[i];
            if (xx < 0 || xx >= n || y < 0 || yy >= m)
                continue;
            if (grid[xx][yy] && distA[xx][yy] == -1)
            {
                distA[xx][yy] = distA[x][y] + 1;
                AOcc.push({xx, yy});
                par[xx][yy] = {x, y};
            }
        }
    }

    int finx = -1, finy = -1, findist = 1e9;

    for (int i = 0; i < n; i++)
    {
        if (grid[i][0] && distA[i][0] >= 0 && (distA[i][0] < distMon[i][0] || distMon[i][0] == -1))
        {
            finx = i;
            finy = 0;
            findist = min(findist, distA[i][0]);
        }
        if (grid[i][m - 1] && distA[i][m - 1] >= 0 && (distA[i][m - 1] < distMon[i][m - 1] || distMon[i][m - 1] == -1))
        {
            finx = i;
            finy = m - 1;
            findist = min(findist, distA[i][m - 1]);
        }
    }

    for (int i = 0; i < m; i++)
    {
        if (grid[0][i] && distA[0][i] >= 0 && (distA[0][i] < distMon[0][i] || distMon[0][i] == -1))
        {
            finx = 0;
            finy = i;
            findist = min(findist, distA[0][i]);
        }
        if (grid[n - 1][i] && distA[n - 1][i] >= 0 && (distA[n - 1][i] < distMon[n - 1][i] || distMon[n - 1][i] == -1))
        {
            finx = n - 1;
            finy = i;
            findist = min(findist, distA[n - 1][i]);
        }
    }

    if (finx == -1)
        cout << "NO\n";
    else
    {
        cout << "YES\n";
        cout << findist << "\n";
    }

    return 0;
}
```