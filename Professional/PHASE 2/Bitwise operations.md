2024-10-09 08:04

Tags:

Status:

# Bitwise operations
>[!Keep in mind]
>- For signed integer do not loop from 31-0 rather 30-0 bcz 32th bit is used for sign +/-
>- while doing << or >> take `long long` because its safe to avoid overflow : (1ll<<x) 

### a. XOR (^ - Exclusive OR): #XOR

Performs a bitwise exclusive OR operation. The result is 1 if the corresponding bits are different; otherwise, it's 0.

**Example:**

```
A = 1010 (10)
B = 1100 (12)
A ^ B = 0110 (6)
```

Copy

### b. Left Shift (<<): #Left_Shift

Shifts the bits of a binary number to the left by a specified number of positions.

**Example:**

```
A = 1010 (10)
A << 2 = 101000 (40) (Shift A to the left by 2 positions)
```

Copy

### c. Right Shift (>>): #Right_Shift

Shifts the bits of a binary number to the right by a specified number of positions. For unsigned integers, zeros are shifted in from the left. For signed integers, the behavior is implementation-dependent.

**Example:**

```
A = 1010 (10)
A >> 1 = 0101 (5) (Right shift A by 1 position)
```

Copy

### d. NOT (~): #NOT

Inverts each bit of a binary number (flips 0s to 1s and vice versa).

**Example:**

```
A = 1010 (10)
~A = 0101 (5) (Bitwise NOT of A)
```

Copy

### e. AND (&): #AND
 
Performs a bitwise AND operation. The result is 1 only if both corresponding bits are 1.

**Example:**

```
A = 1010 (10)
B = 1100 (12)
A & B = 1000 (8) (Bitwise AND of A and B)
```

Copy

### f. OR (|): #OR

Performs a bitwise OR operation. The result is 1 if at least one of the corresponding bits is 1.

**Example:**

```
A = 1010 (10)
B = 1100 (12)
A | B = 1110 (14) (Bitwise OR of A and B)
```



### g. #Subtraction (-):

In binary arithmetic, subtraction can be represented using the concept of two's complement. The two's complement is a way of representing signed integers in binary. The steps involved in finding the two's complement are:

1. **Take the bitwise complement (NOT) of the subtrahend.**
2. **Add 1 to the result obtained in step 1.**

Let's represent the subtraction of B from A using two's complement:

### Example:

```
A = 1010 (10)
B = 1100 (12)

1. Take the bitwise complement (NOT) of B:
   ~B = 0011 (3)

2. Add 1 to the bitwise complement:
   ~B + 1 = 0100 (4)

Now, A - B is equivalent to A + (~B + 1):

A + (~B + 1) = 1010 + (0100) = 1010 + 0100 = 1110 (14)
```

### Operation ( x - 1 ) and Flipping Bits

The operation ( x - 1 ) in binary arithmetic has a specific effect: it flips all the bits to the right of the least significant set (1) bit in ( x ). Here's how it works:

1. **Identify the Least Significant Set Bit (LSB):**
    
    - Find the rightmost bit that is set (1) in ( x ).
    
    Example:
    
    ```
    x = 1010100
    LSB is quoted: 1010`1`00
    ```
    
    Copy
    
2. **Flip Bits to the Right of LSB:**
    
    - Flip (change 0s to 1s and 1s to 0s) all the bits to the right of the LSB (set) , including the LSB itself.
    
    Example:
    
    ```
    x - 1 = 1010100 - 1 = 1010011 (83)
    ```
    
    Copy
    
    The bits to the right of the LSB (including the LSB) have been flipped.
    

This property is often used in bitwise operations to clear the rightmost set bit in a number or to find the next lower number that is a power of two.

---

#### Let's explore some important method which we use again and again:

### `set(i)`, `unset(i)`, `check(i)`, `flip(i)`, and `count()` in the context of #bitwise_operations.

### a. `set(i)`:

The operation `set(i)` is used to set the ith bit (counting from the right, starting with 0) in a binary number. It involves turning the ith bit from 0 to 1 while leaving other bits unchanged.

**Example:**

```
x = 10001 (17)
set(2) of x: 10001 | (1 << 2) = 10101 (21)
```

Copy

In this example, setting the 2nd bit of `x` results in `10101`.

### b. `unset(i)`:

The operation `unset(i)` is used to unset (clear) the ith bit in a binary number. It involves turning the ith bit from 1 to 0 while leaving other bits unchanged.

**Example:**

```
x = 1011100 (92)
unset(3) of x: 1011100 & ~(1 << 3) = 1010100 (84)
```

Copy

In this example, unsetting the 3rd bit of `x` results in `1010100`.

### c. `check(i)`:

The operation `check(i)` is used to check the value of the ith bit in a binary number. It returns true if the ith bit is set (1) and false if it is unset (0).

**Example:**

```
x = 1010100 (84)
check(4) of x: (x & (1 << 4)) != 0  // true
```

Copy

In this example, checking the 4th bit of `x` returns true because the 4th bit is set.

### d. `flip(i)`:

The operation `flip(i)` is used to toggle the value of the ith bit in a binary number. It changes a set bit to unset and vice versa while leaving other bits unchanged.

**Example:**

```
x = 1010100 (84)
flip(1) of x: 1010100 ^ (1 << 1) = 1010101 (85)
```

Copy

In this example, flipping the 1st bit of `x` results in `1010101`.

### e. `count()`:

The operation `count()` is used to count the number of set bits (1s) in a binary number. It involves iterating through each bit and counting the ones.

**Example:**

```cpp
x = 1010100 (84)
count() of x: Count the set bits in 1010100 (84) = 3
```
---



# Bitset STl #bitset_stl

**Operations on Bitset:**

1. **Construction**:
    
    - Creating a bitset object specifying its size.
    
    ```cpp
    bitset<8> bits; // Creates a bitset with 8 bits, all initialized to 0.
    ```
    
    
2. **Setting and Resetting Bits**: #set_bit #reset_bit
    
    - Setting a specific bit to 1.
    - Resetting a specific bit to 0.
    
    ```cpp
    bits.set(2);    // Sets the bit at position 2 to 1.
    bits.reset(5);  // Resets the bit at position 5 to 0.
    ```
    
    
3. **Accessing Bits**:
    
    - Accessing the value of a specific bit.
    
    ```cpp
    bool bitValue = bits[3]; // Retrieves the value of the bit at position 3.
    ```
    
4. **Flipping Bits**: #flip_bit
    
    - Toggling the value of a specific bit.
    
    ```cpp
    bits.flip(1); // Toggles the value of the bit at position 1.
    ```
    
5. **Counting Set Bits**: #count_bit
    
    - Counting the number of set bits in the bitset.
    
    ```cpp
    size_t count = bits.count(); // Counts the number of set bits in the bitset.
    ```
    
    
1. **Bitwise Operations**:
    
    - Performing bitwise AND, OR, XOR operations with other bitsets.
    
    ```cpp
    bitset<8> other(0b11001010);
    bits |= other;  // Bitwise OR operation with another bitset.
    bits &= other;  // Bitwise AND operation with another bitset.
    bits ^= other;  // Bitwise XOR operation with another bitset.
    ```
    
    
2. **Conversion to Integer**:
    
    - Converting the bitset to an integer value.
    
    ```cpp
    unsigned long intValue = bits.to_ulong(); // Converts bitset to unsigned long.
    ```
    
    
3. **String Representation**:
    
    - Converting the bitset to a string.
    
    ```cpp
string bitString = bits.to_string(); // Converts bitset to string representation.
    ```
---


## #BIT_STL

1. **`__builtin_popcount()`**: Counts the number of 1s in the binary representation of a number. #popcount
```cpp
    int n = 29; // Binary: 11101 (3 bits set to 1)
    cout << "__builtin_popcount: " << __builtin_popcount(n) << endl; 
    // Output: 4
```



2. **`__builtin_clz()`**: Counts leading zeros in the binary representation of a number (assuming a 32-bit integer).
```cpp
int n = 16; // Binary: 00000000000000000000000000010000 
cout << "__builtin_clz: " << __builtin_clz(n) << endl; // Output: 27 (leading zeros before the first 1)
```



 3. **`__builtin_ctz()`**: Counts trailing zeros in the binary representation of a number.
```cpp
int n = 40; // Binary: 00000000000000000000000000101000 
cout << "__builtin_ctz: " << __builtin_ctz(n) << endl; // Output: 3 (trailing zeros)
```



4. **`__builtin_parity()`**: Returns 1 if the number of set bits is odd, else 0.

```cpp
int n = 5; // Binary: 101 (even number of 1s) 
cout << "__builtin_parity: " << __builtin_parity(n) << endl; // Output: 0 (even parity)
```

5. In C++, the expression `(int)__lg(x)` uses the built-in function `__lg(x)` to calculate the **largest power of 2** that is less than or equal to `x`, effectively giving you the position of the most significant set bit (or the highest bit with value 1) in the binary representation of `x`. This is equivalent to the floor of the base-2 logarithm of `x`.

	For example:
	
	- If `x = 16` (binary `10000`), `__lg(16)` returns 4.
	- If `x = 17` (binary `10001`), `__lg(17)` returns 4 as well.
	
	It is useful for quick bitwise calculations.

---

### Isolating the LSB

To isolate the LSB, use `x & -x`, which returns the value of the least significant set bit (i.e., the bit with the lowest position that is 1):

`int x = 18; // binary: 10010 
`int lsb = x & -x;  // lsb will be 2 (binary: 00010)`
 `-x` is the same as `~(x - 1)` in two's complement representation

----
### Question:

You are given an array where every element occurs twice, except for two elements which occur only once. Find both the elements which occur only once.

### Example:

1. Input: `[23, 23, 4, 3, 5, 3, 15, 15]` → Output: `4, 5`
2. Input: `[2, 10, 12, 10, 19, 19, 12, 15]` → Output: `2, 15`

### Solution Idea:

1. First, find the XOR of all elements to get `a ^ b`, where `a` and `b` are the two unique elements.
2. Find the rightmost set bit in `a ^ b` to separate elements into two groups.
3. XOR the elements in each group to get `a` and `b`.

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;

void find_two_numbers(vector<int> &arr, int *a, int *b) {
    int n = arr.size();
    *a = 0;
    *b = 0;
    int x = 0;

    // XOR of all elements
    for (int i = 0; i < n; i++) {
        x ^= arr[i];
    }

    // Find the rightmost set bit of XOR result
    int set_bit = x & ~(x - 1);

    // Separate elements into two groups
    for (int i = 0; i < n; i++) {
        if (set_bit & arr[i]) {
            *a = *a ^ arr[i];
        } else {
            *b = *b ^ arr[i];
        }
    }
}
```

  
----
**Question 4** Find the XOR of numbers from 1 to n

**Method 1:** The XOR of numbers from 1 to any number just before multiple of 4 is 0(i.e. 3,7,11).

```cpp
int find_xor_upto_n(int n)
{
    int rem=n%4;
    switch(rem)
    {
        case 0: return n;
        case 1: return 1;
        case 2: return n+1;
        case 3: return 0;
    }
    return 1;
}
```

---
**Question 6** Given a positive integer n, find the count of positive integers i such that 0 <= i <= n and `n+i=n^i.

**Example:**

1. 10 => 4
2. 18 => 8

**Solution Idea:**

**Method 1:** For any integer i if n+i = n^i, then n&i=0. So we will count the number of zero bits in n and calculate how many numbers can be formed using these bits as set bits.

```cpp
int calculate(int n)
{
    int count_unset=0;
    while(n>0)
    {
        if((n&1)==0)    // count number of zero bits
            count_unset++;
        n>>=1;
    }
    return (1<<count_unset);// There can be 2^count numbers
}
```
>[!Hint]
>Because set bits of i for N can't be changed (&), because they are taken 1.
>However unset bits of N has 2 choices of both 0/1 (&), hence 2^count


---
**Question 9** Given an array of integers, find the minimum xor of a pair of elements of the array.

#XOR [[XOR]]

1. {2, 3, 7, 6, 4} => 1
2. {2, 5, 4, 2, 6, 8, 9} => 0

**Solution Idea:**

**Method 1:** Sort the array in increasing order. Then minimum xor will be xor of a_i and a_i+1 for some i. This can be easily proved by contradiction.

```cpp
int minimum_xor(vector<int> &A) {
    int n=A.size();
    int res=INT_MAX;
    sort(A.begin(),A.end());
    for(int i=1;i<n;i++)
    {
        res=min(res,A[i]^A[i-1]);
    }
    return res;
}
```

---

>[!Warning]
>a^(b+c)!=a^b+a^c;

[[XOR]] [[00_DSA]]

----
# #Bit_SUM

>[!Note]
 Bit expressions are sum independent on each bit

 **Problem Statement:** You are given array a containing n elements. You have to find ΣΣ( a[i]^a[j] ).
 ![[Pasted image 20241012234946.png]]
**Code:**

```cpp
int n, int arr[n], int ans=0;

for(int j=0; j<31; j++) //iterating over bits
{
    int cnt_0=0, cnt_1=0;

    for(int i=0; i<n; i++)
    {
        if(arr[i]&(1<<j)) //checking if its 1 or 0
        {
            cnt_1++;
        }
        else
        {
            cnt_0++;
        }
    }

    int pairs = cnt_0 * cnt_1;  //this is the total no.of ^pairs that actually contribute to the final answer

    ans += pairs * (1<<j); //(1<<j) is done bcz check above image
}
cout << ans;
```
It's important to note that `(1<<j)` is a bitwise left shift operation, which effectively represents 2^j. This is used to calculate the contribution of the pairs at the `j`-th bit position.

**Time Complexity:**

1. **Outer Loop:**
    
    - The outer loop runs for a constant number of iterations (31 in this case), as it iterates over each bit position.
    - Time complexity: O(1)
2. **Inner Loop:**
    
    - The inner loop iterates over each element in the array, performing constant-time operations for each iteration.
    - The bitwise operations inside the inner loop (`arr[i] & (1<<j)`) take constant time.
    - Time complexity: O(n)

Since the outer loop is constant time and the inner loop is O(n), the overall time complexity of the algorithm is O(1) * O(n), which simplifies to O(n).

 ---
 **Problem Statement:** Given an _array_ of _N_ positive integers. You can perform this operation any number of times, choose two indices x and y. If array[x] = a and array[y] = b, then after the operation

1. array[x] = a OR b, array[y] = a AND b.

Perform the operations optimally such that ∑array[i]*array[i] for all 1<=i<=n is maximum. Print the largest sum of squares you can get after performing the operations greater than equal to zero times.

![[Pasted image 20241013095355.png]]

```cpp
void solve(){
    int n;
    cin >> n;
    int arr[n];
    int cnt[21]; //keeps the count of no.of 1s at each bit position
    memset(cnt, 0, sizeof(cnt)); 
    
    for(int i = 0; i < n; i++){
        cin >> arr[i];
        for(int j = 0; j <= 20; j++){
            if(arr[i] & (1 << j))  //check bit is 1 or not
                cnt[j]++;
        }
    }
    
    int ans = 0;
    for(int i = 0; i < n; i++){
        int largepos = 0;
        for(int j = 0; j <= 20; j++){
            if(cnt[j]){
                largepos |= (1 << j); //IMP* creating the max number
                cnt[j]--;
            }
        }
        ans+=largepos*largepos;
    }
    cout<<ans<<endl
}

```