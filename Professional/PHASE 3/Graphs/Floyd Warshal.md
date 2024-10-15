14/10/2024 23:50

Tags:

Status:

# Floyd Warshal







### Examples

##### Description

There are _n_ cities and _m_ roads between them. Your task is to process _q_ queries where you have to determine the length of the shortest route between two given cities.

##### Input Format

The first input line has three integers _n_, _m_ and _q_: the number of cities, roads, and queries.  
Then, there are _m_ lines describing the roads. Each line has three integers _a_, _b_ and _c_: there is a road between cities _a_ and _b_ whose length is _c_. All roads are two-way roads.  
Finally, there are _q_ lines describing the queries. Each line has two integers _a_ and _b_: determine the length of the shortest route between cities _a_ and _b_.

##### Output Format

Print the length of the shortest route for each query. If there is no route, print −1 instead.

##### Constraints

1 ≤ _n_ ≤ 500  
1 ≤ _m_ ≤ _n_2  
1 ≤ _q_ ≤ 105  
1 ≤ _a_, _b_ ≤ _n_  
1 ≤ _c_ ≤ 109

##### Sample Input 1

4 3 5 
1 2 5 
1 3 9 
2 3 3 
1 2 
2 1 
1 3
1 4 
3 2

##### Sample Output 1

5 5 8 -1 3

##### Sample Input 2

2 2 1 
1 2 1 
1 2 2 
1 2

##### Sample Output 2

1

```cpp
#include <bits/stdc++.h>

  

using namespace std;

  

typedef long long ll;

  

int mod = 1e9 + 7;

  

const int N = 100010;

  

signed main()

{

    ios_base::sync_with_stdio(0);

    cin.tie(0); cout.tie(0);

  

    int n, m, q;

    cin >> n >> m >> q;

  

    ll dist[n + 1][n + 1];

    for(int i = 1; i <= n; i++) {

        for(int j = 1; j <= n; j++) {

            dist[i][j] = 1e18;

        }

        dist[i][i] = 0;

    }

  

    for(int i = 0; i < m; i++) {

        int a, b, c;

        cin >> a >> b >> c;

        dist[a][b] = dist[b][a] = min(dist[a][b], 1LL * c);

    }

  

    for(int k = 1; k <= n; k++) {

        for(int i = 1; i <= n; i++) {

            for(int j = 1; j <= n; j++) {

                dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]);

            }

        }

    }

  

    while(q--) {

        int a, b;

        cin >> a >> b;

        if(dist[a][b] == 1e18) cout << "-1\n";

        else cout << dist[a][b] << "\n";

    }

  

}
```

---
##### Description

We have given an adjacency representation of a **directed weighted graph** and an array of vertices. At each iteration, a vertex is removed from the graph. Vertices are removed in the order given in the array. When the vertex is removed, all the edges that go in and out are also removed. 

Print the sum of all pairs shortest path **just before each iteration**.

##### Input Format

The first line contains integer _n_ (1 ≤ _n_ ≤ 500) — the number of vertices in the graph.  
Next _n_ lines contain _n_ integers each — the graph adjacency matrix: the _j_-th number in the _i_-th line _aij_ (1 ≤ _aij_ ≤ 105, _aii_ = 0) represents the weight of the edge that goes from vertex _i_ to vertex _j_.  
The next line contains _n_ distinct integers: _x_1, _x_2, ..., _xn_ (1 ≤ _xi_ ≤ _n_) — the order of vertices removed from the graph.

##### Output Format

Print _N_ space-separated numbers, where _i_th number represents the sum of all pairs shortest path just before _i_th removal.

##### Constraints

1 ≤ _N_ ≤ 500

##### Sample Input 1

2 
0 5 
4 0 
1 2

##### Sample Output 1

9 0

##### Sample Input 2

4 
0 3 1 1
6 0 400 1
2 4 0 1 
1 1 1 0
4 1 2 3

##### Sample Output 2

17 23 404 0

```cpp
#include<bits/stdc++.h>

using namespace std;

int n;

vector<vector<int>> dis;

vector<int> ord;

vector<int> ans;

void solve(){

    cin>>n;

    dis.assign(n,vector<int>(n,1e9));

    for(int i=0;i<n;i++){

        for(int j=0;j<n;j++){

            int x;

            cin>>x;

            dis[i][j]=x;

        }

    }

    for(int i=0;i<n;i++){

        int x;

        cin>>x;

        ord.push_back(x-1);

    }

    for(int k=n-1;k>=0;k--){

        for(int i=0;i<n;i++){

            for(int j=0;j<n;j++){

                dis[i][j]=min(dis[i][j],dis[i][ord[k]]+dis[ord[k]][j]);

            }

        }

        int temp=0;

        for(int i=k;i<n;i++){

            for(int j=k;j<n;j++){

                temp+=dis[ord[i]][ord[j]];

            }

        }

        ans.push_back(temp);

    }

    reverse(ans.begin(),ans.end());

    for(auto it:ans){

        cout<<it<<" ";

    }

    cout<<"\n";

  

}

  

int main(){

    int t=1;

    // cin>>t;

    while(t--){

        solve();

    }

}
```