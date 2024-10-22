19/10/2024 18:10

Tags: #Segmented_Sieve #Eratosthenes

Status:

# Sieve of Eratosthenes

The **Sieve of Eratosthenes** is an efficient algorithm for finding all prime numbers up to a specified integer `n`. It works by iteratively marking the multiples of each prime starting from 2, the smallest prime number.

### How it Works:

1. Create a boolean array `prime[]` of size `n+1`, initialized to `true`. The value at index `i` in this array will indicate whether `i` is a prime number.
2. Starting with the first prime number (2), mark all its multiples as `false` (i.e., not prime).
3. Move to the next number in the list that is still marked as `true` (this will be the next prime), and repeat the process until you’ve processed all numbers up to √n.
4. The numbers that remain `true` in the boolean array after all iterations are the prime numbers.

### C++ Code Implementation:

```cpp
#include <iostream>
#include <vector>

using namespace std;

void SieveOfEratosthenes(int n) {
    // Create a boolean array to mark primes
    vector<bool> isPrime(n + 1, true);  // Assume all numbers are prime initially
    isPrime[0] = isPrime[1] = false;    // 0 and 1 are not prime numbers

    // Begin marking non-primes
    for (int p = 2; p * p <= n; p++) {
        // If isPrime[p] is still true, p is a prime
        if (isPrime[p]) {
            // Mark all multiples of p as not prime
            for (int i = p * p; i <= n; i += p)
                isPrime[i] = false;
        }
    }

    // Print all prime numbers
    for (int p = 2; p <= n; p++) {
        if (isPrime[p]) {
            cout << p << " ";
        }
    }
    cout << endl;
}

int main() {
    int n;
    cout << "Enter the value of n: ";
    cin >> n;

    cout << "Prime numbers up to " << n << " are: ";
    SieveOfEratosthenes(n);

    return 0;
}

```

### Time Complexity:

- The time complexity of the Sieve of Eratosthenes is **O(n log log n)**, making it very efficient for finding primes up to a large number `n`.

![[Pasted image 20241019182724.png]]

---


### Find the primes in the range [a,b] where 0<a<=b<=10^12  and b-a<=10^6


- Sieve the range [a,b] using primes found in the range of [1,sqrt(b)].
![[Pasted image 20241020101258.png]]

- say for a=28 and b=36, we sieve from [1,6].
- Phase 1:We use normal sieve to find in this range.
- Phase 2:

```cpp
for(prime:[1,sqrt(b)]){
	long long curMul=((a+prime-1)/prime)*prime; //ceil(a/p) * p :multiple of the prime >= a.
	while (curMul<=b){
		isPrime[curMul]=0;
		curMul+=prime;
		}
}
```


### Time Complexity:

- The time complexity of the Segmented Sieve of Eratosthenes is  `(b-a)loglogb` .