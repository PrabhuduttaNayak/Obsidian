03/10/2024 18:49

Tags:  #binary_search

Status:[[00_DSA]]

# Binary Search O(logN)

### Explanation

Binary search only works on a pre-sorted list.

```javascript
[2, 3, 5, 8, 10, 15, 22, 23]
 ^            ^           ^
low          mid         high
```
* If item at mid point is correct, return it.
* If item at mid point is greater than the target, move `high` to below the mid:

```javascript
[2, 3, 5, 8, 10, 15, 22, 23]
 ^        ^
low      high
```

* If item at mid point is lesser than the target, move `low` to above the mid:

```javascript
[2, 3, 5, 8, 10, 15, 22, 23]
                  ^       ^
                 low     high
```

Keep going until guess is found. This can be done both recursively and iteratively.

### < or <=?
* If you are returning from inside the loop, use `low <= high`
* If you are reducing the search space, use `low < high` and finally return a `low`
---

* ### Lower Bound and Upper bound #lower_bound #upper_bound

- **`lower_bound`:** Finds the first index where the element is **greater than or equal** to the target.
- **`upper_bound`:** Finds the first index where the element is **greater than** the target.

For the vector `{1, 3, 3, 5, 7, 9}` and target `3`:

- `lower_bound` will return index `1` (first occurrence of 3).
- `upper_bound` will return index `3` (just after the last occurrence of 3).
---

### Recognize a BS problem
- Monotonic
- Sorted array
- Minimize maximun
- Maximize minimum
- N<=10^5

	why we do `lo+(hi-l0)/2` instead of `(lo+hi)/2` is intermediate value there dosen't exceed integer limit
	![[Pasted image 20241004002850.png]]
---

1. Iterative Binary Search:
```cpp
int binarySearch(int arr[], int n, int target) {
    int low = 0, high = n - 1;

    while (low <= high) {
        int mid = low + (high - low) / 2;

        // Check if the target is at the middle
        if (arr[mid] == target) //here we put the Check Function
            return mid;
        // If the target is smaller, ignore the right half
        else if (arr[mid] > target) 
            high = mid - 1;
        // If the target is larger, ignore the left half
        else 
            low = mid + 1;
    }
    return -1; // Target not found
}
```

2. Recursive Binary Search:
```cpp
int binarySearch(int arr[], int low, int high, int target) {
    if (low <= high) {
        int mid = low + (high - low) / 2;

        // If target is at the mid
        if (arr[mid] == target)
            return mid;

        // If target is smaller, recur for the left half
        if (arr[mid] > target)
            return binarySearch(arr, low, mid - 1, target);

        // If target is larger, recur for the right half
        return binarySearch(arr, mid + 1, high, target);
    }
    return -1; // Target not found
}
```

---
>[! Note]
>Check to find K rotated array : `arr[mid]<arr[0]` ( *here mid refers to every curr element of the array*)
>Check for Bi-tonic array: `arr[mid]>arr[mid+1]`

we need to build the check function and then do if `check(mid)==1` return `mid` #ratt_lo 



###  Type 1: Painter Partition Problem

You have `n` boards of lengths, and `k` painters. Each painter takes 1 unit time to paint 1 unit of board length. You want to partition the boards in such a way that the maximum amount of time taken by a single painter is minimized.


```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

// Function to calculate the total time for 'k' painters with the current mid as maximum length for any painter.
bool isValid(vector<int>& boards, int n, int k, int mid) {
    int totalPainters = 1; // At least one painter is required
    int currentSum = 0;

    for (int i = 0; i < n; i++) {
        currentSum += boards[i];

        if (currentSum > mid) {
            totalPainters++; // Assign new painter
            currentSum = boards[i]; // Reset currentSum to current board's length
        }

        // If more painters than allowed are needed, this configuration is invalid.
        if (totalPainters > k) {
            return false;
        }
    }

    return true; // If we fit in `k` or fewer painters, the configuration is valid.
}

// Function to find the minimum time needed by any painter to paint all boards
int painterPartition(vector<int>& boards, int n, int k) {
    int low = *max_element(boards.begin(), boards.end()); // Minimum time = max board length
    int high = accumulate(boards.begin(), boards.end(), 0); // Maximum time = sum of all board lengths

    int result = high;

    while (low <= high) {
        int mid = low + (high - low) / 2;

        if (isValid(boards, n, k, mid)) {
            result = mid; // Store the minimum found so far
            high = mid - 1; // Try for a smaller maximum time
        } else {
            low = mid + 1; // If this isn't valid, we increase the allowed maximum time
        }
    }

    return result;
}

int main() {
    vector<int> boards = {10, 20, 30, 40}; // Board lengths
    int k = 2; // Number of painters
    int n = boards.size();

    cout << "Minimum time to paint all boards: " << painterPartition(boards, n, k) << endl;
    return 0;
}

```

>[!NOTE] Things to keep in mind
>Maintain : Number of Painters, Current sum, Limits of low and high 


1. **`*max_element`**: #max_element_STL #STL 
    
    - This function is part of the `<algorithm>` library.
    - It returns an iterator to the maximum element in a given range.
    - Syntax: `*max_element(start_iterator, end_iterator)`
    - Example: `int max_val = *max_element(v.begin(), v.end());`
        - It finds the maximum value in the vector `v`.
2. **`accumulate`**: #array_sum_STL #STL #accumulate_sum
    
    - This function is part of the `<numeric>` library.
    - It calculates the sum of elements in a range.
    - Syntax: `accumulate(start_iterator, end_iterator, initial_value)`
    - Example: `int sum = accumulate(v.begin(), v.end(), 0);`
        - It computes the sum of elements in vector `v`.


### Type 2: Individual contribution
![[Pasted image 20241004140716.png]]
problem is about determining the minimum time required to produce **t** products using **n** machines, where each machine operates at different speeds. The goal is to find out how fast the machines can collectively produce the required products while they are all working simultaneously.

```cpp
// Function to check if it is possible to produce t products in given time `mid`
bool isPossible(vector<int>& machines, int t, long long mid) {
    long long products = 0;
    for (int time : machines) {
        products += mid / time;
        if (products >= t) return true; // Early exit if we have enough products
    }
    return products >= t;
}

// Binary search to find the minimum time to produce t products
long long minTimeToProduce(vector<int>& machines, int t) {
    long long left = 1, right = 1e18, result = right;
    while (left <= right) {
        long long mid = (left + right) / 2;
        if (isPossible(machines, t, mid)) {
            result = mid; // We found a feasible solution, try to minimize further
            right = mid - 1;
        } else {
            left = mid + 1; // Try a larger time if we can't produce t products
        }
    }
    return result;
}f
```
---
### Type 3
### Problem Statement: #K_points_insert_BS

You are given `N` points on a number line, and you need to insert `K` additional points such that the maximum distance between any two consecutive points is minimized. Your task is to determine the minimum possible value of this maximum distance.

### Input:

- An integer `N` representing the number of initial points.
- An integer `K` representing the number of additional points to be inserted.
- A list of `N` integers representing the positions of the existing points on the number line.

### Output:

- Print a single integer representing the minimum possible maximum distance between any two consecutive points after inserting `K` additional points.

### Constraints:

- `1 ≤ N ≤ 10^5`
- `1 ≤ K ≤ 10^9`
- The values of the points on the number line are integers.
```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

// Function to check if it's possible to insert K points
bool canInsertKPoints(vector<int>& points, int N, int K, int maxDist) {
    int count = 0;
    for (int i = 1; i < N; i++) {
        int dist = points[i] - points[i - 1];
        count += (dist - 1) / maxDist; // Calculate how many points we need to insert
        if (count > K) {
            return false; // Can't insert all K points with this maxDist
        }
    }
    return true;
}

// Binary search to find the minimum maximum distance
int minimizeMaxDistance(vector<int>& points, int N, int K) {
    sort(points.begin(), points.end()); // Ensure points are sorted
    int left = 1, right = points[N - 1] - points[0];
    int answer = -1; //could be anything

    while (left <= right) {
        int mid = left + (left - right) / 2;
        if (canInsertKPoints(points, N, K, mid)) {
            answer = mid;  // Update answer and try smaller maxDist
            right = mid - 1;
        } else {
            left = mid + 1;  // Try larger maxDist
        }
    }
    return answer;
}

int main() {
    int N = 5, K = 2;
    vector<int> points = {1, 2, 8, 12, 15}; // Example points
    cout << "Minimum maximum distance: " << minimizeMaxDistance(points, N, K) << endl;
    return 0;
}

```
---

# Kth Sum Value #kth_Sum

![[Pasted image 20241004193646.png]]
>[!Note] iterate on the smaller array and do BS on larger array

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef pair<int, int> Pair;
typedef long long ll;
vector<int> a, b;
#define int long long
signed main()
{
    ios_base::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    int t;
    cin >> t;
    while (t--)
    {
        int n, m, k;
        cin >> n >> m >> k;
        a.resize(n);
        b.resize(m);
        for (int i = 0; i < n; i++)
        {

            cin >> a[i];
        }
        for (int i = 0; i < m; i++)
        {

            cin >> b[i];
        }
        if (n > m)
        {
            swap(n, m);
            swap(a, b);
        }
        sort(a.begin(), a.end());
        sort(b.begin(), b.end());
        int l = a[0] + b[0];
        int h = a[n - 1] + b[m - 1];
        int ans = l;
        while (l <= h)
        {
            int mid = (l + h) / 2;
            int ne = 0;
            for (int i = 0; i < n; i++)
            {
                ne += upper_bound(b.begin(), b.end(), mid - a[i]) - b.begin();
            }
            if (ne >= k)
            {
                ans = mid;
                h = mid - 1;
            }
            else
                l = mid + 1;
        }
        cout << ans << '\n';
    }
    return 0;
}
```
>[!warning]
>Remember that you must iterate in the check function over the smaller array otherwise you will get TLE. So try to find the array which is smaller and do the iteration over it and apply the upper bound over the larger function.

---
### #Longest_Continuous_SubSegment 
##### Description 

Given a binary array of length **N**. The score of an array is the length of the longest continuous subsegment consisting of only one.

Find the maximum score possible if you can change at most **K** elements of the array.

##### Input Format

The first line contains an integer **T**, the number of test cases.  
The first line of each test case contains two space-separated integer **N, K** where   
The next line contains **N** space-separated integers which are either 0 or 1.

```cpp
#include <bits/stdc++.h>
using namespace std;
int arr[1000000];
int n,k;
bool check(int x)
{
   int cnt = 0,i;
   for(i=0;i<x;i++)
   {
       if(arr[i]==0)
           cnt++;
   }
   if(cnt<=k)
       return true;
   for(i=x;i<n;i++)
   {
       if(arr[i-x]==0)
           cnt--;
       if(arr[i]==0)
           cnt++;
       if(cnt<=k)
           return true;
   }
   return false;
}
int main()
{
   ios_base::sync_with_stdio(false);cin.tie(0);cout.tie(0);
   int testcases;
   cin>>testcases;
   while(testcases--){
       int i;
       cin>>n>>k;
       for(i=0;i<n;i++)
           cin>>arr[i];
       int low = k;
       int high = n;
       int ans = low;
       while(low<=high)
       {
           int mid=(low+high)/2;
           if(check(mid))
           {
               ans = mid;
               low=mid+1;
           }
           else
               high = mid-1;
       }
       cout<<ans<<"\n";
   }
}```
>[!Notice]
>Here the monotonic space is 1111100000 not the general case, here we are finding the maximum instead of the minimum


## Kth element problems
![[Pasted image 20241005212227.png]]

