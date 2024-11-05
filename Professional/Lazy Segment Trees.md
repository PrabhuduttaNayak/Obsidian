03/11/2024 21:47

Tags: #ratt_lo 

Status: [[00_DSA]]    [[Segment Trees]] [[Trees]] 

# Lazy Segment Trees


A lazy segment tree is used in scenarios where we need to perform frequent **range updates** and **range queries** efficiently. Here’s a breakdown of the topics and types of problems where a lazy segment tree is particularly useful:

### 1. **Range Updates and Queries**

- If we need to frequently update a **range of elements** in an array (e.g., add a value to all elements in a range) and later query some **aggregate information** (sum, minimum, maximum) over another range, a lazy segment tree is ideal.
- Without lazy propagation, a segment tree would need to propagate the updates across all affected nodes immediately, which could become slow if updates cover large ranges.
- Lazy propagation allows us to delay (or “lazily” apply) updates until they’re actually needed, making both range updates and range queries efficient.

### 2. **Range Addition and Range Sum Queries**

- **Problem Example**: For an array of elements, support:
    - **Range addition**: Add a value `v` to all elements in a subarray `[l, r]`.
    - **Range sum query**: Find the sum of elements in a subarray `[l, r]`.
- Using a lazy segment tree, we can implement range updates and queries in O(log⁡N)O(\log N)O(logN) time.

### 3. **Range Update with Maximum/Minimum Queries**

- **Problem Example**: For an array, support:
    - **Range assignment**: Set all elements in a range `[l, r]` to a specific value `v`.
    - **Range max/min query**: Query the maximum or minimum value in a range `[l, r]`.
- Lazy propagation helps apply these updates efficiently without immediately updating every element in the range.


##### Problem is to update all numbers in the array from range L,R to the value V. ANd find the maximum of all elements in that range

```cpp
	struct node{
		int sum,maxr,lazy;
		node(){
		sum=0;
		lazy=0;
		maxr=0;
		}
	};
	node tree[4*size]; //change size
	
	node merge(node a, node b) {
	    node temp; //no need to update lazy here
	    temp.sum = a.sum + b.sum;
	    temp.maxr = max(a.maxr, b.maxr);
	    return temp;
	}
	
	void push(int id, int l, int r) {
    if (t[id].lazy) { //if lazy is 0 means its not set
    //update
        t[id].sum = t[id].lazy * (r - l + 1); //since all values are v from L-R
        t[id].maxr = t[id].lazy;
        
        if (l != r) { //means children exist, then push lazy to the children
            t[id*2].lazy = t[id].lazy;
            t[id*2 + 1].lazy = t[id].lazy;
        }
        t[id].lazy = 0; //unset the lazy index,bcz now its computed
    }
}
```

The `update` function:

- First, calls `push` to apply any pending updates to the current node.
- Checks for no overlap, in which case it returns immediately.
- If there is a complete overlap (`lq <= l && r <= rq`), it sets the lazy value and calls `push` to apply the update immediately.
- If there is partial overlap, it recursively updates the left and right children and then merges them to update the current node.

```cpp
void update(int id, int l, int r, int lq, int rq, int v) {
    push(id, l, r);
    if (lq > r || l > rq) return;
    if (lq <= l && r <= rq) { // in normal segment tree we used to go till the leaf node as left==right then applied tree[index]=arr[left];
        t[id].lazy = v;
        push(id, l, r);
        return;
    }
    int mid = (l + r) >> 1;
    update(id << 1, l, mid, lq, rq, v);
    update(id << 1 | 1, mid + 1, r, lq, rq, v);
    t[id] = merge(t[id << 1], t[id << 1 | 1]);
}

node query(int id, int l, int r, int lq, int rq) {
    push(id, l, r);
    if (lq > r || rq < l) return node(); // Assuming a default node with neutral values
    if (lq <= l && r <= rq) return t[id];
    int mid = (l + r) >> 1;
    return merge(query(id << 1, l, mid, lq, rq), query(id << 1 | 1, mid + 1, r, lq, rq));
}
```
