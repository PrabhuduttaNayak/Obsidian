21/09/2024 15:13

Tags: #XOR

Status:

# XOR

#### Odd One Out AZ101

##### Description
XOR is like adding binary elements without keeping Carry or adding and then taking %2
It is an odd number of 1 Detector Algorithm

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

>[!Warning]
>a^(b+c)!=a^b+a^c;


### Examples
