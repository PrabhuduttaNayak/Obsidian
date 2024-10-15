22/09/2024 00:26

Tags: #bfs #graph_area #graph_perimeter 

Status:[[BFS]]

# Area and Perimeter of Connected Components

##### Description

You have been given a grid of size _N_ x _N._ Each cell is either empty (.) or occupied (#). Size of each cell is 1 x 1. In the connected component, you can reach any cell from every other cell in the component by repeatedly stepping to adjacent cells in the north, south, east, and west directions.   
Your task is to find the area and perimeter of the connected component having the largest area. The area of a connected component is just the number of '#' characters that are part of it. If multiple connected components tie for the largest area, find the smallest perimeter among them.

```cpp
#include <bits/stdc++.h>

#define endl '\n'
#define mod 1000000007
using namespace std;

  
vector<vector<char>> g;
vector<vector<int>> dist;

int n;
int dx[] = {-1, 0, 1, 0};
int dy[] = {0, 1, 0, -1};

  

void bfs(int start_x, int start_y, int col) {

    queue<pair<int, int>> q;
    q.push({start_x, start_y});
    dist[start_x][start_y] = col;

    while (!q.empty()) {
        auto node = q.front();
        q.pop();

        for (int k = 0; k < 4; k++) {
            int x = node.first + dx[k];
            int y = node.second + dy[k];

     if (x >= 0 && y >= 0 && x < n && y < n && dist[x][y] == 0 && g[x][y] == '#') {
                dist[x][y] = col;
                q.push({x, y});
            }
        }
    }
}
  

int main() {

    typedef vector<int> vi;
    typedef pair<int, int> pi;
    ios_base::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
  
    cin >> n;
    g.resize(n, vector<char>(n));
    dist.resize(n, vector<int>(n, 0));

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            cin >> g[i][j];
        }
    }

    int col = 0;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            if (dist[i][j] == 0 && g[i][j] == '#') {
                col++;
                bfs(i, j, col);
            }
        }
    }

  

    map<int, int> mp_area, mp_perim;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
        
            if (dist[i][j] != 0) {  //for area
                mp_area[dist[i][j]]++;

                for (int k = 0; k < 4; k++) { //for perimeter
                    int x = i + dx[k];
                    int y = j + dy[k];
                    if (x < 0 || y < 0 || x >= n || y >= n || g[x][y] == '.') {
                        mp_perim[dist[i][j]]++;
                    }
                }
            }
        }
    }

    int max_area = 0, min_perim = INT_MAX;
    for (auto &entry : mp_area) {

        int area = entry.second;
        int perim = mp_perim[entry.first];
//if area is same take the area with less perimeter
        if (area > max_area || (area == max_area && perim < min_perim)) { 
            max_area = area;
            min_perim = perim;
        }
    }

    cout << max_area << " " << min_perim << endl;
    return 0;

}
```


### Examples

#Multisource_BFS 
##### Description

Your city is having people infected with a virus. The city in which you live is represented as a grid consisting of **n** rows and **m** columns. Cells containing **2** are the cells where the people infected with the virus are present and the cells having **1** are the cells having people not yet infected with the virus. There are certain empty cells which are represented by **0**. The infected people in a unit time can infect **all** their adjacent cells, i.e, if they are present at cell [i, j] they can infect cells [i-1, j], [i+1, j], [i, j-1] and [i, j+1]. The virus **cannot** pass through empty cells. Your task is to print the minimum time in which all the people are infected with the virus. If the virus cannot infect everyone, print **-1**.

##### Input Format

The first line contains two integers **n** and **m**  — the number of rows and columns, respectively.  
The following **n** lines contain **m** integers each, the j-th element in the i-th line is the number written in the j-th cell of the i-th row.

##### Output Format

Print the minimum time in which everyone can be infected or **-1** if everyone cannot be infected.

##### Constraints

```
1 <= n <= 1000
1 <= m <= 1000
```

Copy

##### Sample Input 1

3 3 
0 1 2 
0 1 2 
1 1 2

##### Sample Output 1

2

##### Sample Input 2

3 3 
0 1 2 
0 1 2 
1 0 2

##### Sample Output 2

-1

```cpp
#include <bits/stdc++.h>

  

using namespace std;

  

#define int long long

#define double long double

  
  

int dx[]={0,0,1,-1};

int dy[]={1,-1,0,0};

  

signed main(){

ios_base::sync_with_stdio(false);

cin.tie(0); cout.tie(0);

  

    int n, m;

    cin >> n >> m;

    vector <vector <int>>grid(n, vector <int> (m));

    for (int i = 0; i < n; i++) {

        for (int j = 0; j < m; j++) {

            cin >> grid[i][j];

        }

    }

    vector <vector <int>> vis(n, vector <int> (m, 1e9));

    queue <pair<int, int>> q;

    for (int i = 0; i < n; i++) {

        for (int j = 0; j < m; j++) {

            if (grid[i][j] == 2) {

                vis[i][j] = 0;

                q.push({i, j});

            }

        }

    }

    while(!q.empty()){

        auto cur = q.front();

        q.pop();

        int x,y;

        for(int i = 0; i <4;i++){

            x=cur.first+dx[i];

            y=cur.second+dy[i];

            if(x>=0 && x<n && y>=0 && y<m && grid[x][y]!=0 && vis[x][y]==1e9){

                vis[x][y]=vis[cur.first][cur.second]+1;

                q.push({x,y});

            }

        }

    }

  

    int ans = 0;

    for (int i = 0; i < n; i++) {

        for (int j = 0; j < m; j++) {

            if (grid[i][j] != 0) {

                ans = max(ans, vis[i][j]);

            }

        }

    }

    if (ans == 1e9)

        ans = -1;

    cout << ans;

  
  
  

    return 0;

}
```