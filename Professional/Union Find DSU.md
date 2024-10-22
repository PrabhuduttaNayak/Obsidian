18/10/2024 13:44

Tags: #dsu

Status: [[00_DSA]] [[MST]]

# Union Find


  Used while calculating number of components of a graph, generally we cant use dfs/bfs for every iteration , Hence we use this method to calculate the components size faster.
  
### Explanation:

1. **UnionFind Constructor:** Initializes the `parent` and `rank` arrays for each node. Each node is its own parent initially, and the rank is set to 1.
    
2. **Find Function with Path Compression:** The `find` function compresses the path as it traverses up the tree. This ensures that future calls to `find` are more efficient. AMORTIZATION
	As the first loop takes N time and the rest take logN time. We do that because we are mostly interested in the root of the tree instead of every parent
1. **Merge Function:** Combines two sets, ensuring the smaller tree gets merged under the larger one, based on the rank, which helps maintain tree balance and optimize find operations.
    
4. **Reset Function:** Resets the DSU structure back to its initial state.
    
5. **Print Function:** Displays the parent of each node.
    
6. **Size Function:** Returns the current number of disjoint sets. #ratt_lo 
  
```cpp
#include <iostream>
#include <vector>
using namespace std;

struct UnionFind {
    int n, set_size, *parent, *rank;

    // Constructor
    UnionFind(int a) {
        n = set_size = a;
        parent = new int[n+1];
        rank = new int[n+1];
        for(int i = 1; i <= n; ++i) {
            parent[i] = i;
            rank[i] = 1;
        }
    }

    // Find function with path compression
    int find(int x) {
        if (x != parent[x])
            return parent[x] = find(parent[x]); // Path compression/ Amortization
        return x;
    }

    // Merge two sets
    void merge(int x, int y) {
        int xroot = find(x), yroot = find(y);
        if (xroot != yroot) {
            if (rank[xroot] >= rank[yroot]) {
                parent[yroot] = xroot;
                rank[xroot] += rank[yroot];
            } else {
                parent[xroot] = yroot;
                rank[yroot] += rank[xroot];
            }
            set_size -= 1; // Decrease the number of disjoint sets
        }
    }

    // Reset the DSU to its initial state
    void reset() {
        set_size = n;
        for(int i = 1; i <= n; ++i) {
            parent[i] = i;
            rank[i] = 1;
        }
    }

    // Return the number of disjoint sets
    int size() {
        return set_size;
    }

    // Print the parent array
    void print() {
        for(int i = 1; i <= n; ++i) 
            cout << i << " -> " << parent[i] << "\n";
    }
};

int main() {
    int n = 5;
    UnionFind dsu(n);
    
    dsu.merge(1, 2);
    dsu.merge(3, 4);
    dsu.merge(2, 3);

    cout << "Parent of each node:\n";
    dsu.print();
    cout << "Number of disjoint sets: " << dsu.size() << endl;

    dsu.reset();
    cout << "\nAfter reset:\n";
    dsu.print();
    cout << "Number of disjoint sets after reset: " << dsu.size() << endl;

    return 0;
}

```

>[!Note]
>- **Constructor Definition**: A constructor is a special member function used to initialize objects of a class or struct. It is automatically invoked when an object of the class/struct is created. In C++, constructors do not return any value (not even `void`), which is why you don't declare a return type for them.
>- **Implicit Behavior**: The purpose of a constructor is to set up the initial state of the object. The language design specifies that it cannot return a value, as its job is only to initialize members and set up the object in a valid state.

---
##### Description

You have given an undirected graph _G_ with _N_ nodes, indexed from 1 to _N_ and _M_ edges, indexed from 1 to _M_.

There are two types of operations:  
**1 X**: Remove the edge numbered _X_.  
**2**: Print the number of connected components in the graph

# Solution Approach

The trick in the problem is to reverse the queries. 
First, we only add those edges in the graph, which have never occurred in any removal query.  
Now after reversing the queries, the removal query is now an addition query. Add the edge in the graph for the first type of query.  
For _2nd_ type of query, find the number of connected components and stored it somewhere at the appropriate index, which is according to the original queries.  
And print the answer in proper order. The time and space complexities are the same as _DSU_.

**Time complexity**: Without path compression: _O(NlogN) &_ With path compression: _O(N)_  
**Space complexity**: _O(N + M + Q)_ extra space