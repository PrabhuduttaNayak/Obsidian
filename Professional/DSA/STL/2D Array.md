21/09/2024 12:09

Tags: #2D_Array

Status:

# 2D Array


## Different ways to create a 2-D array

### Method 1:

The first method is a straightforward approach. You can create a 2-D array with a fixed size using the following syntax:

```cpp
int arr[n][m];
```


### Method 2:

The second method involves initializing the array with data. You can create a 2-D array and initialize its values using the following example:

```cpp
int arr[][4] =  {{ 312, 247, 311, 193 },
                 { 311, 242, 738, 917 },
                 { 941, 252, 549, 595 }};
```


### Method 3: #declaring_pointer_array

The third method involves dynamic allocation of memory for the 2-D array. You can create a 2-D array with dynamic memory allocation using the following syntax:

```cpp
int **arr;
arr = new int *[n];

for(int i = 0 ; i<n ;i++){
    arr[i] = new int [m];
}
```
**`n` rows** **`m` columns** in each row

### Method 4:

The fourth method is similar to the third method, but instead of using a pointer to a pointer, we use an array of pointers. You can create a 2-D array with dynamic memory allocation using the syntax:

```cpp
int *arr[n];

for(int i = 0 ; i< n ;i++){
  arr[i] = new int [m];
}
```

## Passing a 2-D array to a function in C++

In C++, passing a 2-D array to a function can be done in multiple ways. Here are a few examples:

```cpp
const int n = 5;
const int m = 4;

void func_name(int a[N][M]) {
  // your code 
}

void func_name(int a[][5]) {
  // your code 
}

void func_name(int *a[5]) {
  // your code 
}

void func_name(int **a) {
  // your code 
}
```


### References