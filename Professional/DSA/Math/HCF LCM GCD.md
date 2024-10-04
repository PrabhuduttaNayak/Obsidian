21/09/2024 15:59

Tags: #HCF #gcd #euclidean #lcm

Status: Directly used as  `__gcd(a, b)`:
# HCF and LCM

## Euclidean Algorithm:

- **Definition**: The Euclidean algorithm is an efficient method for computing the greatest common divisor (GCD) of two integers.
- **Idea**: The algorithm repeatedly replaces one of the two numbers with the remainder of the division of the other number until the remainder becomes zero. At this point, the other number will be the GCD.
- **Process**:
    1. Given two positive integers a and b, where a>b:
    2. Divide a by b and get the remainder r.
    3. If r is zero, then b is the GCD.
    4. If r is not zero, replace a with b and b with r, and repeat step 2.
- **Properties**:
    - The algorithm works efficiently for positive integers, and it can be extended to handle negative integers by considering their absolute values.
- **Time Complexity**: The time complexity of the Euclidean algorithm is O(log(min(a,b))), where a and b are the two integers.

---

### Highest Common Factor (HCF):

- **Definition**: The HCF or Greatest Common Divisor (GCD) of two or more integers is the largest positive integer that divides each of the integers without leaving a remainder.
- **Notation**: HCF(a, b) or GCD(a, b)
- **Properties**:
    1. HCF(a, 0) = a (for any integer a)
    2. HCF(a, b) = HCF(b, a) (commutative property)
    3. HCF(a, b) = HCF(a - b, b) (Euclid's algorithm)

#### Implementation in C++:

```cpp
#include <iostream>
using namespace std;

// Function to calculate HCF/GCD
int hcf(int a, int b) {
    if (b == 0)
        return a;
    return hcf(b, a % b);
}

int main() {
    int num1, num2;
    cout << "Enter two numbers: ";
    cin >> num1 >> num2;
    
    cout << "HCF of " << num1 << " and " << num2 << " is: " << hcf(num1, num2) << endl;
    
    return 0;
}
```

Copy

- **Time Complexity**: O(log(min(a, b)))

### Lowest Common Multiple (LCM):

- **Definition**: The LCM of two or more integers is the smallest positive integer that is divisible by each of the integers.
- **Notation**: LCM(a, b)
- **Properties**:
    1. LCM(a, b) = (a * b) / HCF(a, b)
    2. LCM(a, b) = LCM(b, a) (commutative property)

#### Implementation in C++:

```cpp
#include <iostream>
using namespace std;

// Function to calculate LCM
int lcm(int a, int b) {
    return (a * b) / hcf(a, b); // Using HCF function from previous example
}

int main() {
    int num1, num2;
    cout << "Enter two numbers: ";
    cin >> num1 >> num2;
    
    cout << "LCM of " << num1 << " and " << num2 << " is: " << lcm(num1, num2) << endl;
    
    return 0;
}
```




### Examples\
##### Description #Bezout

_**Ax + By = C**_

Given three positive integers _A_, _B_ and _C_.

You have to determine whether there exists at least one solution for some integers value of _x_ and _y_ where _x, y_ may be negative or non-negative integers.

>[!info] Bezout's identity states that 
for any two integers a and b, their greatest common divisor (GCD) can be expressed as the smallest positive integer that can be expressed as a linear combination of a and b, that is:
*gcd(a,b)=ax+by* where x and y are integers.

This shows that if the GCD of A and B divides C, then there exists a solution for the equation Ax+By=CAx+By=C in integers.

```cpp
int A, B, C;
        cin >> A >> B >> C;
        int G = __gcd(A, B);
        if(C % G == 0) cout << "Yes\n";
        else cout << "No\n";
```