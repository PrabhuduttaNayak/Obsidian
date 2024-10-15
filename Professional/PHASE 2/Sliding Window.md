2024-10-06 21:02

Tags:  #sliding_window #two_pointer #2pointer

Status:[[00_DSA]] [[Binary Search]]

# Sliding Window

##### Description

Given an array of _N_ integers, find the number of subarrays with a sum less than equal to _K_

- The first line contains _T_, the number of test cases _(1<=T<=10)_.
- The first line of each test case contains two space-separated integers _N, K_ where _1<=N<=10^5, 0<=K<=10^9_.
- Next line contains _N_ space-separated integers _(0<=Ai<=1e4)_.

### Solution
We initialize two pointers - start and current to 0. We also initialize the current sum to 0 and the answer to 0.

We start iterating through the array using the current pointer and add each element to the current sum. If the current sum is greater than K, we start incrementing the start pointer and subtracting the elements from the current sum until the sum is less than or equal to K. We count the number of subarrays using the formula (current-start+1) and increment the answer. Finally, we return the answer as the number of subarrays with a sum less than or equal to KK.

**Time complexity : O(N), where N is the size of the array.**  
The time complexity of the algorithm is O(N), as we are iterating through the array only once, and each element is processed once.

```cpp
#include<bits/stdc++.h>
using namespace std;
#define int long long

void solve()
{
    int n, k;
    cin>>n>>k;
    int arr[n];
    for(int i =0;i<n;i++)
        cin>>arr[i];
    
    int head = -1, tail = 0, ans = 0, sum = 0;

    while(tail<n)
    {
        while(head + 1<n && arr[head+1] + sum <= k) //if it was do while loop we wouldn't have done +1
        {
            head++;
            sum += arr[head];
        }
        ans += head - tail + 1; //for every window
        
        if(tail>head) //important condition
        {
            tail++;
            head = tail - 1;
        } 
        else
        {
            sum -= arr[tail];
            tail++;
        }
    }
    cout<<ans<<endl;
}

signed  main()
{
    ios_base::sync_with_stdio(0);
    cin.tie(0);cout.tie(0);
    int t = 1;
    cin>>t;
    for(int i =0;i<t;i++)
    {
        solve();
    }
    return 0;
}
```

>[!NOte]
>this part is important:
>`int head = -1, tail = 0;`
>`while(tail<n);`
>`if(tail>head) {tail++; head = tail - 1;}`
>

### 3 Sum #3_sum #threesome
##### Description

Given an array _A_ of _N_ integers and an integer _target_, find three integers in _A_ such that the sum is closest to the _target (absolute value of (sum-target) is minimum)_. Print the minimum absolute value of (sum-target). You cannot select an index more than one. All three indexes should be distinct.

##### Input Format

The first line contains _T_, the number of test cases _(1<=T<=100)_.

The first line contains two space-separated integers _N, target_ where 3_<=N<=10^3, -1e18<=target<=1e18._

Next line contains _N_ space-separated integers (-1e9<=Ai<=1e9).

The Sum of _N_ across all test cases ≤ 10^4.

>[!Tip]
>We iterate through the array and fix the first number of the array (index, K), and then use the two-pointer technique on the array to its right side. 
>
>We take the left pointer as L = K + 1 and the right pointer as R = size(A) - 1. 
>
>This would help us ensure that all the indexes taken are distinct. Find the sum of the three integers and take the minimum absolute difference between all such sums and target. 
>
>If the sum < target, decreasing the value of R would only decrease the sum and hence increase the absolute difference, so we increase the value of L.
> 
>Similarly if sum > target, we decrease the value of R. Continue doing the two-pointer until L < R and find the minimum possible absolute difference of sum and target.

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
signed main()
{
   ios_base::sync_with_stdio(false);cin.tie(0);cout.tie(0);
   int testcases;
   cin>>testcases;
   while(testcases--){
   
       ll n,target;
       cin>>n>>target;
       vector< ll > arr(n);
       for(int i=0;i<n;i++)
           cin>>arr[i];
       sort(arr.begin(),arr.end());
       ll best = arr[0]+arr[1]+arr[2];
       
       for(ll i=1;i<(ll)arr.size()-1;i++){
           ll lo = 0;
           ll hi = (ll)arr.size()-1;
           
           while(1){
               if(abs(arr[lo]+arr[i]+arr[hi]-target)<abs(best-target)){
                   best=arr[lo]+arr[i]+arr[hi];
               }
               if(arr[lo]+arr[i]+arr[hi]>target)hi--;
               else lo++;
               if(lo==i||hi==i)break;
           }
       }
       cout<<abs(best-target)<<"\n";
   }
}```

another approach:
### Explanation:

1. **Sorting the array** ensures that we can use two pointers (`left` and `right`) efficiently.
    
2. **For each element `A[i]`**:
    
    - We treat it as the first number of the triplet.
    - The other two numbers are chosen by moving the `left` pointer from `i+1` and the `right` pointer from the end of the array (`n-1`).
3. **Updating closest sum**:
    
    - If the current sum is closer to the target than the previously stored closest sum, update the `closestSum`.
    - Adjust the `left` and `right` pointers based on whether the current sum is less or greater than the target.
4. The function returns the minimum absolute value of `(sum - target)`.

```cpp
#include <bits/stdc++.h>
using namespace std;

int closestThreeSum(vector<int>& A, int target) {
    // Sort the array
    sort(A.begin(), A.end());
    int n = A.size();
    int closestSum = INT_MAX;  // To store the closest sum difference

    // Iterate through each element
    for (int i = 0; i < n - 2; i++) {
        int left = i + 1, right = n - 1;

        // Use two pointers to find the closest sum
        while (left < right) {
            int currentSum = A[i] + A[left] + A[right];
            
            // If the current sum is exactly the target
            if (currentSum == target) {
                return 0; // No need to proceed further if exact match
            }
            
            // If the current sum is closer to the target, update closest sum
            if (abs(currentSum - target) < abs(closestSum)) {
                closestSum = currentSum - target;
            }

            // Adjust the pointers
            if (currentSum < target) {
                left++; // Move left pointer to increase the sum
            } else {
                right--; // Move right pointer to decrease the sum
            }
        }
    }
    // Return the minimum difference
    return abs(closestSum);
}

int main() {
    vector<int> A = {-1, 2, 1, -4}; // Example array
    int target = 1;
    cout << closestThreeSum(A, target) << endl;
    return 0;
}

```


---
# Median of Subarray Sum #Question_Type  #hard #unsolved 
#median_subarray_sum
##### Description [[Binary Search]]

Given an array of _N_ integers _A_. The score of a subarray is the sum of all integers in the subarray. Mr.X calculated the score of all _N*(N+1))/2_ subarray. Mr.X wants you to find the median of the array containing the score of all the subarray

##### Input Format

The first line contains an integer _T_, the number of test cases _(1<=T<=10000)_.

The first line of each test case contains an integer _N_ where _(1<=N<=10^5)_.

Next line contains _N_ space-separated integers _(0<=Ai<=1e9)._

Sum of _N_ across all test cases ≤ 10^6.

>[!Hint]
>We would be doing binary search on the possible values of the median. Here, we would be using the array which has the subarray sums of the original array. We take the left pointer as L = 0 or L = minimum value present in the new array and we take the right pointer as R = sum of all the numbers in the original array. We take mid = (L+R)/2. We need to find the number of values ≤ mid.
It would not be possible to create the new array since N ≤ 105 and the new array would be of size N*(N+1)/2. To avoid making the new array, we will use two pointers to calculate the subarray sums. If a sum from i to j is ≤ mid, that means all sums from i to (values than j) is ≤ mid.
If the number of values ≤ mid is greater than or equal to (M + 1)/2, that means mid can be a possible value of the median or it would be lesser than mid, so we continue binary search on L to R = mid - 1. Otherwise, the value of median would be > mid and so we continue binary search on L = mid+1 to R. Here, M = N*(N+1)/2.
Time Complexity per test case: O(N * log2(ΣAi))

```cpp
#include <bits/stdc++.h>
using namespace std;
#define endl '\n'
#define int long long


bool check(int mid,int k,vector<int>&arr){
	int cnt=0;
	int tail=0,head=-1;
	int n=arr.size();
	int sum=0;

	while(tail<n){
		while(head+1<n and (sum+arr[head+1]<=mid)){
			head++;
			sum+=arr[head];
		}
		cnt+=(head-tail+1);
		if(tail>head){
			tail++;
			head=tail-1;
		}else{
			sum-=arr[tail];
			tail++;
		}
	}
	return cnt>=k;
}



void solve(){

	int n;cin>>n;
	int l=0;
	int r=0;
	vector<int>arr(n);
	for(int i=0;i<n;i++){
		cin>>arr[i];
		r+=arr[i];
	}
	int tot=n*(n+1)/2;

	int k=(tot+1)/2;


	int ans=0;

	while(l<=r){
		int mid=l+(r-l)/2;
		if(check(mid,k,arr)){
			ans=mid;
			r=mid-1;
		}else{
			l=mid+1;
		}
	}
	cout<<ans<<endl;

}

signed main(){
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);cout.tie(NULL);	
    int _t;cin>>_t;while(_t--)
    solve();
}
```

>building the array was not possible, hence BS

---

#### K Odd Number
##### Description

Given an array of _N_ integers, find a subarray with at most _K_ odd numbers and the total sum is maximum but not more than _D_. If no such subarray exists print _"IMPOSSIBLE"_ without double-quotes.

##### Input Format

The first line contains _T_, the number of test cases _(1<=T<=10)_.

The first line contains two space separated integers _N, K, D_ where _1<=N<=10^5,  0<=K<=10^5, -1e9<=D<=1e9_.

Next line contains _N_ space-separated integers _(-1e4<=Ai<=1e4).

>[!Note]
>- Here Ai can also be negative, therefore the subarray sum is not uniformly increasing, its more like a wave which can either increase or decrease. Therefore you cant directly maintain a sliding current sum window. ( I made this mistake, hence you cant maintain subarray sum window here using 2 pointer, when Ai is Negative)
>- Check that here are 2 window properties one is Number of K and Subarray sum of D
>


```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
signed main()
{
   ios_base::sync_with_stdio(false);cin.tie(0);cout.tie(0);
   ll testCases;
   cin>>testCases;
   while(testCases--)
   {
     ll n,k,s;
     cin>>n>>k>>s;
     ll arr[n+1];
     ll prefix[n+1];
     prefix[0] = 0;
     ll ans = -1e18;
     for(ll i=1;i<=n;i++){
       cin>>arr[i];
       prefix[i]=prefix[i-1]+arr[i];
     }
     ll cntOdd = 0;
     multiset < ll > currWindowPrefixSum;
     currWindowPrefixSum.insert(0);
     ll prev = 1;
     for(ll i=1;i<=n;i++){
       cntOdd+=(abs(arr[i])%2);
       if(cntOdd>k){
           while(prev<=i){
               currWindowPrefixSum.erase(currWindowPrefixSum.find(prefix[prev-1]));
               if(abs(arr[prev])%2){
                   prev++;
                   cntOdd--;
                   break;
               }
               prev++;
           }
       }
       if(!currWindowPrefixSum.empty()){
           auto itr = currWindowPrefixSum.lower_bound(prefix[i]-s);
           if(itr!=currWindowPrefixSum.end()){
               ans = max(ans,prefix[i]-(*itr));
           }
       }
       currWindowPrefixSum.insert(prefix[i]);
     }
     if(ans==-1e18){
       cout<<"IMPOSSIBLE\n";
       continue;
     }
     cout<<ans<<"\n";
 }

}```

