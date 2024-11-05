22/10/2024 19:14

Tags: #binomial_Coefficient

Status:

# Miscellaneous

##### Description

Your task is to find the answer to the following sum.

![](https://lh3.googleusercontent.com/Pg9_ZmO-6k3fD_6Rn2JA5MU8eB4CjF-TwQTPfd2AllMyGWCynn1I-qyqOxvi_CEUtPf8gpBzWrfxAMvpA6_vzuLBRO9gEPUcFzFAgrUIRqbAk2JFsPbqTzwU9TPQHqf1abpAMd2I)

##### Input Format

The only line of input contains two space-separated integers _n_ and _m_.





### Fermat's Little Theorem #fermat_Theorem
![[Pasted image 20241022194402.png]]

# Hockey Stick Formula #pascal_triangle
![[Pasted image 20241027132637.png]]
sum of those diagonal elements is equal to the last head of the end, hence Hockey Stick



---
# Vandermonde's Identity

![[Pasted image 20241027132837.png]]

---

![[Pasted image 20241027133137.png]]

---

![[Pasted image 20241027133254.png]]


---

# Catalan Number #ratt_lo #important }
![[Pasted image 20241027135946.png]]

---
# #Fibonacci Numbers

![[Pasted image 20241027173807.png]]
![[Pasted image 20241027174854.png]]

![[Pasted image 20241027174913.png]]
![[Pasted image 20241027174920.png]]

![[Pasted image 20241027175302.png]]
divides

![[Pasted image 20241027175320.png]] #Fibonacci #gcd 

---
##### Description

For a given **n**, find the number of even and odd numbers among the set, **{ nC0, nC1,... nCn }.**

##### Input Format

First-line contains **T** **(1 ≤ T ≤ 105),** the number of test cases. Next T lines contain one integer per line, denoting **n (0 ≤ n ≤ 1012)**. 

##### Output Format

For each test case, output two space-separated integers specifying the number of even numbers and odd numbers respectively.

##### Constraints

##### Sample Input 1

2 3 4

##### Sample Output 1

0 4 3 2

##### Note

For 3, values are: **1 3 3 1**. All are odd. Hence **0 4.**  
For 4, values are: **1 4 6 4 1**. Hence **3 2.**

```cpp
#include <bits/stdc++.h>

using namespace std;

typedef long long ll;

signed main()
{
	ios_base::sync_with_stdio(0);
	cin.tie(0); cout.tie(0);

	int T; cin >> T;
	while(T--) {
		ll n; cin >> n;
		ll ans = 1;
		for(ll i = 0; i < 64; i++) {
			if((n >> i) & 1) {
				ans *= 2;
			}
		}
		cout << n + 1 - ans << " " << ans << "\n";
	}
}

```
Using Lucas Theorem ([https://en.wikipedia.org/wiki/Lucas%27s_theorem](https://en.wikipedia.org/wiki/Lucas%27s_theorem)) we can find that _**iff x & y = y, then xCy will be odd. Otherwise even.**_ That's it! Find the number of integers in the submask in **n.**  
See the solution code for more implementation

---

![[Pasted image 20241028175806.png]]


---
![[Pasted image 20241028183328.png]]


---
##### Description

Find the number of solutions of following equation.

_x1_ + _x2_ + _x3_ + … + _xn_ = _k_, satisfying that 0 ≤ _xi_ < _m_, modulo 1000000007.

##### Input Format

The only line of input contains three space-separated integers _n, m, k_.

##### Output Format

Print the number of solutions.

##### Constraints

1 ≤ _n, m, k_ ≤ 105

##### Sample Input 1

2 4 5

##### Sample Output 1

2

##### Sample Input 2

20 10 50

##### Sample Output 2

366736536

##### Note

For the first test cases, only two solutions exist (2, 3) and (3, 2).

# Solution Approach

![coin](https://maang.in/img/coding/up.svg)

The only thing we need to handle is to get rid of that annoying constraint _xi < m_. To do that, we apply the inclusion-exclusion principle. Let _ei = xi ≥ m_, then _No_ is our desired answer. Clearly, this set of properties is homogeneous. Take _T = {1, 2, …, j} (j ≤ n)_, then _NT_ is the number of solutions with _x1 ≥ m, x2 ≥ m, …, xj ≥ m_. Setting _yi = xi - m(i ≤ j), yi = xi(i > j)_, and it's the same as the number of solutions of the system

_y1 + y2 + … + yn = k - jm_,

thus the answer is therefore

_No = Summation over j = [0, n] (-1)j * nCj * (n + k - jm - 1) C (n - 1)_

**Time complexity:** _O(n)_ with some preprocessing.
#factorial #power #nCr  #inclusion_exclusion

```cpp
#include<bits/stdc++.h>
using namespace std;

#define ll long long int

const int N = 500010;

int mod = 1e9 + 7;

int power(int a, int b, int M) {
    if(!b) return 1;
    int temp = power(a, b / 2, M);
    temp = 1LL * temp * temp % M;
    if(b % 2) temp = 1LL * temp * a % M;
    return temp;
}

int fact[N];

void pre() {
    fact[0] = 1;
    for(int i = 1; i < N; i++) {
        fact[i] = 1LL * fact[i - 1] * i % mod;
    }
}

int ncr(int n, int r) {
    if(n < r) return 0;
    assert(n >= 0 && n < N && r >= 0 && r < N);
    int ans = fact[n];
    ans = 1LL * ans * power(fact[n - r], mod - 2, mod) % mod;
    ans = 1LL * ans * power(fact[r], mod - 2, mod) % mod;
    if(ans < 0) ans += mod;
    assert(ans >= 0 && ans < mod);
    return ans;
}

signed main()
{
    ios_base::sync_with_stdio(0);
    cin.tie(0); cout.tie(0);

    pre();
    int n, m, k;
    cin >> n >> m >> k;

    int ans = 0;

    for(int i = 0; i <= n; i++) {
        int temp = ncr(n, i);
        if(n + k - i * m - 1 < n - 1) break;
        temp = 1LL * temp * ncr(n + k - i * m - 1, n - 1) % mod;
        if(temp < 0) temp += mod;
        if(i % 2) ans = (ans - temp + mod) % mod;
        else ans = (ans + temp) % mod;
    }

    assert(ans >= 0 && ans < mod);

    cout << ans << "\n";

    return 0;
}

```
---
#### Count Good Subsequences #unsolved #nCr #inclusion_exclusion 

##### Description

You are given a sequence _A1, A2, …, AN_. Let's call a subsequence _Ai1, Ai2, …, Aik_ (for any _k_ > 0, 1 ≤ _i1_ < _i2_ < … < _ik_ ≤ _N_) good if the median of this subsequence is an element of this subsequence.   
Find the number of good subsequences. Since this number may be large, compute it modulo 1000000007 (109+7).

_Note:_

1. The median of a set of data is the middlemost number in the set. The median is also the number that is halfway into the set. To find the median, the data should first be arranged in order from least to greatest.
2. For odd length sequence, the median is the middle element in the sorted sequence. While for even length sequence, it is the average of the middle two elements.

##### Input Format

The first line of input contains _T_ - the number of test cases.  
The first line of each test case contains a number _N_ - the size of the array.  
The second line of each test case contains _N_ space-separated integers _A1, A2, ..., AN_.

##### Sample Input 1

1 3 2 3 2

##### Sample Output 1

5

# Solution Approach

![coin](https://maang.in/img/coding/up.svg)

First of all, let us formulate the definition of median. If the size of the selected subset is odd, the median is just the middle element of subset after sorting. Since the middle element is present in the subset, all subsets of odd size are valid. It can be easily proven that there are 2_N_−1 such subsets. So, we can directly count these and move toward even size subsets.  
If the size of the selected subset is even, the median is defined as the average of two middlemost elements after sorting. Now, say we have two middle elements _x_ and _y_, with condition _x_ ≤ _y_. Let _z_ = (_x_ + _y_) / 2 be the median of sequence. If we write _y_ = _x_ + _d_, _d_ ≥ 0, we can see, _z_ = _x_ + _d_ / 2 and also, _z_ = _y_ − _d_ / 2. This way, we can see, that the median of a sequence can never be smaller than _x_ and greater than _y_. So, For _z_ to be present in subset, we need either _z_ = _x_ or _z_ = _y_. But, this would imply _d_ = 0, Hence leading to the conclusion that f**or an even size subset to be valid, the two middlemost elements should be equal.** This forms the crux of our solution, and now, we need to count the number of even sized subsets with equal middle elements.  
After all this, there are a number of approaches to solving this problem, all of which required us to sort the array A_A_ first.

Let us consider a _O_(_N_3) solution first. Iterate over every pair of equal elements (_i_, _j_) such that _Ai_ ​= _Aj_​ and iterate over the size 2_X_ of subset from _X_ = 1 to _N_. The number of ways to make the subset of size _X_ with two fixed middle elements is just the product of the number of ways we can select _X_ − 1 elements from [1, _i_ − 1] and _X_ − 1 elements from [_j_ + 1, _N_].

This solution requires to iterate over every pair (_i_, _j_) which takes _O_(_N_2) time and _O_(_N_) time per pair, leading to Overall time complexity _O_(_N_3).

We were fixing two equal elements and tried to count the number of ways we can make subsets of all sizes. Now, We shall fix only the **Left Middle Element** (Or Right one, whichever implementation you prefer).

Suppose we fixed the _i_th element as the left middle element. Now, We will iterate over all sizes 2_X_ and try to include _X_ − 1 elements from [1, _i_ − 1] and _X_ elements from [_i_ + 1, _N_]. We need the right middle element to be same as the left middle element. So, When choosing _X_ elements from [_i_+1,_N_], we need at least one occurrence of _A_[_i_]. This is same as subtracting all the ways to select _X_ elements in the range [_i_ + 1, _N_] which do not have _A_[_i_] at all. Suppose Number of occurrence of _A_[_i_] in range [_i_ + 1, _N_] is _f_, then we can count the number of ways to select _X_ elements from range [_i_ + 1, _N_] such that it contains at least one occurrence of _A_[_i_] as _T_ = Number of ways to select _X_ elements out of _N_−_i_ elements less Number of ways to select _X_ elements out of _N_−_i_−_f_ elements.

We can select _X_−1 elements from [1, _i_ − 1] in suppose _U_ ways. Then, Number of ways we can have good subsets with _A_[_i_] as the left middle element is _UT_. Summation of this product for all sizes for all elements gives us the number of good subsets of even size. We can add to it, the number of good odd sized subsets and print the answer.

```cpp
#include <bits/stdc++.h>
using namespace std;

using ll = long long;
vector<ll> fact;
vector<ll> factInv;
int mod = 1e9 + 7;

ll binpow(ll a, ll b) {
  ll ans = 1;
  while (b) {
    if (b % 2) ans = (ans * a) % mod;
    a = (a * a) % mod;
    b /= 2;
  }
  return ans;
}

ll ncr(ll n, ll r) {
  return (((fact[n] * factInv[n - r]) % mod) * factInv[r]) % mod;
}

void solve() {
  int n;
  cin >> n;
  vector<int> v(n);
  vector<int> pos(n, 1);
  for (int i = 0; i < n; i++) {
    cin >> v[i];
  }
  sort(v.begin(), v.end());

  for (int i = n - 2; i >= 0; i--) {
    if (v[i] == v[i + 1]) {
      pos[i] += pos[i + 1];
    }
  }

  ll ans = binpow(2, n - 1);

  for (int i = 0; i < n; i++) {
    if (pos[i] == 1) continue;
    int temp = min(i, n - i - 1 - 1);
    for (int j = 0; j <= temp; j++) {
      if (j == 0) {
        ans = (ans + ncr(pos[i + 1], j + 1)) % mod;
        continue;
      }
      ans += (ncr(i, j) * ncr(n - 1 - i, j + 1)) % mod;
      ans %= mod;
      if (n - 1 - i - pos[i + 1] >= (j + 1)) {
        ans -= (ncr(i, j) * ncr(n - 1 - i - pos[i + 1], j + 1)) % mod;
        ans = (ans + mod) % mod;
      }
    }
  }
  cout << ans << "\n";
}

signed main() {
  ios_base::sync_with_stdio(0);
  cin.tie(0);
  cout.tie(0);
  fact = vector<ll>(2001);
  factInv = vector<ll>(2001);
  fact[0] = 1;
  for (int i = 1; i <= 2000; i++) {
    fact[i] = (i * fact[i - 1]) % mod;
  }
  factInv[2000] = binpow(fact[2000], mod - 2);
  for (int i = 2000; i > 0; i--) {
    factInv[i - 1] = (i * factInv[i]) % mod;
  }

  int _t;
  cin >> _t;
  while (_t--) solve();
}

```


---

# Josephus Problem

![[Pasted image 20241029221023.png]]

only works for k=2

The **Josephus problem** is a theoretical problem related to a famous counting-out game. In this problem:

1. There are nnn people standing in a circle, numbered from 1 to nnn.
2. Starting from a chosen position, you count around the circle and eliminate every kkk-th person.
3. This counting continues until only one person remains, who is the winner.

The goal is to find the position of the last remaining person.

### Problem Explanation and Recursive Solution

For a given nnn (total number of people) and kkk (step size), we need to find the position of the last survivor. Let’s denote the position of the last survivor with nnn people as Josephus(n,k)\text{Josephus}(n, k)Josephus(n,k).

#### Recursive Relation

1. **Base Case**: If there is only one person, they survive:
    
    Josephus(1,k)=0\text{Josephus}(1, k) = 0Josephus(1,k)=0
    
    (we use 0-based indexing, meaning the position is 0).
    
2. **Recursive Step**: For n>1n > 1n>1, if we know the position of the survivor in a group of n−1n - 1n−1 people, we can extend it to nnn people as follows:
    
    Josephus(n,k)=(Josephus(n−1,k)+k)mod  n\text{Josephus}(n, k) = (\text{Josephus}(n - 1, k) + k) \mod nJosephus(n,k)=(Josephus(n−1,k)+k)modn
    
    This formula accounts for the circular nature of the counting, with each recursive call reducing the problem by one person.
    

### Code for Recursive Solution

Here’s the recursive C++ code for the Josephus problem:

```cpp
#include <iostream>
using namespace std;

// Function to solve the Josephus problem recursively
int josephus(int n, int k) {
    // Base case: only one person remains
    if (n == 1)
        return 0;
    // Recursive call: find position in reduced problem and adjust for current size
    return (josephus(n - 1, k) + k) % n;
}

int main() {
    int n, k;
    cout << "Enter the number of people (n): ";
    cin >> n;
    cout << "Enter the step size (k): ";
    cin >> k;

    // The function returns 0-based index, so add 1 for 1-based index
    cout << "The safe position is " << josephus(n, k) + 1 << endl;
    return 0;
}
```

### Explanation of the Code

1. **Recursive Function**:
    
    - `josephus(n, k)` calls itself with a smaller group of n−1n - 1n−1 until it reaches the base case where n=1n = 1n=1.
    - Each recursive call reduces the circle by one person and adjusts the survivor position using modular arithmetic to account for the circle.
2. **1-Based Indexing**:
    
    - Since the Josephus problem is often presented with 1-based indexing (the first person is at position 1), we add `1` to the result in `main()`.
-

```cpp
int josephusIterative(int n, int k) { int result = 0; // Starting with 0-based indexing 
	for (int i = 2; i <= n; i++) { 
		result = (result + k) % i; } 
	return result; }
```

---

#### Tough Mex #unsolved 

##### Description

For a (possibly empty) sequence of positive integers _S_, mex is defined as _f_(_S_) as the smallest positive integer that does not appear in _S_. For example, _f_([]) = 1, _f_([3,4,3,5]) = 1, _f_([2,5,1,1,2,3]) = 4.

You have given a sequence of _N_ integers _A1, A2, …, AN_. Your task is to find the sum of _f_(_S_) over all 2_N_ possible subsequences _S_ of this sequence.

Since the resulting sum can be very big, compute it modulo 998244353.

##### Input Format

The first line of the input contains a single integer _T_ denoting the number of test cases. The description of _T_ test cases follows.  
The first line of each test case contains a single integer _N_.  
The second line contains _N_ space-separated integers _A1, A2, …, AN_.

##### Output Format

For each test case, print a single line containing one integer ― the sum of _f_(_S_) over all subsequences modulo 998244353.

##### Constraints

1 ≤ _T_ ≤ 10  
1 ≤ _N_ ≤ 105  
1 ≤ _Ai_ ≤ 109 for each valid _i_

##### Sample Input 1

2 2 1 1 3 1 2 1

##### Sample Output 1

7 17

##### Note

**Example case 1:** The values for all possible subsequences are _f_([]) = 1, _f_([1]) = 2, _f_([1]) = 2, _f_([1,1]) = 2.

**Example case 2:** The values for all possible subsequences are _f_([]) = 1, _f_([1]) = 2, _f_([2]) = 1, _f_([1]) = 2, _f_([1,2]) = 3, _f_([2,1]) = 3, _f_([1,1]) = 2, _f_([1,2,1]) = 3.

# Solution Approach

![coin](https://maang.in/img/coding/up.svg)

The mex of a subsequence of length _N_ cannot exceed _N_. So we can replace all elements greater than _N_ with _N_.  
Fixing the value of mex, let’s try to count the number of subsequences with specific mex.  
For a given mex, we need each positive value smaller than mex to be present at least once, value mex shouldn’t be present, and values greater than mex do not affect us.  
If _fx_​ denote the frequency of value _x_, the number of non-empty subsets of these _fx_​ elements is 2_fx_​−1. We need to select one of the subsets for each _x_ less than current mex, giving us (2f1 - 1) * (2f2 - 1)  _…. *_ (2f(x-1) - 1).  
To consider elements greater than mex, we can take any subset of those. If there are _y_ elements greater than mex, it contributes 2_y_ subsets for each choice of subsets of smaller values.


```cpp
#include <bits/stdc++.h>

using namespace std;

typedef long long ll;

const int N = 200010;
const int MOD = 998244353;

int t, n; 
ll two[N], a[N], freq[N], sf[N];

int main() {
  two[0] = 1;
  for (int i = 1; i < N; ++i) two[i] = 2 * two[i - 1] % MOD;
  cin >> t;
  while (t--) {
	scanf("%d", &n);
	for (int i = 0; i < n; ++i) {
	  scanf("%lld", a + i);
	  ++freq[min(a[i], n + 1LL)];
	}
	sf[n + 69] = 0;
	for (int i = n + 68; i; --i) sf[i] = sf[i + 1] + freq[i];
	ll ans = 0, pf = 1;
	for (ll i = 1; i <= n + 1; ++i) {
	  ans = (ans + i * (pf * two[sf[i + 1]] % MOD)) % MOD;
	  pf = pf * (two[freq[i]] - 1) % MOD;
	}
	ans += MOD, ans %= MOD;
	printf("%lld\n", ans);
	for (int i = 0; i <= n + 69; ++i) freq[i] = 0;
  }
  return 0;
}

```


----
##### Description

Let us consider a set of fractions x _/ y_, such that 0 ≤ _x / y_ ≤ 1, _y_ ≤ _n_ and _gcd (x, y)_ = 1.

For example, say _n_ = 5. Then we have the following set in increasing order : 0/1, 1/5, 1/4, 1/3, 2/5, 1/2, 3/5, 2/3, 3/4, 4/5, 1/1

You are given _n_, _a_ and _b_. The task is to find the rank of _a / b_ in a set of fractions as stated above which is in increasing order.

_Note:_ Fractions should be finite.

##### Input Format

The first line of contains a number _T_ (1 ≤ _T_ ≤ 20) - the number of testcases. Then _T_ lines follow.  
In each of _T_ lines you are given _n, a, b_ (1 ≤ _n_ ≤ 100000, _a_ ≤ _b_ ≤ _n_) such that _gcd(a, b)_ = 1.

##### Output Format

Print _T_ lines. The _i_th line contains the rank of a fraction _a / b_ for a given _n, a_ and _b_ in the _(i + 1)_th line of input.

##### Constraints

##### Sample Input 1

5 5 3 4 8 5 7 123 23 32 500 99 123 1000 501 611

##### Sample Output 1

9 17 3332 61269 249428

##### Note

_**Explanation 1:**_  
For _n_ = 5, we have the following set in increasing order: 0/1, 1/5, 1/4, 1/3, 2/5, 1/2, 3/5, 2/3, 3/4, 4/5, 1/1  
So the rank of 3 / 4 is 9.