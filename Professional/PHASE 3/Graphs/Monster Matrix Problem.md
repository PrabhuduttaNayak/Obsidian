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


---
# #Budget_Travelling

##### Description

You want to visit the country of wonderland. There are **N** cities in the country. Not all cities are connected by roads. You know which cities are connected. 

You landed in city **A**, and you want to visit city **B.** You already booked your car, but it doesn’t have any petrol. The capacity of the tank of the car is **C.** You know the **Per Liter cost** of petrol in each city, and you also have the map of the country, i.e., you know the length of road between two cities. **To travel one unit of distance, you need one liter of petrol.**

Find the minimum cost to travel from city **A** to city **B**.

##### Input Format

The first line of input contains **N -** the number of cities in the country of wonderland.  
The second line contains **M** - the number of roads in the country.  
Next **M** lines contain three integers **u**, **v**, **d** - there is a road between city **u** and **v** of length **d**.  
The next lines contain **N** space-separated integers P[1], P[2], …., P[N] - **P[i]** is the per liter cost of petrol in the city **i**.  
The last line of input contains **A**, **B**, and **C** - the starting city, the destination city, and the capacity of the tank of car you booked.  
It’s guaranteed that it’s always possible to reach city **B** from **A**.

##### Output Format

Print the minimum cost to reach city **B** on a new line.

##### Constraints

1 ≤ N ≤ 10000  
1 ≤ M ≤ 100000  
1 ≤ C ≤ 100  
1 ≤ u, v ≤ N  
1 ≤ d ≤ C  
1 ≤ A, B ≤ N  
1 ≤ P[i] ≤ 100  
 

##### Sample Input 1

5 
5 
1 2 1 
2 3 1 
3 4 1 
4 5 1 
1 4 6 
1 10 10 10 1 
1 5 8

##### Sample Output 1

4

##### Sample Input 2

6 
6 
1 2 1
2 3 1 
3 4 1 
4 5 1 
1 6 1
6 5 5 
10 10 10 10 1 1 
1 5 8

##### Sample Output 2

15

```cpp
#include <bits/stdc++.h>

using namespace std;

  

#define F first

#define S second

#define MP make_pair

using ii = pair<int, int>;

  

int n, m, C;

vector<ii> g[10010];

int P[10010];

  

int dist[10001][101];

int vis[10001][101];

  

int dijk(int st, int en)

{

    for (int i = 1; i <= n; i++)

    {

        for (int j = 0; j <= C; j++)

        {

            dist[i][j] = 1e9;

            vis[i][j] = 0;

        }

    }

  

    priority_queue<pair<int, ii>> pq;

    pq.push(MP(0, MP(st, 0)));

    dist[st][0] = 0;

    while (!pq.empty())

    {

        pair<int, ii> cur = pq.top();

        pq.pop();

        int node = cur.S.F;

        int fuel = cur.S.S;

        int dis = -cur.F;

  

        if (vis[node][fuel] == 1)

            continue;

        vis[node][fuel] = 1;

  

        if (fuel < C && dis + P[node] < dist[node][fuel + 1])

        {

            dist[node][fuel + 1] = dis + P[node];

            pq.push(MP(-dist[node][fuel + 1], MP(node, fuel + 1)));

        }

        for (auto v : g[node])

        {

            if (fuel >= v.S && dist[v.F][fuel - v.S] > dis)

            {

                dist[v.F][fuel - v.S] = dis;

                pq.push(MP(-dist[v.F][fuel - v.S], MP(v.F, fuel - v.S)));

            }

        }

    }

    int mini = 1e9;

    for (int i = 0; i <= C; i++)

    {

        mini = min(mini, dist[en][i]);

    }

  

    assert(mini < 1e9);

    return mini;

}

  

void solve()

{

  

    cin >> n;

    cin >> m;

    for (int i = 0; i < m; i++)

    {

        int u, v, w;

        cin >> u >> v >> w;

        g[u].push_back({v, w});

        g[v].push_back({u, w});

    }

    for (int i = 1; i <= n; i++)

    {

        cin >> P[i];

    }

    int st, en;

    cin >> st >> en >> C;

  

    cout << dijk(st, en) << "\n";

}

  

signed main()

{

    ios_base::sync_with_stdio(0);

    cin.tie(0);

    cout.tie(0);

    //int _t;cin>>_t;while(_t--)

    solve();

}
```

---
# #Snakes_and_Ladder
##### Description

Abhishek loves Snakes and Ladders game, he can always roll the die to whatever number he want between 11 to 66. Help him to find the least number of dice rolls to reach the destination i.e the 100th100th cell from the 1st1st cell.

Rules :-

1. The game is played with a cubic die of 66 faces numbered 11 to 66.
    
2. Starting from square 11, land on square 100100 with the exact roll of the die. If moving the number rolled would place the player beyond square 100100, no move is made.
    
3. If a player lands at the base of a ladder, the player must climb the ladder. Ladders go up only.
    
4. If a player lands at the mouth of a snake, the player must go down the snake and come out through the tail. Snakes go down only.
    

##### Input Format

The first line contains the number of tests, tt.

For each test case:

- The first line contains nn, the number of ladders.
- Each of the next nn lines contains two space-separated integers, the start, and end of a ladder.
- The next line contains the integer mm, the number of snakes.
- Each of the next mm lines contains two space-separated integers, the start, and end of a snake.

##### Output Format

For each of the tt test cases, print the least number of rolls to move from start to finish on a separate line. If there is no solution, print −1−1.

##### Constraints

1≤t≤101≤t≤10 1≤n,m≤151≤n,m≤15

The board is always 10×1010×10 with squares numbered 11 to 100100 . Neither square 11 nor square 100100 will be the starting point of a ladder or snake. A square will have at most one endpoint from either a snake or a ladder.

##### Sample Input 1

2 3 32 62 42 68 12 98 7 95 13 97 25 93 37 79 27 75 19 49 47 67 17 4 8 52 6 80 26 42 2 72 9 51 19 39 11 37 29 81 3 59 5 79 23 53 7 43 33 77 21

##### Sample Output 1

3 5

# Solution Approach

![coin](https://maang.in/img/coding/up.svg)

**Modeling the board as a graph** Let's model the board as a graph. Every square on the board is a node. The source node is square 11. The destination node is square 100100. From every square, it is possible to reach any of the 66 squares in front of it in one move. So every square has a directed edge to the 66 squares in front of it.

**Handling the snakes and ladders** For a snake starting at square ii and finishing at jj, we can consider that there is no node with index ii in the graph. Because reaching node ii is equivalent to reaching node jj since the snake at node ii will immediately take us down to node jj. The same analogy goes for ladders too. To handle the snakes and ladders, let's keep an array _go_immediately_to[]_ and initialize it like this.

```
        if(no snake or ladder starts at node i)
            go_immediately_to[i] = i.
        else
            j = ending node of the snake or the ladder starting at node i.
            go_immediately_to[i] = j.
```

Now let's run a standard Breadth-First Search(BFS). Whenever we reach a node ii, we will consider that we have reached the node _go_immediately_to[i]_ and then continue with the BFS as usual. The distance of the destination is the solution to the problem.

**Time Complexity**

The size of the board is always 10×1010×10. You can consider time complexity of each BFS is constant as the number of snakes or ladders won't have much effect. There are T test cases; total complexity is O(T)O(T) omitting the constant factor of 10×1010×10.

```cpp
#include<bits/stdc++.h>

using namespace std;

  

int n, m;

queue<int> q;

int arr[110], dist[110];

bool vis[110];

bool isValid(int node)

{

    if(node < 1 || node > 100 || vis[node])

        return false;

    else

        return true;

}

int BFS(int source)

{

    memset(vis, 0, sizeof(vis));

    while(!q.empty())

        q.pop();

  

    vis[source] = 1;

    q.push(source);

    dist[source] = 0;

    while(!q.empty())

    {

        int current_node = q.front();

        q.pop();

        for(int i = 1; i<=6; i++)

        {

            int next_node = arr[current_node+i];

            if(isValid(next_node))

            {

                q.push(next_node);

                vis[next_node] = 1;

                dist[next_node] = dist[current_node]+1;

            }

        }

    }

    if(!vis[100])

        return -1;

    else

        return dist[100];

}

int main()

{

    int i, j, cs, t, u, v;

    cin >> t;

    for(cs = 1; cs<=t; cs++)

    {

        cin >> n;

  

        for(i = 1; i<=100; i++)

            arr[i] = i;

  

        for(i = 1; i<=n; i++)

        {

            cin >> u >> v;

            arr[u] = v;

        }

  

        cin >> m;

  

        for(i = 1; i<=m; i++)

        {

            cin >> u >> v;

            arr[u] = v;

        }

  

        cout << BFS(1) << endl;

    }

  

}
```