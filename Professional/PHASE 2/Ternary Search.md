2024-10-06 09:03

Tags: #Ternary_search

Status: [[Binary Search]] [[00_DSA]]

# Ternary Search

## [Binary search Vs Ternary Search:](https://www.geeksforgeeks.org/binary-search-preferred-ternary-search/)

The time complexity of the binary search is less than the ternary search as the number of comparisons in ternary search is much more than binary search. Binary Search is used to find the maxima/minima of [monotonic functions](https://www.geeksforgeeks.org/binary-search-intuition-and-predicate-functions/) where as Ternary Search is used to find the maxima/minima of [unimodal functions](https://www.geeksforgeeks.org/mathematics-unimodal-functions-bimodal-functions/).![[Pasted image 20241006100524.png]]

## Working of Ternary Search:

The concept involves dividing the array into ****three equal segments**** and determining in which segment the ****key**** element is located. It works similarly to a binary search, with the distinction of reducing time complexity by dividing the array into three parts instead of two.

Below are the step-by-step explanation of working of Ternary Search: [[Recursion]]

1. ****Initialization:****
    - Set two pointers, ****left**** and ****right****, initially pointing to the first and last elements of our search space.
2. ****Divide the search space:****
    - Calculate two midpoints, ****mid1**** and ****mid2****, dividing the current search space into three roughly equal parts:
    - mid1 = left + (right – left) / 3
    - mid2 = right – (right – left) / 3
    - The array is now effectively divided into ****[left, mid1]****, ****(mid1, mid2****), and ****[mid2, right]****.
3. ****Comparison with Target:****.
    - If the ****target**** is equal to the element at ****mid1**** or ****mid2****, the search is successful, and the index is returned
    - If the ****target**** is less than the element at ****mid1****, update the ****right**** pointer to ****mid1 – 1****.
    - If the ****target**** is greater than the element at ****mid2****, update the ****left**** pointer to ****mid2 + 1****.
    - If the ****target**** is between the elements at ****mid1**** and ****mid2****, update the ****left**** pointer to ****mid1 + 1**** and the ****right**** pointer to ****mid2 –**** 1.
4. ****Repeat or Conclude********:****
    - Repeat the process with the reduced search space until the ****target**** is found or the search space becomes empty.
    - If the search space is empty and the ****target**** is not found, return a value indicating that the ****target**** is not present in the arra

```cpp
// C++ program to illustrate
// recursive approach to ternary search
#include <bits/stdc++.h>
using namespace std;

// Function to perform Ternary Search
int ternarySearch(int l, int r, int key, int ar[])
{
    if (r >= l) {

        // Find the mid1 and mid2
        int mid1 = l + (r - l) / 3;
        int mid2 = r - (r - l) / 3;

        // Check if key is present at any mid
        if (ar[mid1] == key) {
            return mid1;
        }
        if (ar[mid2] == key) {
            return mid2;
        }

        // Since key is not present at mid,
        // check in which region it is present
        // then repeat the Search operation
        // in that region
        if (key < ar[mid1]) {

            // The key lies in between l and mid1
            return ternarySearch(l, mid1 - 1, key, ar);
        }
        else if (key > ar[mid2]) {

            // The key lies in between mid2 and r
            return ternarySearch(mid2 + 1, r, key, ar);
        }
        else {

            // The key lies in between mid1 and mid2
            return ternarySearch(mid1 + 1, mid2 - 1, key, ar);
        }
    }

    // Key not found
    return -1;
}

// Driver code
int main()
{
    int l, r, p, key;

    // Get the array
    // Sort the array if not sorted
    int ar[] = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

    // Starting index
    l = 0;

    // end element index
    r = 9;

    // Checking for 5

    // Key to be searched in the array
    key = 5;

    // Search the key using ternarySearch
    p = ternarySearch(l, r, key, ar);

    // Print the result
    cout << "Index of " << key
         << " is " << p << endl;

    // Checking for 50

    // Key to be searched in the array
    key = 50;

    // Search the key using ternarySearch
    p = ternarySearch(l, r, key, ar);

    // Print the result
    cout << "Index of " << key
         << " is " << p << endl;
}
```

### ****Iterative Approach of Ternary Search****:

```cpp
// C++ program to illustrate
// recursive approach to ternary search
#include <bits/stdc++.h>
using namespace std;

// Function to perform Ternary Search
int ternarySearch(int l, int r, int key, int ar[])
{
    if (r >= l) {

        // Find the mid1 and mid2
        int mid1 = l + (r - l) / 3;
        int mid2 = r - (r - l) / 3;

        // Check if key is present at any mid
        if (ar[mid1] == key) {
            return mid1;
        }
        if (ar[mid2] == key) {
            return mid2;
        }

        // Since key is not present at mid,
        // check in which region it is present
        // then repeat the Search operation
        // in that region
        if (key < ar[mid1]) {

            // The key lies in between l and mid1
            return ternarySearch(l, mid1 - 1, key, ar);
        }
        else if (key > ar[mid2]) {

            // The key lies in between mid2 and r
            return ternarySearch(mid2 + 1, r, key, ar);
        }
        else {

            // The key lies in between mid1 and mid2
            return ternarySearch(mid1 + 1, mid2 - 1, key, ar);
        }
    }

    // Key not found
    return -1;
}

// Driver code
int main()
{
    int l, r, p, key;

    // Get the array
    // Sort the array if not sorted
    int ar[] = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

    // Starting index
    l = 0;

    // end element index
    r = 9;

    // Checking for 5

    // Key to be searched in the array
    key = 5;

    // Search the key using ternarySearch
    p = ternarySearch(l, r, key, ar);

    // Print the result
    cout << "Index of " << key
         << " is " << p << endl;

    // Checking for 50

    // Key to be searched in the array
    key = 50;

    // Search the key using ternarySearch
    p = ternarySearch(l, r, key, ar);

    // Print the result
    cout << "Index of " << key
         << " is " << p << endl;
}

// This code is contributed
// by Akanksha_Rai

```

****Time Complexity:****  [[Time Complexity]]

- Worst case: O(log3N)
- Average case: Θ(log3N)
- Best case: Ω(1)
## ****Advantages:****

- Ternary search can find maxima/minima for unimodal functions, where binary search is not applicable.
- Ternary Search has a time complexity of O(2 * log3n), which is more efficient than linear search and comparable to binary search.
- Fits well with optimization problems.

## ****Disadvantages:****

- Ternary Search is only applicable to ordered lists or arrays, and cannot be used on unordered or non-linear data sets.
- Ternary Search takes more time to find maxima/minima of monotonic functions as compared to Binary Search.