11/10/2024 11:29

Tags:

Status:

# #Binary_Search_REAL_domain


### Using #epsilon (precision threshold)

`double epsilon = 1e-6;`

```cpp
#include <iostream>
#include <cmath>

using namespace std;

double function(double x) {
    // Define your function here (e.g., f(x) = x^2)
    return x * x;
}

double binarySearch(double target, double low, double high, double epsilon) {
    while ((high - low) > epsilon) {
        double mid = (low + high) / 2;
        double f_mid = function(mid);

        if (abs(f_mid - target) < epsilon) {
            return mid; // Close enough to the target
        }

        if (f_mid < target) {
            low = mid;  // Search in the right half
        } else {
            high = mid; // Search in the left half
        }
    }

    return (low + high) / 2; // Best approximation
}

int main() {
    double target = 9.0;     // Example: find sqrt(9)
    double low = 0.0, high = 10.0;
    double epsilon = 1e-6;   // Precision threshold

    double result = binarySearch(target, low, high, epsilon);
    cout << "Approximate result: " << result << std::endl;

    return 0;
}

```

### using number of Iterations

```cpp
#include <iostream>
#include <cmath>

using namespace std;

double function(double x) {
    // Define your function (e.g., f(x) = x^2)
    return x * x;
}

double binarySearch(double target, double low, double high, int iterations) {
    double mid;
    
    for (int i = 0; i < iterations; ++i) {
        mid = (low + high) / 2;
        double f_mid = function(mid);

        if (f_mid < target) {
            low = mid;  // Search in the right half
        } else {
            high = mid; // Search in the left half
        }
    }
    
    return mid; // Approximation after specified iterations
}

int main() {
    double target, low, high;
    int iterations;

    cout << "Enter target value (e.g., sqrt of 9 = 9): ";
    cin >> target;
    cout << "Enter the lower bound of the search interval: ";
    cin >> low;
    cout << "Enter the upper bound of the search interval: ";
    cin >> high;
    cout << "Enter the number of iterations: ";
    cin >> iterations;

    double result = binarySearch(target, low, high, iterations);
    cout << "Approximate result after " << iterations << " iterations: " << result << endl;

    return 0;
}

```



### Examples