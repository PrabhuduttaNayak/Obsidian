22/10/2024 11:57

Tags:

Status: [[Prime]] [[00_DSA]]

# Totient

![[Pasted image 20241022115858.png]]

### Example in C++

```cpp
#include <iostream>
using namespace std;

// Function to compute the Totient of n
int phi(int n) {
    int result = n;   // Initialize result as n

    // Consider all prime factors of n and subtract their multiples from result
    for (int p = 2; p * p <= n; ++p) {
        // Check if p is a prime factor
        if (n % p == 0) {
            // If p is a prime factor, subtract multiples of p from result
            while (n % p == 0)
                n /= p;
            result -= result / p;
        }
    }

    // If n has a prime factor greater than sqrt(n) (a prime number larger than sqrt)
    if (n > 1)
        result -= result / n;

    return result;
}

int main() {
    int n;
    cout << "Enter a number: ";
    cin >> n;
    cout << "Totient of " << n << " is " << phi(n) << endl;
    return 0;
}

```
### Explanation:

- **Initialization**: The `phi(n)` function starts by initializing the result as n.
- **Prime factorization loop**: The for-loop runs through all integers p up to sqrt{n}​ to check if p is a prime factor of n. If p divides n, it reduces n by dividing out all powers of p and updates the result by subtracting multiples of p.
- **Final check**: If n still has a prime factor greater than sqrt{n}, the function reduces the result by that prime factor.