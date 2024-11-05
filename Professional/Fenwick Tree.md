04/11/2024 23:53

Tags: #fenwick_tree #ratt_lo 

Status: [[Segment Trees]] [[00_DSA]]

# Fenwick Tree

A company has n employees with certain salaries. Your task is to keep track of the salaries and process queries. 
Input The first input line contains two integers n and q: the number of employees and queries. 
The employees are numbered 1,2,...n. 
The next line has n integers p_1,p_2,....p_n: each employee's salary. 
After this, there are q lines describing the queries. Each line has one of the following forms: 
! k x: change the salary of employee k to x 
? a b: count the number of employees whose salary is between a...b




To handle this problem efficiently, we need a way to:

1. Update an employee's salary quickly.
2. Count the number of employees with salaries within a given range [a,b][a, b][a,b] quickly.

A **Segment Tree with Coordinate Compression** or a **Fenwick Tree with Binary Search** approach will work well here, especially if we expect multiple updates and range queries.

Here’s the solution approach:

1. **Data Structures**:
    
    - Use a Fenwick Tree (or Binary Indexed Tree) or Segment Tree to handle salary updates and range queries.
    - Coordinate Compression for salaries, as this reduces our salary range to manageable indices in the tree.
2. **Algorithm**:
    
    - For each employee, we initially map their salary to a compressed index.
    - Use a Fenwick Tree to maintain the frequency of each salary.
    - For an update, adjust the frequency of the old salary and add the new salary.
    - For each query, use the Fenwick Tree to get the number of employees with salaries within the range [a,b][a, b][a,b].

### C++ Solution Using Fenwick Tree with Coordinate Compression

Here’s the code implementing this solution:

```cpp
#include <bits/stdc++.h>
using namespace std;

class FenwickTree {
    vector<int> tree;
public:
    FenwickTree(int n) : tree(n + 1, 0) {}
    
    // Add value to index i
    void add(int i, int value) {
        while (i < tree.size()) {
            tree[i] += value;
            i += i & -i;
        }
    }
    
    // Get sum of elements up to index i
    int sum(int i) {
        int s = 0;
        while (i > 0) {
            s += tree[i];
            i -= i & -i;
        }
        return s;
    }
    
    // Get sum in range [a, b]
    int rangeSum(int a, int b) {
        return sum(b) - sum(a - 1);
    }
};

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);

    int n, q;
    cin >> n >> q;
    
    vector<int> salaries(n);
    for (int i = 0; i < n; i++) {
        cin >> salaries[i];
    }
    
    // Unique and sorted salaries for coordinate compression
    set<int> uniqueSalaries(salaries.begin(), salaries.end());
    vector<int> compressedSalaries(uniqueSalaries.begin(), uniqueSalaries.end());
    unordered_map<int, int> salaryToIndex;
    for (int i = 0; i < compressedSalaries.size(); i++) {
        salaryToIndex[compressedSalaries[i]] = i + 1;  // Using 1-based index for Fenwick Tree
    }

    // Fenwick Tree to store the frequency of each salary index
    FenwickTree fenwick(compressedSalaries.size());
    
    // Initialize the Fenwick Tree with initial salaries
    vector<int> currentIndex(n);
    for (int i = 0; i < n; i++) {
        int idx = salaryToIndex[salaries[i]];
        fenwick.add(idx, 1);
        currentIndex[i] = idx;
    }
    
    while (q--) {
        char type;
        cin >> type;
        if (type == '!') {
            int k, x;
            cin >> k >> x;
            --k;  // 0-based index for employee k
            
            int oldIdx = currentIndex[k];
            int newIdx = salaryToIndex[x];
            
            if (oldIdx != newIdx) {
                fenwick.add(oldIdx, -1);
                fenwick.add(newIdx, 1);
                currentIndex[k] = newIdx;
            }
        } else if (type == '?') {
            int a, b;
            cin >> a >> b;

            int leftIdx = lower_bound(compressedSalaries.begin(), compressedSalaries.end(), a) - compressedSalaries.begin() + 1;
            int rightIdx = upper_bound(compressedSalaries.begin(), compressedSalaries.end(), b) - compressedSalaries.begin();

            if (leftIdx > rightIdx) {
                cout << 0 << '\n';
            } else {
                cout << fenwick.rangeSum(leftIdx, rightIdx) << '\n';
            }
        }
    }

    return 0;
}

```

### Explanation of Key Parts

1. **Coordinate Compression**:
    
    - We create a `compressedSalaries` array to hold unique, sorted salary values. Each unique salary gets mapped to a unique index (1-based for compatibility with Fenwick Tree).
    - `salaryToIndex` maps each actual salary to its compressed index.
2. **Fenwick Tree Operations**:
    
    - `add(i, value)`: Updates the frequency of a salary at index `i`.
    - `rangeSum(a, b)`: Calculates the number of employees with salaries in the compressed index range [a,b][a, b][a,b], which corresponds to the actual salary range [a,b][a, b][a,b] after compression.
3. **Processing Queries**:
    
    - For an update `! k x`, we adjust the old salary and the new salary in the Fenwick Tree.
    - For a query `? a b`, we translate the salary range [a,b][a, b][a,b] to compressed indices and use the Fenwick Tree to get the count of employees in that range.

### Complexity

- **Time Complexity**:
    - **Build**: O(nlog⁡n)O(n \log n)O(nlogn) for inserting salaries into the Fenwick Tree.
    - **Query**: O(log⁡n)O(\log n)O(logn) per query due to the Fenwick Tree operations.