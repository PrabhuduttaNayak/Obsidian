27/10/2024 12:40

Tags: [[00_DSA]] [[Trees]] [[Segment Trees]] 

Status: Solve Range Queries problems from CSES 
- [Range Updates and Sums](https://cses.fi/problemset/task/1735)
- [Polynomial Queries](https://cses.fi/problemset/task/1736)1
- [Salary Queries](https://cses.fi/problemset/task/1144)
- [Range Xor Queries](https://cses.fi/problemset/task/1650)
- [Range Update Queries](https://cses.fi/problemset/task/1651)
# Segment Trees


### 1. What is a Segment Tree?

A **Segment Tree** is a binary tree used for storing intervals or segments. It allows querying and updating over a range of elements efficiently (in `O(log n)` time). Segment trees are particularly useful for range-based operations, such as sum, minimum, maximum, or any associative function on an array’s segments.

### 2. Segment Tree Structure

The segment tree in this code is represented as an array `tree[]`. Each node of this tree represents a segment of the original array, storing either a value or an aggregated value (like a sum) over that segment.

- **`tree[index]`**: Stores the sum of the segment of `arr[]` that this node represents.
- **`arr[]`**: The original array.

### 3. Building the Segment Tree

The `build` function constructs the segment tree based on the values of `arr[]`. Here’s how it works:

```cpp

int arr[200400];
int tree[800400]; //tree is taken 4 times the size bcz of logn new nodes
void build(int index, int left, int right) {
    if (left == right) {
        tree[index] = arr[left];
        return;
    }
    int mid = (left + right) / 2;
    build(index * 2, left, mid); // Left child
    build(index * 2 + 1, mid + 1, right); // Right child
    tree[index] = tree[2 * index] + tree[2 * index + 1];
}
```

- The `build` function recursively divides the array into two halves (left and right).
- When `left == right`, we’ve reached a leaf node, which corresponds to an element in `arr[]`.
- After building the left and right children, each internal node (`tree[index]`) stores the sum of its two child nodes (`tree[2 * index] + tree[2 * index + 1]`).

**Time Complexity**: `O(n)`, where `n` is the size of `arr[]`.

### 4. Querying the Segment Tree

The `query` function performs a range sum query over `arr[]`. It checks if the current segment overlaps with the query range `[ll, rr]` and returns the sum within this range.

```cpp
int query(int index, int left, int right, int ll, int rr) {
    if (ll <= left && rr >= right) return tree[index];//Complete overlap important*
    if (ll > right || rr < left) return 0; // No overlap
    int mid = (left + right) / 2;
    return query(index * 2, left, mid, ll, rr) + query(index * 2 + 1, mid + 1, right, ll, rr);
}
```

- **Complete Overlap**: If the segment `[left, right]` of `tree[index]` is completely within `[ll, rr]`, we return `tree[index]`.
- **No Overlap**: If `[left, right]` is entirely outside `[ll, rr]`, we return 0.
- **Partial Overlap**: If there’s a partial overlap, we recursively query the left and right children and sum their results.

**Time Complexity**: `O(log n)`, as the tree height is `log n`.

### 5. Updating the Segment Tree

The `update` function changes an element in `arr[]` and updates the segment tree to reflect this change.

```cpp
void update(int index, int left, int right, int pos, int value) {
    if (pos < left || pos > right) return; // No overlap
    
    if (left == right) { // Leaf node
        //if(left==pos){  // we can't directly left==right==pos// this line is not needed i guess
            tree[index] = value;
            return;
        //}
    }

    int mid = (left + right) / 2;
    update(index * 2, left, mid, pos, value);
    update(index * 2 + 1, mid + 1, right, pos, value);
    tree[index] = tree[2 * index] + tree[2 * index + 1];
}
```

- The function checks if `pos` (the position to update) lies within `[left, right]`. If not, it returns as no update is needed.
- If `left == right` and `left == pos`, we’ve reached the leaf node, so we update `tree[index]` with `value`.
- If there’s a partial overlap, we recursively update both children and then recalculate `tree[index]` to maintain consistency in the tree.

**Time Complexity**: `O(log n)`.
```cpp
signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
  
    int n, q;
    cin >> n >> q;
    for (int i = 1; i <= n; i++) {
        cin >> arr[i];
    }
    build(1, 1, n); // Build the segment tree
  
    while (q--) {
        int k;
        cin >> k;
        if (k == 1) { // Update operation
            int index, val;
            cin >> index >> val;
            update(1, 1, n, index, val);
        } else { // Query operation
            int left, right;
            cin >> left >> right;
            cout << query(1, 1, n, left, right) << endl;
        }
    }
    return 0;
}
```
### Summary

- **Build**: Constructs the tree in `O(n)` time by aggregating sums at each level.
- **Query**: Fetches the sum over a range in `O(log n)` by dividing the problem and combining results.
- **Update**: Changes an element and recalculates affected nodes in `O(log n)`.

This code is efficient for scenarios requiring multiple range queries and updates on an array, as it significantly reduces the time complexity compared to naive approaches.


---
#### First Min Queries

##### Description

You have given an array _A_ of _n_ integers. Your task is to answer _q_ queries. Each query contains an integer _x_.   
For each query, print the minimum index _i_ such that _Ai_ ≥ _x,_ and then decrease the value at position _i_ by _x,_ i.e., _Ai = Ai - x._ If there doesn't exist any index _i_ satisfying the condition, then simply print 0.

##### Input Format

The first input line has two integers _n_ and _q_: the number of values and queries.  
The second line has _n_ integers _A1, A2, …, An_: the array values.  
Finally, there are _q_ integers describing the queries. _x1, x2, …, xq_.

##### Output Format

Print the index _i_ as mentioned in the problem statement. If such an index doesn't exist, then print 0

# Solution Approach

![coin](https://maang.in/img/coding/up.svg)

The segment tree is built initially to store the values of the array. Recursively build the left and right subtrees of each node by dividing the range in half. Update each node with the maximum value of its children. For each query, a binary search is performed on the segment tree to find the minimum index where the value is greater than or equal to the given value. If such an index is found, the value at that index is decreased by the given value.

Time Complexity:

Building the segment tree takes O(n) time. Each query performs a binary search on the segment tree, which takes O(log⁡n) time. Overall, the time complexity of the solution is O(n+qlog⁡n)), where n is the size of the array and qq is the number of queries.


```cpp
#include <bits/stdc++.h>

using namespace std;

#define int long long
#define double long double

int arr[200400];
int tree[800800];

void build(int index, int left, int right) {
    if (left == right) {
        tree[index] = arr[left];
        return;
    }
    int mid = (left + right) / 2;
    build(index * 2, left, mid);
    build(index * 2 + 1, mid + 1, right);
    tree[index] = max(tree[index * 2], tree[index * 2 + 1]);
}

int query(int index, int left, int right, int num) {
    if (tree[index] < num) return -1; // If the maximum in this range is less than num, return -1
    if (left == right) return left; // Return the index of the leaf node that meets the condition

    int mid = (left + right) / 2;
    if (tree[index * 2] >= num) return query(index * 2, left, mid, num); // Check the left child
    else return query(index * 2 + 1, mid + 1, right, num); // Otherwise, check the right child
}

void update(int index, int left, int right, int pos, int num) {
    if (pos < left || pos > right) return; // If pos is out of bounds

    if (left == right) { // Leaf node
        tree[index] -= num;
        arr[pos] = tree[index];
        return;
    }

    int mid = (left + right) / 2;
    if (pos <= mid) update(index * 2, left, mid, pos, num);
    else update(index * 2 + 1, mid + 1, right, pos, num);

    tree[index] = max(tree[index * 2], tree[index * 2 + 1]); // Recalculate the maximum after the update
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);

    int n, q;
    cin >> n >> q;

    for (int i = 0; i < n; i++) {
        cin >> arr[i];
    }
    build(1, 0, n - 1);

    for (int i = 0; i < q; i++) {
        int x;
        cin >> x;
        int ans = query(1, 0, n - 1, x);
        if (ans >= 0)
            update(1, 0, n - 1, ans, x);
        cout << (ans + 1) << " ";
    }
    cout << "\n";

    return 0;
}
```

---


#### Even and Odd Queries
##### Description

You've given an array _A_ of _N_ integers. Your task is to support the following queries.

- 0 _x_ _y_ : Modify the number at index _x_ to _y_.
- 1 _l r_ : Print the count the number of even numbers in range _l_ to _r_ inclusive.
- 2 _l r_ : Print the count the number of odd numbers in range _l_ to _r_ inclusive.

##### Input Format

The first input line has an integer _N,_ the number of elements.  
The second line has _N_ integers _A1, A2, …, AN_: the array values.  
The third line has an integer _Q,_ the number of queries.  
Each of the next _Q_ lines contains the query of one of the three types described in the statement. 

##### Output Format

Print the output of query of type 1 and 2 on a new line.

```cpp
#include <bits/stdc++.h>

using namespace std;
/*You've given an array A of N integers. Your task is to support the following queries.

0 x y : Modify the number at index x to y.
1 l r : Print the count the number of even numbers in range l to r inclusive.
2 l r : Print the count the number of odd numbers in range l to r inclusive. */
#define int long long
#define double long double

int n;
int arr[200400][2]; // 1st odd, 2nd even
int tree[800800][2];

void build(int index, int left, int right) {
    if (left == right) {
        tree[index][0] = arr[left][0];
        tree[index][1] = arr[left][1];
        return;  // Base case return to prevent further recursion
    }
    int mid = (left + right) / 2;
    build(index * 2, left, mid);
    build(index * 2 + 1, mid + 1, right);
    tree[index][0] = tree[index * 2][0] + tree[index * 2 + 1][0];
    tree[index][1] = tree[index * 2][1] + tree[index * 2 + 1][1];
}

void modify(int index, int left, int right, int pos, int y) {
    if (pos < left || pos > right) return;
    if (left == right) {  // Leaf node
        if (y % 2 == 0) {
            tree[index][0] = 0;
            tree[index][1] = 1;
        } else {
            tree[index][0] = 1;
            tree[index][1] = 0;
        }
        return; // Base case return to avoid further recursion
    }
    int mid = (left + right) / 2;
    modify(index * 2, left, mid, pos, y);
    modify(index * 2 + 1, mid + 1, right, pos, y);
    tree[index][0] = tree[index * 2][0] + tree[index * 2 + 1][0];
    tree[index][1] = tree[index * 2][1] + tree[index * 2 + 1][1];
}

int printEven(int index, int left, int right, int ll, int rr) {
    if (ll <= left && rr >= right) {  // Complete overlap
        return tree[index][1];
    }
    if (ll > right || rr < left) return 0;  // No overlap
    int mid = (left + right) / 2;
    return printEven(index * 2, left, mid, ll, rr) + printEven(index * 2 + 1, mid + 1, right, ll, rr);
}

int printOdd(int index, int left, int right, int ll, int rr) {
    if (ll <= left && rr >= right) {  // Complete overlap
        return tree[index][0];
    }
    if (ll > right || rr < left) return 0;  // No overlap
    int mid = (left + right) / 2;
    return printOdd(index * 2, left, mid, ll, rr) + printOdd(index * 2 + 1, mid + 1, right, ll, rr);
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);

    cin >> n;
    for (int i = 1; i <= n; i++) {
        int k;
        cin >> k;
        if (k % 2 == 0) {
            arr[i][1] = 1;
            arr[i][0] = 0;
        } else {
            arr[i][1] = 0;
            arr[i][0] = 1;
        }
    }
    build(1, 1, n);

    int q;
    cin >> q;
    for (int i = 0; i < q; i++) {
        int k;
        cin >> k;
        if (k == 0) {  // Modify operation
            int x, y;
            cin >> x >> y;
            modify(1, 1, n, x, y);
        } else if (k == 1) {  // Query even count
            int l, r;
            cin >> l >> r;
            cout << printEven(1, 1, n, l, r) << endl;
        } else if (k == 2) {  // Query odd count
            int l, r;
            cin >> l >> r;
            cout << printOdd(1, 1, n, l, r) << endl;
        }
    }

    return 0;
}
```

---

#### Subarray Sum Queries #unsolved  #ratt_lo #important 
##### Description 

There is an array consisting of _n_ integers. Some values of the array will be updated, and after each update, your task is to report the maximum subarray sum in the array.

##### Input Format

The first input line contains integers _n_ and _m_: the size of the array and the number of updates. The array is indexed 1, 2,…, _n_.  
The next line has _n_ integers: _x_1, _x_2, …, _xn_: the initial contents of the array.  
Then there are _m_ lines describing the changes. Each line has two integers _k_ and _x_: the value at position _k_ becomes _x_.

##### Output Format

After each update, print the maximum subarray sum. Empty subarrays (with sum 0) are allowed.

##### Constraints

1 ≤ _n_, _m_ ≤ 2 x 105  
−109 ≤ _xi_ ≤ 109  
1 ≤ _k_ ≤ _n_  
−109 ≤ _x_ ≤ 109

##### Sample Input 1

5 3 1 2 -3 5 -1 2 6 3 1 2 -2

##### Sample Output 1

9 13 6

# Solution Approach

![coin](https://maang.in/img/coding/up.svg)

The main idea is to divide the array into smaller segments recursively and calculate the maximum subarray sum for each segment. The merge operation combines the results from child segments to calculate the maximum subarray sum for the parent segment. While merging we need to consider the maximum of three subarrays:

1. maximum possible sum of left subarray
2. maximum possible sum of right subarray
3. maximum possible subarray by merging some suffix subarray sum of the left subarray and some prefix subarray sum of the right subarray.

The update operation modifies the array and updates the corresponding nodes in the segment tree. The query operation retrieves the maximum subarray sum from the root of the segment tree.

```cpp
#include <bits/stdc++.h>
using namespace std;

#define ll long long int
#define LD long double

const int N = 200010;

int inf = 1e9;
int mod = 1e9 + 7;

struct ST
{
    ll maxi, lsum, rsum, sum;
    ST(ll sum = 0, ll maxi = 0, ll lsum = 0, ll rsum = 0) : sum(sum), maxi(maxi), lsum(lsum), rsum(rsum) {}
};

ST operator+(const ST &A, const ST &B) //important
{
    ST temp;
    temp.lsum = max(A.lsum, A.sum + B.lsum);
    temp.rsum = max(B.rsum, B.sum + A.rsum);
    temp.sum = A.sum + B.sum;
    temp.maxi = max(max(A.maxi, B.maxi), A.rsum + B.lsum);
    return temp;
}

struct segtree
{
    vector<ST> tree;
    segtree(int n = N)
    {
        tree.resize(4 * n);
    }

    void update(int node, int start, int end, int idx, ll val)
    {
        if (start == end)
        {
            ll x = max(val, 0LL);
            tree[node] = ST(val, x, x, x);
        }
        else
        {
            int mid = (start + end) >> 1;
            if (idx <= mid)
                update(2 * node, start, mid, idx, val);
            else
                update(2 * node + 1, mid + 1, end, idx, val);
            tree[node] = tree[2 * node] + tree[2 * node + 1];
        }
    }

    ll query()
    {
        return tree[1].maxi;
    }
};

signed main()
{
    // freopen("IN", "r", stdin);
    // freopen("OUT", "w", stdout);

    ios_base::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);

    int n, q;
    cin >> n >> q;

    segtree T(n);

    for (int i = 0; i < n; i++)
    {
        ll val;
        cin >> val;
        T.update(1, 0, n - 1, i, val);
    }

    for (int i = 0; i < q; i++)
    {
        int k;
        ll x;
        cin >> k >> x;
        k--;
        T.update(1, 0, n - 1, k, x);
        cout << T.query() << "\n";
    }
    return 0;
}
```


---
#### Range Queries II #unsolved #Question_Type 
##### Description

You have given an array _A_ of _n_ elements. Your task is to process _q_ queries of the following types.

- 1 _i j_ _x_ : Increase each value at positions from [_i, j_] by _x_.
- 2 _i_ : Print the value at position _i_.

##### Input Format

The first input line has two integers _n_ and _q_: the number of values and queries.  
The second line has _n_ integers _A1, A2, …, An_: the array values.  
Finally, there are _q_ lines describing the queries. Each line has three integers: either "1 _i j x_" or "2 _i_".

##### Output Format

Print the result of each query of type 2.

##### Constraints

1 ≤ _n_, _q_ ≤ 2 x 105  
1 ≤ _Ai, x_ ≤ 109  
1 ≤ _i_ ≤ _n_  
1 ≤ _i_ ≤ _j_ ≤ _n_

##### Sample Input 1

8 3 3 2 4 5 1 1 5 3 2 4 1 2 5 1 2 4

##### Sample Output 1

5 6

>[!Note]
>- We are just building here the prefix sum array not added with the original array. We are directly adding the arr index at the end to our result.
>- Used update function twice to once add the left element and once delete the right+1 element
>- Query is returning the sum of all extra elements above the array

```cpp
#include <bits/stdc++.h>

using namespace std;
#define int long long
const int MAX_SIZE = 1e7;
long long arr[MAX_SIZE], tree[MAX_SIZE * 4];


void update(int index, int left, int right, int pos, int value) {
  if (pos > right or pos < left) {
    return;
  }
  if (left == right) {
    tree[index] += value;
    return;
  }
  int mid = left + (right - left) / 2;

  update(index * 2, left, mid, pos, value);
  update(index * 2 + 1, mid + 1, right, pos, value);
  tree[index] = (tree[index * 2] + tree[index * 2 + 1]);
}

int query(int index, int left, int right, int lq, int rq) {
  if (lq > right or rq < left) {
    return 0;
  }
  if (left >= lq and right <= rq) {
    return tree[index];
  }
  int mid = left + (right - left) / 2;

  return (query(index * 2, left, mid, lq, rq) +
          query(index * 2 + 1, mid + 1, right, lq, rq));
}

void solve() {
  int n, q;
  cin >> n >> q;

  for (int i = 1; i <= n; i++) {
    cin >> arr[i];
  }

  while (q--) {
    int choice;
    cin >> choice;

    if (choice == 1) {
      int a, b, x;
      cin >> a >> b >> x;
      update(1, 1, n, a, x);
      update(1, 1, n, b + 1, -x);
    } else {
      int a;
      cin >> a;
      cout << query(1, 1, n, 1, a) + arr[a]<< endl;
    }
  }
}

signed main() {
  ios::sync_with_stdio(false);
  cin.tie(nullptr);
  cout.tie(nullptr);
  solve();
}
```

---

# #Bracket_Queries #ratt_lo #unsolved #important 
##### Description

You have given a bracket sequence _s1, s2, …, sn_ or in other words, a string _s_ of length _n_, consisting of characters '(' and ')'.   
You've been asked _Q_ queries. The _i_th query is of the form _li_ and _ri_. The answer to the _i_-th query is the length of the maximum correct bracket **subsequence** of sequence _sli_, _sli_ + 1, ..., _sri_.  
Find the correct answer to all queries.

##### Input Format

The first line contains a sequence of characters _s_1, _s_2, ..., _sn_ without any spaces. Each character is either a "(" or a ")".   
The second line contains integer _Q_ — the number of queries.   
Each of the next _Q_ lines contains a pair of integers. The _i_-th line contains integers _li_, _ri_ — the description of the _i_-th query.

##### Output Format

For each query, print the answer on a new line.

```cpp
#include <bits/stdc++.h>

using namespace std;
/*
Description
You are given a bracket sequence s1, s2, …, sn or in other words, a string s of length n, consisting of characters '(' and ')'. 
You've been asked Q queries. The ith query is of the form li and ri. The answer to the i-th query is the length of the maximum 
correct bracket subsequence of sequence sli, sli + 1, ..., sri.
Find the correct answer to all queries.
*/

#define int long long
#define double long double
    
int n;

struct node {
    int leftC;   // count of unmatched '('
    int rightC;  // count of unmatched ')'
    int sum;     // length of valid bracket subsequence

    node(int leftC = 0, int rightC = 0, int sum = 0) : leftC(leftC), rightC(rightC), sum(sum) {}
};

node tree[8008000];

node merge(node A, node B) { //important
    node temp;
    int matched = min(A.leftC, B.rightC); // match the '(' from A with ')' from B
    temp.sum = A.sum + B.sum + matched * 2; // add matched pairs to the total valid length
    temp.leftC = A.leftC + B.leftC - matched; // remaining '(' after matching
    temp.rightC = A.rightC + B.rightC - matched; // remaining ')' after matching
    return temp;
}

void build(int index, int left, int right, const string &s) {
    if (left == right) {
        // Leaf node
        if (s[left - 1] == '(') {
            tree[index] = node(1, 0, 0); // 1 unmatched '('
        } else {
            tree[index] = node(0, 1, 0); // 1 unmatched ')'
        }
        return;
    }

    int mid = (left + right) / 2;
    build(index * 2, left, mid, s);
    build(index * 2 + 1, mid + 1, right, s);
    tree[index] = merge(tree[index * 2], tree[index * 2 + 1]);
}

node query(int index, int left, int right, int ll, int rr) {
    if (ll > right || rr < left) {
        // No overlap
        return node(0, 0, 0);
    }
    if (left >= ll && right <= rr) {
        // Complete overlap
        return tree[index];
    }
    int mid = (left + right) / 2;
    node leftResult = query(index * 2, left, mid, ll, rr);
    node rightResult = query(index * 2 + 1, mid + 1, right, ll, rr);
    return merge(leftResult, rightResult); ///// D
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);

    string s;
    cin >> s;

    n = s.size();
    build(1, 1, n, s);

    int q;
    cin >> q;
    for (int i = 0; i < q; i++) {
        int l, r;
        cin >> l >> r;
        cout << query(1, 1, n, l, r).sum << endl; // print the sum for the valid subsequence length
    }

    return 0;
}
```
### Merge Logic Explanation

Let's assume we have two nodes, `A` (left segment) and `B` (right segment), that we want to merge into a single node `temp`:


```cpp
	node merge(node A, node B) {     
	node temp;          // Step 1: Match open brackets from A with close brackets from B     
	int matched = min(A.leftC, B.rightC);
```

- `matched` calculates the number of pairs of `(` from the left segment (`A.leftC`) that can be matched with `)` from the right segment (`B.rightC`).
- `min(A.leftC, B.rightC)` ensures we don’t over-match—only as many pairs as are available in both nodes.


```cpp
// Step 2: Calculate the valid bracket subsequence length     
temp.sum = A.sum + B.sum + matched * 2;
```

- `temp.sum` represents the length of the valid bracket subsequence in the combined range.
    - `A.sum` is the length of the valid bracket subsequence in the left segment.
    - `B.sum` is the length of the valid bracket subsequence in the right segment.
    - `matched * 2` is added because each matched pair `( )` contributes 2 characters to the valid sequence length.


```cpp
    // Step 3: Calculate the remaining unmatched brackets     
    temp.leftC = A.leftC + B.leftC - matched;     
    temp.rightC = A.rightC + B.rightC - matched;
```

- After matching, some brackets may remain unmatched:
    - `temp.leftC` is the total unmatched open brackets `(` in the combined range.
        - `A.leftC + B.leftC` is the sum of unmatched open brackets from both segments.
        - `- matched` removes the open brackets that were matched with close brackets from the other segment.
    - `temp.rightC` is the total unmatched close brackets `)` in the combined range.
        - `A.rightC + B.rightC` is the sum of unmatched close brackets from both segments.
        - `- matched` removes the close brackets that were matched with open brackets from the other segment.

Finally, `temp` is returned as the result of the merge.
`return temp; }`

```cpp
node merge(node A, node B) { //important
    node temp;
    int matched = min(A.leftC, B.rightC); // match the '(' from A with ')' from B
    temp.sum = A.sum + B.sum + matched * 2; // add matched pairs to the total valid length
    temp.leftC = A.leftC + B.leftC - matched; // remaining '(' after matching
    temp.rightC = A.rightC + B.rightC - matched; // remaining ')' after matching
    return temp;
}
```


---
## Given an array of n integers, your task is to process q queries of the form: what is the xor sum of values in range [a,b]?

```cpp
#include <bits/stdc++.h>
using namespace std;

const int MAXN = 1e5 + 5;
int arr[MAXN];        // Original array
int tree[4 * MAXN];   // Segment tree

// Build segment tree
void build(int index, int left, int right) {
    if (left == right) {
        // Leaf node: store the original array value
        tree[index] = arr[left];
        return;
    }
    int mid = (left + right) / 2;
    build(index * 2, left, mid);
    build(index * 2 + 1, mid + 1, right);
    tree[index] = tree[index * 2] ^ tree[index * 2 + 1];  // XOR of left and right child
}

// Range query for XOR
int rangeXor(int index, int left, int right, int ql, int qr) {
    // No overlap
    if (ql > right || qr < left) return 0;
    
    // Complete overlap
    if (ql <= left && right <= qr) return tree[index];
    
    // Partial overlap
    int mid = (left + right) / 2;
    int leftXor = rangeXor(index * 2, left, mid, ql, qr);
    int rightXor = rangeXor(index * 2 + 1, mid + 1, right, ql, qr);
    return leftXor ^ rightXor;
}

```

