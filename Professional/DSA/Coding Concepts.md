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


### The difference between `std::endl` and `\n` in C++ lies in how they handle output: #endl #/n

- **`endl`**: Inserts a newline character **and** flushes the output buffer, which ensures all output is immediately sent to the destination (console, file, etc.). This can cause slight performance overhead due to the flushing process.
    
- **`\n`**: Only inserts a newline character without flushing the buffer. It's faster since the output buffer is only flushed periodically or when needed.
    

In most cases, use `\n` for better performance unless immediate flushing is necessary.