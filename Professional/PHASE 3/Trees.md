15/10/2024 09:33

Tags: [[DFS]] [[MST]] 

Status:

# Tree Diameter

##### Description

You are given a tree consisting of _n_ nodes. The diameter of a tree is the maximum distance between two nodes. Your task is to determine the diameter of the tree.
# Solution Approach

Choose any node in the tree as the starting point.

Perform a DFS traversal from the starting node to find the farthest node from it. During the traversal, keep track of the maximum distance reached and the corresponding node.

Once you've reached the farthest node, perform another DFS traversal starting from that node. Again, keep track of the maximum distance reached and the corresponding node.

The maximum distance obtained from the second traversal is the diameter of the tree.

Time Complexity : 2 DFS traversals, O(N)
 
```cpp
#include <bits/stdc++.h>
using namespace std;
#define endl '\n'
#define int long long

vector<int> g[200100];
int dep[200100];
int par[200100];
int numChild[200100];
int subTrees[200100];
int isleaf[200100];

void dfs(int node, int parent, int depth){
    dep[node] = depth;
    par[node] = parent;

	numChild[node]=0;
	subTrees[node]=1;
	
    for(auto v:g[node]){
        if(v!=parent){
	        numChild[node]++;
            dfs(v,node,depth+1);
            subTrees[node]+=subTrees[v];
        }
    }
    if(numChild[node]==0)isleaf[node]=1;  
}   
void solve(){
    int n;
    cin>>n;
    
    for(int i=0;i<n-1;i++){
        int a,b;
        cin>>a>>b;
        g[a].push_back(b);
        g[b].push_back(a);
    }
    if(n==1){
        cout<<0<<endl;;
        return;
    }
    dfs(1,0,0);   // 1st Traversal to find the 1st end point of diameter
    int maxch = 1;
    for(int j=2;j<=n;j++){
        if(dep[j]>dep[maxch]){
            maxch=j;
        }
    }
    
    dfs(maxch,0,0);  //2nd Traversal to find the second end point of diameter
    maxch = 1;
    for(int j=2;j<=n;j++){
        if(dep[j]>dep[maxch]){
            maxch=j;
        }
    }
    cout<<dep[maxch]<<endl;
}

signed main(){
    ios_base::sync_with_stdio(0);
    cin.tie(0);cout.tie(0);
    
    solve();
}
```

>[!Note]
>If you are maintaining a visited array to traverse the dfs instead of a parent array, make sure to reassign the visited vector to values of 0, because then the *if*  condition of the DFS won't occur and the depth vector can't be over written

#### All diameters of a tree have same Centers, number of centers can't be >=2.

To calculate center add the below code to the above code
```cpp
    int d = dep[maxch];
    int a =maxch;

    // cout<<d<<endl;
    if(d%2==0){
        int center = a;
        int x = d/2;
        while(x--){
            a = par[a];
            center = a;
        }
        cout<<center<<endl;
    }
    else{
        cout<<-1<<endl; //if there are 2 centres
    }
```


#### To calculate number of diameters, we count the number of leaves that have leaves with depth= Diameter-1 for each children, and then take Sum of Product of all Pairs for each children

#### Centre and  Centroid are not always same


![[Pasted image 20241017113616.png]]
![[Pasted image 20241017120727.png]]



### How to calculate centroid of a Tree

##### Solution Approach

We can find a centroid in a tree by starting at the root. Each step, loop through all of its children. If all of its children have subtree size less than or equal to N/2​, then it is a centroid. Otherwise, move to the child with a subtree size that is more than N/2​ and repeat until you find a centroid.

Time Complexity : DFS ~ O(N+M)O(N+M)

```cpp
#include <bits/stdc++.h>
using namespace std;

#define ll int64_t  // A macro to define `ll` as a 64-bit integer
#define endl '\n'   

int n;  
vector<vector<int>> adj;  
vector<int> cnt, centroids;  


void dfs(int node, int p) {
    cnt[node] = 1;  
    
    bool is_centroid = true;  // Assume `v` is a centroid until proven otherwise

    for (auto v : adj[node]) {
        if (p == v) continue; 
        dfs(v, node);  
        cnt[node] += cnt[v]; 
        // Check if any subtree rooted at a child node has more than half the total nodes
        // If true, the current `node` cannot be a centroid
        if (cnt[v] > n / 2) is_centroid = false;
    }

    // Now that all children have been processed, check if the part of the tree outside `node`'s subtree is too large
    // If more than half the nodes are outside `node`'s subtree, it can't be a centroid
    if (n - cnt[node] > n / 2) is_centroid = false;
    if (is_centroid) centroids.push_back(node);
}

void solve() {
    cin >> n;  

    adj.assign(n + 1, vector<int>());
    cnt.assign(n + 1, 0);

    int u, v;
    for (int i = 0; i < n - 1; i++) {
        cin >> u >> v;
        adj[u].push_back(v);  
        adj[v].push_back(u);
    }    
    dfs(1, -1);

    // Output the smallest centroid (since there could be multiple centroids)
    cout << *min_element(centroids.begin(), centroids.end()) << endl;
}
```

---
#### Sum of Distances
##### Description

You are given a tree consisting of _n_ nodes. _d(u, v)_ is the distance between nodes _u_ and _v_, or number of edges in between the path connecting two nodes _u_ and _v_. Your task is to find the sum of distances over all possible pairs of nodes.
```cpp
#include <bits/stdc++.h>
using namespace std;
  
#define int long long
#define double long double
  
vector<vector<int>> g;
vector<int> parent;
vector<int> subTree;
vector<int> depth;
vector<pair<int, int>> edge;
  
void dfs(int node, int par, int lvl){
    parent[node]=par;    
    subTree[node]=1;
    depth[node]=lvl;
    for(auto v: g[node]){
        if(v!=parent[node]){
            dfs(v,node,lvl+1);
            subTree[node]+=subTree[v];
        }
    }
}
  
signed main(){
ios_base::sync_with_stdio(false);
cin.tie(0); cout.tie(0);
  
    int n;
    cin>>n;
    g.resize(n+1)

    parent.resize(n+1);
    subTree.resize(n+1);
    depth.resize(n+1);

    for(int i=1; i<n; i++){
        int a,b;
        cin>>a>>b;
        g[a].push_back(b);
        g[b].push_back(a);
        edge.push_back(make_pair(a,b));
    }
  
    int sum=0;
    dfs(1,0,0);
    for(int i=1; i<=n; i++){
     sum+=subTree[i]*(n-subTree[i]);
    }
    cout<<sum<<endl;  

    return 0;
}
```


----
### To find the number of Diameters of a Tree #number_of_Diameters_Tree

1. A tree can have at most two centers. The center of a tree is also the center of its diameter. To find the center(s) of a tree, follow these steps:
    
2. Find the length of the diameter of the tree. The diameter is the longest path between any two nodes in the tree.
    
3. Once you have the length of the diameter, move from the farthest node towards the center by going up a distance of diameter/2 nodes.
    
4. If the length of the diameter (diameter) is even, there are two centers in the tree. These centers are located at nodes ⌊diameter2⌋⌊2diameter​⌋ and ⌊diameter2⌋−1⌊2diameter​⌋−1 on the diameter path.
    

By identifying the length of the diameter and then moving towards the center, you can determine the center(s) of the tree. If the diameter length is even, there will be two centers at equal distances from both ends of the diameter.

Lets approach both the cases :

### Case 1 : 1 center

1. Consider the center of the tree, let's call it Center.
    
2. Imagine the tree to be rooted at the Center.
    
3. Traverse each adjacent node from the Center and consider them as separate subtrees.
    
4. For each subtree, determine the number of leaf nodes that are located at a depth of ⌊diameter2⌋⌊2diameter​⌋ from the root of that subtree. (The diameter is the maximum distance between two nodes in the tree.)
    
5. Calculate the summation of all possible pairwise products of the number of leaf nodes in each subtree obtained from step 4. This will give you the count of all possible diameters in the tree when there is only one center.
    

The idea behind this approach is that each adjacent node from the center represents a separate subtree. By finding the number of leaf nodes at a depth of diameter/2 in each subtree and considering pairwise combinations, you account for all possible diameters that can be formed by connecting leaf nodes from different subtrees via the center.

By following these steps, you can determine the count of all possible diameters in a tree when there is one center.

### Case 2 : 2 centers

1. Consider the two centers of the tree. Let's call them Center1 and Center2.
    
2. Start by imagining the tree to be rooted at Center1. From Center1, explore each adjacent node and consider them as separate subtrees.
    
3. For each subtree, determine the number of leaf nodes that are located at a depth of ⌊diameter2⌋⌊2diameter​⌋ from the root of that subtree. (The diameter is the maximum distance between two nodes in the tree.)
    
4. Repeat the same process, but this time imagine the tree to be rooted at Center2. Explore each adjacent node from Center2 and determine the number of leaf nodes at a depth of ⌊diameter2⌋⌊2diameter​⌋ from the root of each subtree.
    
5. For each subtree on the side of Center1, and for each subtree on the side of Center2, calculate the number of leaf nodes satisfying the condition mentioned in step 3 and step 4.
    
6. Finally, find the summation of all possible pairwise products of the number of leaf nodes in each subtree obtained from step 5. This will give you the count of all possible diameters in the tree.
    

By following these steps, you can determine the count of all possible diameters in a tree with 2 centers.

```cpp
#include <bits/stdc++.h>
using namespace std;

#define int long long
#define pb push_back
typedef vector<int> vi;

int n;
vector<vector<int>> tree;
vector<int> lev, parent;
vector<int> coun;
int cnt1, cnt2, cnt=0;

void dfs(int node, int par, int dis) {
    lev[node] = dis;
    parent[node] = par;
    for (auto ele : tree[node]) {
        if (ele != par) {
            dfs(ele, node, dis + 1);
        }
    }
}

void dfs1(int node, int par, int l,int val) {//dfs directly calculates the number of nodes
    lev[node] = l;
    if (lev[node] == val) cnt++;
    for (auto ele : tree[node]) {
        if (ele != par) {
            dfs1(ele, node, l+1,val);
        }
    }
}

void solve() {
    cin >> n;
    if (n == 1) {cout << "1" << endl; return;}
    tree.assign(n + 1, vector<int>());
    lev.assign(n + 1, 0);
    parent.assign(n + 1, 0);
    
    for (int i = 1; i < n; i++) {
        int u, v;
        cin >> u >> v;
        tree[u].push_back(v);
        tree[v].push_back(u);
    }

    // First DFS to find one endpoint of the diameter
    dfs(1, 0, 0);
    int ch = 1;
    for (int i = 1; i <= n; i++) {
        if (lev[i] > lev[ch]) ch = i;
    }

    // Second DFS to find the diameter
    dfs(ch, 0, 0);
    int diameter = 0;
    for (int i = 1; i <= n; i++) {
        if (lev[i] > lev[ch]) ch = i;
    }
    diameter = lev[ch];

    int dis = diameter / 2;
    int c1 = -1, c2 = -1;

    // Move to the center of the tree
    while (dis--) {
        ch = parent[ch];
    }
    c1 = ch;

    if (diameter % 2 == 1) {
        c2 = parent[c1];
    }
    
    int ans = 0;
    if (c2 != -1) {
        // Two centers case
        dfs1(c1, c2, 0,diameter / 2);
        cnt1 = cnt;
        cnt = 0;
        dfs1(c2, c1,0,diameter / 2);
        cnt2 = cnt;
        ans += (cnt1 * cnt2);
    } else {
        // Single center case
        int sum = 0;
        for (auto ele : tree[c1]) { //nC2 wont work
            dfs1(ele, c1, 0,diameter / 2 - 1); // 1 unit reduce to avoid center
//counting the possible no.of nodes for each children of center node
            coun.push_back(cnt);
            sum += cnt;
            cnt = 0;
        }
        for (auto e : coun) { //choosing between nodes of one child branch and others
            ans += (e * (sum - e));
        }
        ans /= 2;
    }
    cout << ans << endl;
}

int32_t main() {
    ios_base::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    solve();
    return 0;
}
```

>[!Note]
>Here nC2 won't work for single center because there can be a shorter path between the leaf nodes of a diameter of the same child of the center node.

