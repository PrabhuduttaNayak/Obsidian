Tag: #vertical_grid #bfs [[BFS]] 

# Description

You have been given a vertical grid of size _N_ x 10, and a number _K_. Since it's vertical, gravity shows its effect on it. Each cell in the grid contains a ball which has some colour. Colour values can vary between 1 to 9_._  
Each cell in the grid is represented by a number, _cij_ for cell (_i, j_). If _cij_ = 0, then the cell (_i, j_) is empty. Otherwise, it contains a ball with colour _cij._ Each cell is either empty (indicated by a 0), or a ball in one of nine different colours (indicated by characters 1..9). Gravity causes balls to fall downward, so there is never a 0 cell below a ball.  
Two cells belong to the same connected region if they are directly adjacent either horizontally or vertically, and they have the same non-zero colour. Any time a connected region exists with at least _K_ cells, its balls all disappear, turning into zeros. If multiple such connected regions exist at the same time, they all disappear simultaneously. Afterwards, gravity might cause balls to fall downward to fill some of the resulting cells that became zeros. In the resulting configuration, there may again be connected regions of size at least _K_ cells. If so, they also disappear (simultaneously, if there are multiple such regions), then gravity pulls the remaining balls downward, and the process repeats until no connected regions of size at least _K_ exist.  
Given the initial vertical grid, your task is to output a final picture of the grid after these operations have occurred.

##### Input Format

The first line of input contains _N_ and _K_. The remaining _N_ lines specify the initial state of the vertical grid.

##### Output Format

Please output _N_ lines, describing a picture of the final vertical grid.

##### Constraints

1 ≤ _N_ ≤ 100  
1 ≤ _K_ ≤ 10N
```
INPUT
6 3
0000000000
0000000300
0054000300
1054502230
2211122220
1111111223

OUTPUT
0000000000
0000000000
0000000000
0000000000
1054000000
2254500000
```
----
```cpp
#include <bits/stdc++.h>

using namespace std;

using state = pair<int, int>;

#define F first
#define S second

int n, k;

vector<vector<int>> vec;
int dx[4] = {1, -1, 0, 0};
int dy[4] = {0, 0, 1, -1};

int bfs(state no, int par) {

    vector<vector<int>> vis(n, vector<int>(10, 0));  // Initialize visit matrix
    vector<state> temp;
    queue<state> q;

    q.push(no);
    vis[no.F][no.S] = 1;
    temp.push_back(no);

    while (!q.empty()) {

        state node = q.front();
        q.pop();

        for (int i = 0; i < 4; i++) {
            int x = node.F + dx[i];
            int y = node.S + dy[i];

            if (x >= 0 && x < n && y >= 0 && y < 10 && vec[x][y] == par && !vis[x][y]) {
                vis[x][y] = 1;
                temp.push_back({x, y});
                q.push({x, y});
            }
        }
    }


    if (temp.size() >= k) {
        for (auto& t : temp) {
            vec[t.F][t.S] = 0;  // Set the connected component to 0
        }
        return 1;
    }
    return 0;
}

  

void solve() {

    cin >> n >> k;
    vec.assign(n, vector<int>(10, 0));
    // Input handling: converting input strings to the 2D grid

    for (int i = 0; i < n; i++) {
        string s;
        cin >> s;
        for (int j = 0; j < 10; j++) {
            vec[i][j] = s[j] - '0';
        }
    }


    while (true) {
        int cnt = 0;
        // Search for all connected components and process them

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < 10; j++) {
                if (vec[i][j] != 0) {
                    cnt += bfs({i, j}, vec[i][j]);
                }
            }
        }

        // If no components were removed, we are done
        if (cnt == 0) break;
        // Gravity effect: Drop the blocks after removal

        for (int j = 0; j < 10; j++) {
            vector<int> a;                 w
            for (int i = n - 1; i >= 0; i--) {
                if (vec[i][j] != 0) {
                    a.push_back(vec[i][j]);
                }
            }
            // Fill the column from the bottom with the remaining blocks
            int k = 0;
            for (int i = n - 1; i >= 0; i--) {
                if (k < a.size()) {
                    vec[i][j] = a[k++];
                } else {
                    vec[i][j] = 0;
                }
            }
        }
    }

    // Output the final grid

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < 10; j++) {
            cout << vec[i][j] ;        }
        cout << endl;
    }
}

int main() {
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    solve();

    return 0;

}
```