21/09/2024 15:13

Tags: #XOR

Status:

# XOR

#### Odd One Out AZ101

##### Description

You are given an array of N integers. The frequency of exactly one integer is odd. Find that integer.

```cpp
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            int x;
            cin >> x;
            ans ^= x;
        }
        cout << ans << "\n";
```



### Examples
