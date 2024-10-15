21/09/2024 12:48

Tags: #precision #ceil #floor #round #binPow #inverse #prime #divisors 

Status: `MOD value is 1e9+7` #mod

# General C++


### 0. ``Precision`` 
Sometimes we need to read from stdin or write to stdout, numbers up to a certain precision. 
This is handled using **``setprecision(n)``** function, where the precision of any number is set to n. ``Fixed`` is used along with setprecision, which provides precision correct to the decimal numbers mentioned in brackets.

```cpp
cout << "Answer upto 8 decimal places : " ; 
cout << fixed << setprecision(8) << ans << "\n";
```

### 1. **`ceil()`** (Ceiling Function)

- **Rounds up to the nearest integer**.
- It returns the smallest integer **greater than or equal** to the given number.

### 2. **`floor()`** (Floor Function)

- **Rounds down to the nearest integer**.
- It returns the largest integer **less than or equal** to the given number.
- 
### 3. **`round()`** (Round Function)

- **Rounds to the nearest integer**.
- If the decimal part is **0.5 or greater**, it rounds **up**.
- If the decimal part is **less than 0.5**, it rounds **down**.
- 
### Binary Exponentiation
The function `binpow(a, b)` in the image implements binary exponentiation, where:

- `a` is the base.
- `b` is the exponent.
```cpp
lli binpow(lli a, lli b) {
    if(b == 0) return 1;  // Base case: anything to the power of 0 is 1
    if(b % 2 == 1) return a * binpow(a, b - 1) % mod; // If b is odd
    else {
        lli ans = binpow(a, b / 2);  // Recursively compute the square root
        return ans * ans % mod;      // Square the result and take mod
    }
}

```

### For Integer Exponentiation #exponent #integerPow

```cpp
int integerPow(int base, int exp) {
    int result = 1;
    while (exp > 0) {
        if (exp % 2 == 1) result *= base;
        base *= base;
        exp /= 2;
    }
    return result;
}
```
### Modular Inverse Using Binary Exponentiation #binpow #inverse

When dealing with modular arithmetic, we often need to compute the **modular inverse** of a number `x` under some modulus `MOD`. The modular inverse of `x` under `MOD` is a number `y` such that:

(x×y)%MOD=1

Using **Fermat's Little Theorem**, if `MOD` is a prime number, we can compute the modular inverse as:

x^MOD−1≡1 (mod MOD)
```cpp
binpow(x, MOD - 2) % MOD;
```


### nCr Calculation with Modular Inverse

The `ncr()` function in the image is used to calculate the **binomial coefficient** C(n,r)C(n, r)C(n,r) under modulo arithmetic.

The formula for C(n,r) is:
![[Pasted image 20240921144142.png]]

However, when working with modular arithmetic, division is not straightforward. Instead, we multiply by the modular inverse of the denominator:

![[Pasted image 20240921144203.png]]

Here’s how the code works:
you have to write the factorial ``fact`` function
```cpp
lli ncr(lli n, lli r) {
    if(r < 0 || r > n) return 0;  // Handle invalid cases
    lli den = (fact[r] * fact[n - r]) % mod;  // Compute r! * (n-r)!
    return fact[n] * binpow(den, mod - 2) % mod;  // Use modular inverse
}
```

### Prime Check #prime

```cpp
bool isPrime(int n) {
    if (n <= 1) {
        return false;
    }
    for (int i = 2; i <= sqrt(n); ++i) {
        if (n % i == 0) {
            return false;
        }
    }
    return true;
}
```

### Find Divisors #divisors

```cpp
vector<int> findDivisors(int n) {
    vector<int> divisors;
    // Iterate from 1 to sqrt(n)
    for (int i = 1; i <= sqrt(n); ++i) {
        // If i is a divisor of n
        if (n % i == 0) {
            // If the divisors are the same, only add it once (for perfect squares)
            if (n / i == i) {
                divisors.push_back(i);
            } else {
                // Add both divisors
                divisors.push_back(i);
                divisors.push_back(n / i);
            }
        }
    }
    return divisors;
}
```
---
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
