21/09/2024 12:20

Tags: #neighbour

Status:

# Neighbour navigate
#### To navigate in all 8 direction

```cpp
int dx[8] = {1, -1, 0, 0, 1, 1, -1, -1};
int dy[8] = {0, 0, 1, -1, 1, -1, 1, -1};

        for (int i = 0; i < 8; i++)
        {
            int nx = x + dx[i];
            int ny = y + dy[i];
            if (valid(nx, ny, n, m) && arr[nx][ny] == '.')
            {
                sur = false;
                break;
            }
        }
```

----
#### To navigate in 4 directions

```cpp
int dx[]={0,0,1,-1}; // Updated to 4-directional movement
int dy[]={1,-1,0,0}; // Updated to 4-directional movement

for(int i=0;i<4;i++){
        int x=s.first+dx[i];
        int y=s.second+dy[i];

        if(isValid(make_pair(x,y)) && !vis[x][y]){
            dfs(make_pair(x,y), comp);
        }
```

#### #Knight move 

```cpp
const int dx[] = {2, 1, -1, -2, -2, -1, 1, 2};
const int dy[] = {-1, -2, -2, -1, 1, 2, 2, 1};
```


### Examples